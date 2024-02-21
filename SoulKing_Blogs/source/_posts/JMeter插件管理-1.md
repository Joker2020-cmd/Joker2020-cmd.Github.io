---
title: 3.JMeter插件管理
date: 2023-09-05 17:39:51
tags:
   - JMeter
categories:
   - JMeter学习 
cover: https://s2.loli.net/2023/08/30/LkrjWplvHyc1nDm.png
---

## **JMeter插件管理**

JMeter是一个Java开发的开源软件，开源的软件有一个好处，就是会有很多第三方开发出来的插件，使得JMeter在处理某一些功能的时候更加的方便。并且这些插件拿过来就可以使用，完全免费的。

我们安装好的JMeter，自身会携带一些必须的组件，一般来说是符号我们平时工作需要的，但是有一些功能或者组件，可能使用第三方插件效果更好。

总结：JMeter作为来源性能测试工具，第三方团队开发了更多的配置功能，即JMeter插件。

### 1.**安装JMeter插件管理器**

##### （1）JMeter插件管理器介绍：

- JMeter插件管理器的使用方法很简单:不要手动安装各种插件，它提供了友好的用户界面来完成:安装、升级、卸载等操作。
- JMeter插件管理器所管理的插件包括jmeter-plugins.org上面常见的插件，和各种第三方插件，甚至核心JMeter插件。

##### （2）安装JMeter插件管理器，步骤如下：

在我们刚刚安装好的JMeter中，是没有插件管理器的，这需要我们手动进行安装。

查看插件管理器，点击Option（选项），在显示出的菜单的最下面就能够看到。

如下图，此时的JMeter是没有安装插件管理器的。

{%asset_img 40-05-23-174049.png%}

我们需要到网站：http://jmeter-plugins.org/downloads/all/中下载JMeter插件管理器。

如下图：

{%asset_img 40-05-23-174059.png%}

上图中很明确的告诉我们:

- Installing Plugins:安装插件。

- The easiest way to get the plugins is to install Plugins Manager. Then you'll be able to install any other plugins just by clicking a checkbox.

  获得插件的最简单方法是安装插件管理器。

  然后，您只需要单击复选框即可安装任何其他插件。

- Download plugins-manager.jar and put it into lib/ext directory, then restart JMeter

  下载plugins-manager.jar并将其放入lib/ext 目录中。

{%asset_img 41-05-23-174105.png%}

然后重新启动JMeter。

在点击Options（选项），就能看到JMeter插件管理器Plugins Manager选项了。

{%asset_img 41-05-23-174109.png%}

总结：

安装JMeter插件管理器大概流程是：

- JMeter插件管理器，是一个jar文件。
- 将该文件拷贝到JMeter安装目录中的"\lib\ext"目录下。
- 然后重启JMeter

这样就可以使用JMeter插件管理器了。

同时在JMeter界面中的快捷工具栏中，也会出现插件管理器图标，如下图：

{%asset_img 41-05-23-174115.png%}

### 2.**JMeter插件管理器界面说明**

我们点击Option（选项），然后点击Plugins Manager选项，进入JMeter插件管理器。

如下图：

{%asset_img 41-05-23-174120.png%}

说明：

- Installed Plugins(已安装的插件)：即插件目录中已经包含的插件，可以通过选中勾选框，来使用这些插件。
- Available Plugins（可下载的插件）:即可以安装的一些插件，通过选中勾选框，来下载你所需要的插件。
- Upgrades（可更新的插件）：即显示有最新版本的一些插件，通过选中勾选框，点击界面右下角的Apply Changes and Restart JMeter按钮来下载，之后更新重启JMeter。

提示：一般不建议进行更新操作，因为最新的插件都有一些兼容问题，而且很可能导致JMeter无法使用（经常报加载类异常错误）！！！

### 3.JMeter插件安装和卸载

#### （1）通过JMeter插件管理器安装插件

点击Option（选项），然后点击Plugins Manager选项，进入JMeter插件管理器。

然后点击Available Plugins，来下载我们所需的插件，例如以PerfMon插件为例:

{%asset_img 41-05-23-174125.png%}

勾选PerfMon插件，点击界面右下角的Apply Changes and Restart JMeter按钮安装。

下载安装好后，就可以在Installed Plugins列表中看到PerfMon插件了。

如下图：

{%asset_img 41-05-23-174130.png%}

提示：下载下来的PerfMon插件文件，会出现在JMeter的apache-jmeter-5.0、lib\ext目录中。

{%asset_img 41-05-23-174143.png%}

#### （2）通过JMeter插件管理器卸载插件

关于卸载插件，可以在Installed Plugins列表中，取消勾选perfMon插件，然后点击Apply Changes and Restart JMeter按钮。这样就是把PerfMon插件卸载掉，同时也删除了apache-jmeter-5.0\lib\ext目录中插件文件。（如果有需要可以再按照上面的步骤安装所需要的插件）

重点提示：

Review Changes窗格很重要，它列出了在点击"Apply"按钮后应该完成的所有更改。

有时插件之间存在依赖关系，因此可能会有额外的插件卸载，所以在应用之前先回顾一下变化。

如下图：

{%asset_img 41-05-23-174149.png%}

本篇文章来源于：[博主繁花似锦](https://www.cnblogs.com/liuyuelinfighting/p/14900019.html)
