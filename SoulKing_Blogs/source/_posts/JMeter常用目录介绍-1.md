---
title: 4.JMeter常用目录介绍
date: 2023-09-05 17:31:30
tags:
   - JMeter
categories:
   - JMeter学习 
cover: https://s2.loli.net/2023/08/30/LkrjWplvHyc1nDm.png
---

## JMeter常用目录介绍

在正式使用JMeter之前，建议大家还是先了解JMeter的常见的目录结构。了解一下这些东西以后，才能快速的找到需要的某些配置文件来进行修改。

我们之前下载的是JMeter5.0版本，解压后得到下面的目录结构，如下图：

{%asset_img 33-05-23-173335.png%}

### 1.**bin目录**

bin目录存放的是JMeter的主jar包，启动脚本、配置文件、日志等文件。

{%asset_img 33-05-23-173341.png%}

#### 1.examples目录：

目录中有CSV样例，如下图：

{%asset_img 33-05-23-173348.png%}

JMeter以后在做参数化的时候，就可以用到CSV。（在学习Postman的时候也用过CSV）

#### 2.JMeter.bat:

Windows系统中JMeter的启动文件。

#### 3.JMeter.sh:

Linux系统中JMeter的启动文件。

#### 4.JMeter.log:

JMeter运行的日志文件。在JMeter运行过程中所产生的日志信息都会存储在JMeter.log文件中。

#### 5.JMeter.properties:

系统配置文件。该文件我们会经常的进行一些修改，如我们之前说的修改JMeter默认显示语言等操作。这个文件很重要，一定要记住。

注意：当配置文件修改后，需要重启JMeter才能生效。

#### 6.JMeter-server.bat：

在windows环境下做分布式测试时要用到的服务器配置文件。

#### 7.JMeters-server:

在Linux环境下做分布式测试时要用的服务器配置文件。

**提示：**JMeter.properties系统配置文件中的SSL设置重点关注如下几个配置信息。

```bash
#指定HTTPS协议层
https.default.protocol=TLS

#指定SSL版本
https.default.protocol=SSLv3

#设置启动的协议
https.socket.protocols=SSLv2Hello SSLv3 TLSv1

#缓存控制，控制SSL是否可以在多个迭代中复用
https.use.cached.ssl.context=true
```

### 2.**docs目录**

docs目录为JMeter的接口文档目录。

{%asset_img 33-05-23-173356.png%}

可打开docs目录中api\index.html页面来查看。

因为JMeter是一个开源的工具，如果你需要对JMeter做二次开发，就需要查看这里边的一些接口。

### 3.**extras目录**

extras目录是JMeter的扩展插件目录，该目录属于附加目录。例如进行持续集成时，会将用到的Ant、Maven的插件放在这个目录下面。

{%asset_img 34-05-23-173400.png%}

该目录提供了JMeter对Ant的支持，可以使用Ant来实现自动化测试。例如批量脚本执行，产生html格式的报表。测试运行时，可以把测试数据记录下来，JMeter会自动生成一个.Jtl文件，将该文件放到extras目录下，运行"ant-Dtest=文件名 report"，就可以生成测试统计报表。

总结：该目录平时主要用到的就是JMeter和Ant的集成所需要用到的jar包、build.xml模板、报告模板等文件。

### 4.**lib目录**

该目录是JMeter启动时的默认的classpath目录(JMeter会自动在JMeter_HOME/lib和ext目录下寻找需要的类，lib下存放JMeter所依赖的外部jar)，这就意味着在使用JMeter进行测试的过程中，所有需要引用到的jar包都必须放在该目录下。

{%asset_img 34-05-23-173405.png%}

- lib目录下存放JMeter所依赖的外部插件，这些插件文件均为jar包。

  例如：httpclient.jar、httpcore.jar、httpmime.jar等等。

- 其中lib\ext目录下存放有JMeter依赖的核心jar包，例如:ApacheJMeter_core.jar、ApacheJMeter_java.jar等等。

- lib\junit下存放junit测试脚本。

  提示：

  尤其要注意的就是在扩展JMeter的时候，代码中所有import需要用到的jar包都是存放在lib目录，而不是lib\ext目录下。

### 5.**License目录**

JMeter的证书目录。

### 6.**Printable_docs目录**

该目录存放的是JMeter的官方的帮助文档，唯一的遗憾就是文档是英文的，没有中文版。

说明：printable_docs目录的usermanual子目录下的内容，是JMeter的用户手册文档，其中component_reference.html文件是最常用到的核心元件帮助文档。

本篇文章来源于：[博主繁花似锦](https://www.cnblogs.com/liuyuelinfighting/p/14900019.html)
