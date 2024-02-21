---
title: 进行JMeter分布式测试遇到的坑
date: 2023-09-21 14:47:13
tags:
   - 分布式测试
categories:
   - JMeter分布式测试学习 
cover: https://s2.loli.net/2023/08/30/LkrjWplvHyc1nDm.png
---

## **进行JMeter分布式测试遇到的坑**

自己在使用JMeter进行分布式测试的时候，遇到了很多的坑。下面总结起来，方便以后查看。

## 1.控制机端

1. **执行机没有关闭防火墙**

   在执行机IP+端口号设置正确的情况下，控制机在启动测试计划的时候，出现如下情况：

   windows系统，下的GUI界面：

   {%asset_img 48-21-23-144816.png%}

   Linux系统下，出现：

   {%asset_img 48-21-23-144820.png%}

   如上情况说明，无法找到执行机与之进行连接，最先应想的就是执行机中的防火墙是不是没有关闭，我关闭执行机的防火墙后，以上错误消失。

   CentOS7中关闭防火墙如下：

   1. 查看防火墙状态命令：systemctl status firewalld.service

      {%asset_img 48-21-23-144824.png%}

      active(running):说明防火墙是开启状态。

   2. 关闭运行的防火墙使用命令：systemctl stop firewalld.service

      {%asset_img 48-21-23-144828.png%}

      inactive(dead):说明防火墙已经是关闭状态。

   3. 前面的方法，一旦重启操作系统，防火墙就自动开启了，该怎么设置才能永久关闭防火墙呢？

      - systemctl disable firewalld.service,开机禁止防火墙服务器。
      - systemctl enable firewalld.service,开机启动防火墙服务器。

      提示：开启防火墙命令systemctl statr firewalld.service

   4. **内存溢出**

      在执行JMeter命令时，提示如下信息。

      windows系统，提示如下：

      Error occurred during initialization  of  VM Could not  reserve enough space  for  object heap errorleve1=1

      Linux系统，提示如下：

      {%asset_img 48-21-23-144833.png%}

      提示你，虚拟机初始化期间无法为HotSpot保留足够的空间。

      解决办法：打开jmeter.bat文件，查找set HEAP，把set HEAP=-Xms128m -Xmx512m修改为set HEAP=-Xms512m-Xmx1024m。

      （看实际需求往大调整）

## 2.执行机端

1. **启动JMeter-server服务情况1**

   出现如下所示：

   {%asset_img 48-21-23-144837.png%}

   **服务器启动异常**：Java运程方法调用抛出异常：无法启动服务。localhost.localdomain是一个本地回环地址。

   **原因**：

   要在远程对象上调用方法，RMI客户端首先必须从RMI注册表中检索远程存根对象。此存根对象包含服务器地址，该服务器地址稍后将在调用远程方法时用于连接到远程对象（与RMI注册表的连接和与远程对象的连接是两个完全不同的东西）。默认情况下，服务器将尝试检测自己的地址并将其传递给存根对象。不幸的是，用于检测服务器地址的算法并不总是产生有用的结果（取决于网络配置）。

   通过设置RMI服务器上的系统属性Java.rmi.server.hostname,可以覆盖传递给存根对象的服务器地址。

   默认情况下，RMI注册表使用端口1099.

   解决方案：

   **方法1：**

   运行JMeter命令启动jmeter-server服务时，增加如下参数：

   **./jmeter-server  -Djava.rmi.server.hostname=(执行机-本机IP)**

   **方法2：**

   修改jmeter-server文件：

   ```properties
   #修改RMI_HOST_DEF如下
   RMI_HOST_DEF=-Djava.rmi.server.hostname=192.16.*.*(本机ip)
   ```

   之后执行命令./jmeter-server,来启动jmeter-server服务即可。（可方法一同理）

   补充：

   Linux系统中后台执行，启用jmeter-server服务：

   nohup  ./jmeter-server  -Djava.rmi.server.hostname=192.168.x.x

   查看jmeter-server服务是否启动成功，执行命令：ps axu | grep  jmeter

   **方法3：**

   也有可能是hostname与localhost不一致导致的。

   当在/etc/hosts文件中进行映射时，通过localhost无法映射到一个有效的IP地址，就会出现上述问题。

   操作步骤：

   获取机器名称，增加一条域名与IP的映射。

   1. **先在/etc/hosts里添加一行，如下所示：**

      机器的实际IP地址为：192.168.134.130

      {%asset_img 48-21-23-144845.png%}

   2. **然后修改/etc/sysconfig/network文件里面的HOSTNAME。**

      {%asset_img 48-21-23-144848.png%}

      则可以访问成功。

      （以上三种方式都可以试试，其实是一类问题）

   3. **启动jmeter-server服务情况二**

      出现如下图所示：

      {%asset_img 48-21-23-144852.png%}

      提示你，找不到系统指定的文件。

      原因：要么拥有RMI over SSL的有效密钥库，要么禁用掉SSL。

      解决方案禁用掉SSL：修改bin/jmeter.properties配置文件，server.rmi.ssl.disable=true即可解决。

      提示：master和slave机器上jmeter.properties配置文件中，server.rmi.ssl.disable都改成true。

   4. **启动jmeter-server服务情况三**

      执行机启动jmeter-server服务，提示"Could notfind ApacheJMeter_core.jar"

      原因：是因为JMeter没有配置环境变量。

## 3.**分布式测试控制机接收不到返回结果**

即：也就是关于控制机的多网卡问题。

如果控制机（master）只有一个网卡，不需要关注这个问题。

如果我们在练习的时候，只有一台电脑，需要启动虚拟机完成演示的时候，就需要注意这里的问题了。

1. **多网卡出现的问题说明**

   JMeter采取了RMI进行远程调用，如果控制机（master）有多个网卡，它只是使用其中任意一个网卡。

   默认情况下，他会使用无线局域网适配器这块网卡，而虚拟机使用的网卡是以太网适配器VIware  Network Adapter VMnet8。

   如下图所示：

   {%asset_img 48-21-23-144857.png%}

   这就导致JMeter的控制机（master）和执行机（slave）不在同一网段内，会导致无法互通，出现的情况可能是控制机无法连接执行机，或者是执行机中的测试结果无法回传给控制机。

2. **多网卡出现的问题解决方式**

   我的测试机情况如下：

   一台windows系统主机作为控制机，IP地址如上图中所示。

   在该主机上启动了三台虚拟机，IP地址分别为192.168.134.129、192.168.134.130、192.168.134.131。

   1. **执行机中的配置**

      修改jmeter-server文件

      RMI_HOST_DEF=-Djava.rmi.server.hostname=xxx.xxx.xx.xx(执行机的IP)

      当然也可以不配置，在执行机启动jmeter-server服务的时候，执行如下命令，也是一样的。

      ./jmeter-server -Djava.rmi.server.hostname=xxx.xx.xx.xx

   2. **控制机中的配置**

      修改jmeter.bat启动文件（我是windows系统主机作为控制机）

      在文件中新增如下内容：

      ```properties
      set rmi_host=-Djava.rmi.server.hostname=192.168.134.100
      #可添加，可修改
      set ARGS=%DUMP% %HEAP% %NEW% %SURVIVOR% %TENURING% %PERM% %DDRAW% %rmi_host%
      ```

      如下图所示：

      {%asset_img 49-21-23-144901.png%}

      这样就把控制机的IP网段从192.168.1.101换成了192.168.134.100

      **提示：**

      - 设置网段的IP不要和虚拟机中的IP冲突。
      - 这里的配置是基于上一篇文章【JMeter分布式测试】中的配置。
      - 如果不需要进行分布式了，不要忘记把JMeter.bat启动文件修改回来，防止做其他工作时有影响。

      这样就设置后，windows系统主机就可以控制三个Linux虚拟机分布式执行脚本了。

## 4.**参数化问题**

1. **参数化问题说明**

   - 通常，我们编写、调试脚本都是在windows机器上，而真正性能测试时，脚本几乎都在Linux下运行。
   - 使用CSV数据文件做参数化时，是需要指定文件路径的。
   - 这里就有个问题：window下写的文件路径到了Linux下是不正确的，导致无法正常读取CSV文件。
   - 为了解决这个问题，下面将要讲解一个简单的万能解决方法。

2. **两个前提**

   - 我们的CSV文件必须在JMeter的bin目录下创建，然后再添加自己要的数据。
   - Jmeter命令必须从bin目录下启动。

3. **操作方式**

   CSV数据文件设置组件中，直接按下面的格式写：

   {%asset_img 49-21-23-144907.png%}

   这样就可以了，只要把CSV文件上传到Linux系统JMeter下的bin目录，这个脚本就可以跨平台执行了。

4. **总结**

   - ${_ _p(user.dir,)} ${_ _p(file.separator,)}test.txt可以根据不同的系统，不同的JMeter安装路径。自动获取Jmeter路径，然后再获取不同系统下的文件路径分隔符，最后加上文件名称拼成文件路径。
   - 这样就可以解决使用CSV数据文件做参数化时，跨平台导致路径不一致的问题。
   - 重点前提：CSV文件放在JMeter的bin目录下，且到bin目录中执行JMeter命令。

## 5.**其他补充**

1. **修改端口号**

   JMeter分布式测试的默认端口号是1099，我们可以自定义修改端口号。

   再jmeter.properties配置文件中修改：

   - server_port=1086:表示master机器要远程连接的端口，即：remote_host=xxx:1086.
   - server.rmi.localport=1086:表示slave server启动显示的端口。

   其实上面两者一样，修改改server_port即可。

2. **查看分布式测试过程中**

   在jmeter.properties配置文件中修改mode=Standard。

   - 用于查看分布式测试过程中，每个压力机的测试结果。
   - 若不启用，在运行过程中，控制器是无法实时看到压力机的结果。

   提示：一般不需要开启。
   
   
   
   本篇文章来源于：[博主繁花似锦](https://www.cnblogs.com/liuyuelinfighting/p/14900019.html)
   
   
