---
title: 2.JMeter的安装和启动
date: 2023-09-05 17:17:41
tags:
   - JMeter
categories:
   - JMeter学习 
cover: https://s2.loli.net/2023/08/30/LkrjWplvHyc1nDm.png
---

## JMeter的安装和启动

#### 1. 安装Java环境

由于JMeter是纯Java的桌面应用程序，因此它的运行环境需要Java环境，即需要安装JDK或JRE。（也就是安装JDK环境）

步骤简要说明:

- 下载并安装JDK
- 配置环境变量

#### 2. JMeter下载

JMeter官方地址:https://jmeter.apache.org/

{%asset_img 19-05-23-171931.png%}

点击Download Releases进入JMeter下载页面:

{%asset_img 19-05-23-171942.png%}

接下来点击Apache JMeter archives...，我们下载一个5.0版本的JMeter。

{% asset_img 19-05-23-171950.png%}

如下图，点击下载:

{%asset_img 20-05-23-172000.png%}

提示：工具下载成功后，直接解压就可以使用了，不用进行安装。

**注意**:

我们不要点击在Apache JMeter archives...的页面中点击sourse下载JMeter

如下图:

{%asset_img 20-05-23-172014.png%}

进入下载页面:

{%asset_img 20-05-23-172025.png%}

不要下载这个安src.zip的JMeter安装包。因为这个包中的bin目录下并没有ApacheJMeter.jar文件，所有JMeter是无法启动的，双击bin目录中的jmeter.bat文件，会出现下图情况:

{%asset_img 20-05-23-172032.png%}

所以记住，要下载binarles页面中的JMeter安装包。

#### 3.JMeter启动

JMeter解压后的文件夹，进入bin目录中，双击jmeter.bat文件，即可打开JMeter。

JMeter开启会出现如下两个窗口:

{%asset_img 20-05-23-172037.png%}

{%asset_img 20-05-23-172042.png%}

注意：打开的时候会有两个窗口，JMeter的命令窗口和JMeter的图形操作界面，不可以关闭命令窗口。

这样我们就成功启动了JMeter。

#### 4.JMeter切换中文语言

JMeter工具默认是支持简体中文的，需要我们手动切换一下。

如下图：安装完JMeter第一次进入的时候，默认是英文语言显示的。

{%asset_img 20-05-23-172047.png%}

我们可以切换成中文语言：

options——>Choose Language——>Chinese(Simplified)

就这样很简单，中文显示如下图:

{%asset_img 20-05-23-172055.png%}

这样设置只是暂时把JMeter切换成为中文语言，但是当我们下次再打开JMeter时又恢复了英文，还需要再进行设置，这样一来就很麻烦。

直接改JMeter**的默认语言**

修改JMeter的bin目录下，有一个jmeter.properties文件，这个是JMeter的核心配置文件。

打开文件后，找到language，将language=en改成language=zh或者zh_CN即可，如下。

{%asset_img 21-05-23-172103.png%}

这样每次打开JMeter时，就默认就是中文语言了。

本篇文章来源于：[博主繁花似锦](https://www.cnblogs.com/liuyuelinfighting/p/14900019.html)

