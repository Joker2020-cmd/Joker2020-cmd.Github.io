---
title: 服务器部署系统(nginx)
date: 2023-09-13 11:47:07
tags:
   - 服务器部署系统
   - Nginx
categories:
   - Nginx搭建
keywords: "Nginx基础学习"
abbrlink: ""
decription:
top_img:
cover: https://s2.loli.net/2023/09/12/9OemvhVWbf785rQ.png
---

## **服务器部署系统（Nginx负载均衡）**

### 1.**部署步骤**

1.  **上游服务器部署tomcat**
2.  **安装JDK**
3.  **安装Mysql**
4. **上传部署项目**
5.  **代理服务器（安装Nginx）**

### 2.**详细步骤说明**

1.  **上游服务器部署tomcat**

   **以apache-Tomcat-7.0.57.tar.gz为例：**

   --->

   [官网下载]: https://tomcat.apache.org/	"Tomcat下载"

   并将Tomcat压缩包通过MobaXterm上传到Linux服务器,在指令控制台通过命令：

   ```shell
   # 解压文件指令
   tar -xvzf apache-tomcat-7.0.57.tar.gz
   ```

2. **安装JDK（注意：版本要匹配！！！）**

   **以jdk-8u131-linux-i586.rpm为例：**

   --->

   [官网下载]: http://www.codebaoku.com/jdk/jdk-index.html	"JDK下载"

   并将jdk-8u131-linux-i586.rpm软件安装包通过MobaXterm上传到Linux服务器，在指令控制台通过命令：

   ```shell
   # RPM软件安装包安装指令
   rpm -ivh jdk-8u131-linux-i586.rpm
   ```

   或

   ```shell
   # RPM软件安装包安装指令
   rpm -ivh jdk-8u131-linux-i586.rpm --nodeps --force
   ```

   在 RPM 文件安装过程中，`--nodeps` 和 `--force` 是两个常用的选项，它们的作用如下：

   1. `--nodeps`：这个选项告诉 RPM 安装程序忽略依赖关系。通常情况下，RPM 安装程序会检查将要安装的软件包是否依赖于其他软件包，如果依赖关系没有满足，安装将会失败。使用 `--nodeps` 选项可以跳过这个依赖检查，强制安装软件包，即使依赖关系没有满足。但是需要注意，使用 `--nodeps` 可能会导致系统中的软件包依赖关系出现问题。
   2. `--force`：这个选项告诉 RPM 安装程序强制安装软件包，即使已经存在相同名称和版本的软件包。通常情况下，如果已经安装了相同名称和版本的软件包，RPM 安装程序会拒绝安装，并显示错误消息。使用 `--force` 选项可以覆盖已安装的软件包，强制安装新的软件包，即使它们的名称和版本相同。但是需要注意，使用 `--force` 可能会导致系统中的软件包出现冲突或不一致的情况。

3. **安装Mysql**

   **以MySQL-5.5.53-1.linux2.6.i386.rpm-bundle.tar为例：**

   --->

   [官方下载]: https://www.mysql.com/downloads/	"MySQL下载"

   并将MySQL-5.5.53-1.linux2.6.i386.rpm-bundle.tar压缩包通过MobaXterm上传到服务器，在指令控制台通过命令：

   ```shell
   tar -xvzf MySQL-5.5.53-1.linux2.6.i386.rpm-bundle.tar
   ```

   或

   ```shell
   tar -xvf MySQL-5.5.53-1.linux2.6.i386.rpm-bundle.tar
   ```

   首先确认数据库的登陆密码是否是root，不是root则改为root   
   	登陆到数据库执行修改密码命令：

   ```mysql
   grant  all privileges on  *.* to 'root'@'localhost' identified by 'root';
   ```

   ```mysql
   flush privileges;
   ```

   将xxx.sql文件导入到数据库中，不用进入Mysql。

   ```shell
   mysql -uroot -proot < xxx.sql
   ```

   

4. **上传部署项目**

   **以某某系统为例：**

   --->将开发的应用程序，打包成可执行的xx.jar的Java应用程序，将xx.jar的Java应用程序和资源文件通过MobaXterm上传到服务器，在环境配置完成后，在控制台指令通过命令：

   ```shell
   java -jar xx.jar
   ```

   启动Java应用程序

5. **代理服务器（Nginx安装）**

   在一般情况下，安装 Nginx 并不需要安装 `gcc` 和 `gcc-c++`，因为 Nginx 是一个已经编译好的二进制文件，不需要在服务器上进行编译。然而，如果你需要自定义编译 Nginx 或者安装一些 Nginx 的第三方模块，那么可能需要安装 `gcc` 和 `gcc-c++`。

   `gcc` 和 `gcc-c++` 是 GNU Compiler Collection 的一部分，它们提供了编译和链接 C 和 C++ 程序所需的工具和库。在编译 Nginx 或者一些第三方模块时，可能会使用到 C 或 C++ 语言的代码，因此需要安装这两个软件包。

   要安装 `gcc` 和 `gcc-c++`，可以根据你使用的 Linux 发行版执行相应的命令：

   - 在 Ubuntu 或者其他基于 Debian 的发行版上，可以使用以下命令安装：

   - ```shell
     sudo apt-get update sudo apt-get install build-essential
     ```

   - 在 CentOS 或者其他基于 Red Hat 的发行版上，可以使用以下命令安装：

   - ```shell
     sudo yum update sudo yum groupinstall "Development Tools"
     ```

   安装完成后，你就可以使用 `gcc` 和 `g++` 命令进行编译和链接操作了。

   ////////////////////////////////////////////////////////////////////////

   下载gcc和gcc-c++软件安装包，并将gcc和gcc-c++安装包内的所有RPM软件安装文件安装，首先切到gcc和gcc-c++目录下，执行命令：

   ```shell
   rpm -ivh *.rpm --nodeps --force
   ```

   或

   ```shell
   rpm -ivh /home/xx/xx/gcc *.rpm --nodeps --force
   ```

   **以nginx-1.13.9.tar.gz为例**：

   --->

   [官网下载]: https://www.newbe.pro/Mirrors/Mirrors-Nginx/	"Nginx下载"

   ，并将nginx-1.13.9.tar.gz压缩包通过MobaXterm上传到服务器，在指令控制台通过命令：

   ```shell
   tar -xvzf nginx-1.13.9.tar.gz
   ```

   完成nginx解压，进入到目录。

   ​          执行环境检查

   ```shell
    ./configure   --without-http_rewrite_module
   ```

   ​	      编译和安装 

   ```shell
   make   && make install
   ```

   **验证：**
   	进入到/usr/local/nginx/sbin/
   	./nginx   启动

   ```shell
   ./nginx
   ```

   与

   ```shell
   ./nginx -s reload
   ```

   ​	

   打开浏览器，直接访问linuxip地址即可。
   	WELCOME  to  nginx!

   ////////////////////////////////////////////////////////////////

   **设置代理**

   配置nginx的配置文件，找到nginx的安装目录，默认路径/usr/local/nginx/conf/nginx.conf。编辑nginx.conf文件的命令：

   ```shell
   vi nginx.conf
   ```

   进入vim文本查看模式，按键盘A进入可编辑模式，

   在Server上方添加上游服务器列表：

   ```shell
   upstream 上游服务器变量名 {
        server 第一台上游服务器IP地址:访问应用端口号;
        server 第二台上游服务器IP地址:访问应用端口号;
        ip_hash;
        多台服务器，以此类推...
   }
   ```

   Server内部添加代理URL地址：

   ```shell
   localhost / {
         proxy_pass http://上游服务器变量名;
         ......
   }
   ```

   按ESC键，键盘输入:wq, 回车退出并保存。

### 3.**过程出现问题**

1. **上游部署的服务器要关闭防火墙**

   关闭防火墙命令：

   ```shell
   systemctl stop firewalld
   ```

   或

   ```shell
   service iptables stop
   ```

   如果不关闭防火墙，会导致Nginx代理服务器无法访问

2. **JDK进行安装出现Could not .... tool.pack  tool.tar**

   原因1：JDK卸载时，没有卸载干净，切换到jdk-xxx的安装目录下，执行命令：

   ```shell
   rpm -e jdk-xxxx
   ```

   原因2：由于jdk安装包版本太旧，这是jdk安装包自身问题，重新下载个jdk安装包

3. **Nginx的配置文件(/usr/local/nginx/conf)-nginx.conf修改--error：400**

   在 Nginx 配置文件中，变量名不允许出现下划线。这是因为 Nginx 配置文件中的变量命名规则与变量命名规则在其他编程语言中可能不同。

   根据 Nginx 的变量命名规则，变量名只能包含字母、数字和连字符（`-`），并且不能以连字符开头。下划线不在允许的字符范围内，因此不能在变量名中使用下划线。

   如果你在 Nginx 配置文件中使用了下划线作为变量名的一部分，Nginx 将无法正确解析该配置，并可能导致配置文件加载失败或出现其他错误。

   为了解决这个问题，你可以考虑使用连字符（`-`）来替代下划线。如果你需要在变量名中表示多个单词，可以使用驼峰命名法或使用连字符（`-`）来分隔单词。

   例如，如果你想使用一个变量来表示用户的 ID，可以使用类似于 `user_id` 或 `userId` 的命名方式，而不是 `user_id`。这样可以避免在 Nginx 配置文件中出现下划线导致的问题。

   总之，确保在 Nginx 配置文件中的变量名中只使用允许的字符（字母、数字和连字符），并遵循 Nginx 的变量命名规则。

4. **MobaXterm与服务端操作不实时同步**

   如果你在使用 MobaXterm 连接到远程服务器时，发现操作不实时同步，可能是由于远程服务器的终端设置或网络延迟引起的。

   有几种方法可以尝试解决这个问题：

   1. 调整 MobaXterm 的设置：在 MobaXterm 的会话设置中，尝试将 "Terminal-type string" 设置为 "xterm" 或 "xterm-256color"，并确保 "Terminal emulation" 设置为 "Xterm"。这有时可以解决与远程服务器的终端同步问题。
   2. 使用其他终端工具：如果问题仍然存在，可以尝试使用其他 SSH 客户端或终端工具，如 PuTTY、SecureCRT 等，看是否能够实时同步操作。
   3. 检查网络连接和延迟：确保你的网络连接稳定，并检查网络延迟。如果网络延迟较高，可能会导致操作不实时同步。你可以使用 `ping` 命令测试网络延迟，例如 `ping google.com`。
   4. 联系系统管理员：如果问题仍然存在，可能是服务器端的配置或性能问题导致的。联系服务器的系统管理员，寻求帮助并了解是否有相关的配置或调整可以改善同步问题。
   5. 登录服务器，进入配置文件目录查看配置文件是否已经修改的，如果不是，进行修改并保存。

   需要注意的是，以上方法可能不适用于每个情况，具体的解决方法可能因个人情况而异。如果问题持续存在，建议与系统管理员或技术支持人员进行进一步的沟通和协助。

5. **nginx服务器访问系统，页面无法跳转**

   在 Nginx 配置中，`id_hash` 是一种用于负载均衡的配置指令，它用于根据客户端 IP 地址进行哈希算法，以便将请求分发给相同的后端服务器。

   当使用 `id_hash` 配置指令时，Nginx 会计算客户端 IP 地址的哈希值，并将该哈希值与后端服务器列表中的服务器数量取模，以确定将请求发送到哪个后端服务器。这样做的目的是确保相同 IP 地址的请求始终被发送到同一台后端服务器，以保持会话的一致性。

   使用 `id_hash` 配置指令的一个常见应用场景是在负载均衡集群中使用会话保持（session persistence），确保用户的会话数据在同一台服务器上保持一致，避免因为请求被分发到不同的服务器而导致会话丢失或数据不一致的问题。

   以下是一个示例的 Nginx 配置片段，展示了如何使用 `id_hash` 配置指令：

   ```nginx
   http {    
       upstream backend {        
           ip_hash;        
           server backend1.example.com;             server backend2.example.com;             server backend3.example.com;         }     
       server {        
           listen 80;        
           server_name example.com;         location / {            
               proxy_pass http://backend;        }    
       }
   } 
   ```

   在上述配置中，`ip_hash` 指令用于基于客户端 IP 地址进行哈希负载均衡，确保相同 IP 的请求发送到同一台后端服务器。你也可以使用 `id_hash` 替代 `ip_hash`，其效果是一样的。

   需要注意的是，使用哈希负载均衡时，如果后端服务器的数量发生变化，可能会导致哈希算法重新分配请求，这可能会影响会话的一致性。因此，在调整后端服务器数量时，需要谨慎考虑会话保持的问题。
