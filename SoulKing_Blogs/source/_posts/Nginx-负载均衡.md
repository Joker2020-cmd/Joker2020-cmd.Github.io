---
title: Nginx负载均衡
date: 2023-09-12 15:49:58
tags:
   - Nginx
categories:
   - Nginx搭建
keywords: "Nginx基础学习"
abbrlink: ""
decription:
top_img:
cover: https://s2.loli.net/2023/09/12/9OemvhVWbf785rQ.png
---

## **Nginx（负载均衡配置）**

### 1.**在Nginx中配置负载均衡可以通过以下步骤进行**

1. #### **安装Nginx**

   首先，确保已经在服务器上安装了Nginx。可以使用适合你的Linux发行版本的包管理器来安装Nginx。

2. #### **创建上游服务器列表**

   在Nginx配置文件中，定义一个上游服务器列表，即后端应用服务器的地址和端口。可以在nginx.conf或者sites-available目录中配置文件中进行配置。例如：

   ```nginx
   upstream backend {
       server backend1.exaple.com:8000;
       server backend2.exaple.com:8000;
       server backend3.exaple.com:8000;
       ........
       
   }
   ```

   在此示例中，我们定义了一个名为"backend"的上游服务器列表，并指定了一个后端应用服务器的地址和端口。

3. #### **配置负载均衡策略**

   在Nginx配置文件中，配置负载均衡策略。可以使用不同的负载均衡算法，如轮询（默认），IP哈希，最少连接等。例如：

   ```nginx
   http {
       upstream backend {
           server backend1.example.com:8000;
           server backend2.example.com:8000;
           server backend3.example.com:8000;
           server backend4.example.com:8000;
           .......
       }
       
       server {
           listen 80,
               
           localhost / {
              proxy_pass http://backend;
              proxy_set_header Host $host;
              proxy_set_header X-Real-IP $remote_addr;
           }
       }
   }
   ```

   在此示例中，我们在localhost块中使用了proxy_pass指令来将请求发送到上游服务器列表"backend"。还使用了proxy_set_header指令来设置一些HTTP头信息。

4. #### **重启Nginx**

   完成上述配置后，保存配置文件并重启Nginx服务，使其生效。可以使用以下命令重启Nginx：

   ```shell
   sudo service nginx restart
   ```

   或者，如果使用的是Systemd系统，可以使用以下命令：

   ```shell
   sudo systemctl restart nginx
   ```

   现在，Nginx将根据你的负载均衡配置请求分发到上游服务器列表中的后端应用服务器。你可以根据实际需求进行负载均衡配置，包括添加更多的后端服务器、调整负载均衡算法等。

### **实验：搭建三台服务器，其中2台tomcat服务器、1台Nginx服务器，并用Nginx代理2台tomcat服务器实现负载均衡。**

### **环境准备**

1. 使用VMware虚拟机安装三台linux的服务器系统,分别2台centos 6,1台Red Hat. 
2. Nginx压缩包,tomcat压缩包,JDK安装包,gcc\gcc-c++安装
3. Mobaxterm远程终端软件

### **开始搭建服务器:**

#### tomcat安装(压缩包名-apache-tomcat-7.0.57.tar.gz):

```shell
tar -xvf apache-tomcat-7.0.57.tar.gz #解压
```

或`

```shell
tar -xvzf apache-tomcat-7.0.57.tar.gz #解压
```

#### JDK安装(安装包名-jdk-7-linux-i586.rpm)

```shell
 rpm -ivh jdk-7-linux-i586.rpm --nodeps --force
```

#### Nginx安装(压缩包名-nginx-1.13.9.tar.gz)

```shell
tar -xvf nginx-1.13.9.tar.gz 
```

或

```shell
tar -xvzf nginx-1.13.9.tar.gz
```

#### gcc\gcc-c++安装

```
rpm -ivh /gcc *.rpm
```

```
rpm -ivh /gcc-c++ *.rpm
```

**注意**

关闭防火墙:

```shell
sudo iptables -F
```

或

```shell
service stop firewalld
```

或

```shell
systemctl iptables stop
```

2台tomcat服务器分别进入各自的启动目录下.执行启动文件

linux:

双击startup.sh

前者启动无效后,先进入启动目录使用命令:

```shell
./startup.sh
```

windows:

双击startup.bat

前者启动无效后,进入启动目录使用命令:

```shell
startup
```

启动后,访问主机地址+端口号,看看是否能访问出tomcat主页.

然后如上述更改Nginx的配置文件即可.

Nginx服务器启动,进入Nginx的bin目录,执行命令:

```shell
./nginx
```

Nginx的重新启动命令:

```
./nginx -s reload
```

### 命令全部通过Mobaxterm远程终端软件控制台执行.

### **扩展:**

安装一个文件夹下的所有RPM文件,你可以使用以下命令来批量安装:

```shell
rpm -ivh /path/to/folder/*.rpm
```

其中, /path/to/folder是包含所有RPM文件的文件夹路径.

这个命令使用通配符*.rpm来匹配并安装文件夹的所有RPM文件.

-i选项表示安装软件包.

-v选项表示显示详细的安装过程.

-h选项表示以人类可读的方式显示进度信息.

请注意,这个命令需要管理员权限来执行安装操作.

VIM是一个流行的文本编辑器,经常用于在终端中编辑文件,你可以通过以下命令启动vim:

```shell
vim filename
```

其中,  filename是要编辑的文件的名称,如果文件不存在,vim将会创建一个新的文件.

在vim中,你可以使用各种命令来编辑文件,以下是一些常用的vim命令:

- i 进入插入模式,允许你在文件中插入和编辑文本.
- Esc退出插入模式,返回到正常模式.
- :w保存文件.
- :q退出vim.
- :wq保存文件并退出vim.
- :q!强制退出vim,丢弃所有修改.
- /pattern在文件中搜索指定的模式.
- n在搜索结果中向下移动到下一个匹配项.
- N在搜索结果中项上移动到上一个匹配项.

对于tomcat服务器启动端口占用问题:

在conf文件中找到tomcat服务器的配置文件-server.xml,

重新配置端口:

```xml
# 未改动前,默认
<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443">
# 更改后(修改port值为未占用的端口号即可)
<Connector port="8007" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443">
```

