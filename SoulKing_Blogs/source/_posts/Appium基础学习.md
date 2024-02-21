---
title: Appium学习
date: 2023-08-29 09:37:08
categories:
   - Appium基础学习
tags: 
   - Appium学习
keywords: "Appium基础"
abbrlink: ""
decription:
top_img:
cover: https://s2.loli.net/2023/08/30/52LHYFsPjieb4K8.png

---



# **移动端自动化测试框架之Appium**

## Appium元素定位

元素定位工具：

- Android使用Android—SDK里的Uiautomatorviewer工具。
- IOS使用Appium Desktop里的Appium Inspector检测器。

#### 1.	**By_id定位（废弃统一用MobileBy类）**

通过id属性定位元素，IOS应用上的元素没有这个属性，所有仅支持Android。

代码如下：

```python
#单数
driver.find_element_by_id("id属性值")
#复数
driver.find_elements_by_id("id属性值")
```

如下图所示：利用Uiautomatorviewer工具查看元素信息，resource-id属性就是元素的id属性。

{% asset_img  image-20230822155516893.png %}

练习：

开启Appium服务，执行如下代码：

```python
"""
1.学习目标
	必须掌握appium中元素定位基本方法(这些方法我们在Selenium中学习过)
	练习目标：掌握元素定位方式id定位
2.操作步骤
	id定位
		只适用于Android，IOS不支持，id并不是唯一属性标识
		driver.find_element_by_id("id属性值")#单数
		driver.find_elements_by_id("id属性值")#复数
3.需求
	在设置App中使用id属性定位”显示“
"""
#1.导入appium
import time
from appium import webdriver

#2.创建Desired capabilities对象，添加启动参数
desired_caps = {
    "platformName":"Android",#系统名称
    "platformVersion":"7.1.2",#系统版本
    "deviceName":"127.0.0.1:62001",#设备名称
    "appPackage":"com.android.settings",#App包名
    "appActivity":".Settings"#App启动名
}

#3.启动App
driver = webdriver.Remote("http://127.0.0.1:4723/wd/hub",desired_caps)

#4.定位元素
#在移动端，元素的id不再是唯一的，
#我们可以在Uiautomatorviewer工具查看到，一个页面中类似元素的id都是相同的。
#这个时候我们就要用driver.find_element_by_id来定位元素
#对应的字段resource-id属性值。
#4.1 定位设置ap界面中的所有标题
id_elements= driver.find_elements_by_id("android:id/title")

#4.2 查看一共获取了多少标题title
print("元素数量："+str(len(id_elements)))

#4.3 遍历获取标题对应的文本
for element in id_elements:
    print(element.text)#遍历打印所有元素的text值
    
#5.关闭App
time.sleep(3)
driver.quit()
```

输出结果如下：

{% asset_img  image-20230822161734990.png %}

#### 2.	by_name定位

Appium版本在1.5之后，Android就不再支持name的元素定位方法。

Android如果使用name方法，则报如下错误：

{% asset_img  image-20230822162016028.png %}

IOS可以正常使用name元素定位。

代码如下：

```python
#单数
driver.find_element_by_name("name")
#复数
driver.find_element_by_name("name")
```

如图所示：利用Appium Inspector检查器，name属性指的是name属性。

{% asset_img image-20230822162337100.png %}

使用命令：driver.find_element_by_name("3个月")

这里我们就不做演示了。

#### 3.	**by_class_name定位**

通过class_name属性定位元素。

代码如下：

```python
#单数
driver.find_element_by_class_name("class值")
#复数
driver.find_element_by_class_name("class值")
```

1. **Android**:

   如图所示：利用Uiautomatorviewer工具查看，class_name属性指的是class属性。

   {% asset_img image-20230822162910046.png %}

2. **IOS:**

   如图所示：利用Appium Inspector检查器，class_name属性指的是type属性。

   {% asset_img image-20230822163042684.png %}

   使用命令：driver.find_element_by_class_name("XCUIElementTypeStaticText")

   练习：

   ```python
   """
   1.学习目标
   	必须掌握appium中元素定位基本方法(这些方法我们在Selenium中学习过)
   	练习目标：掌握元素定位方式id定位
   2.操作步骤
   	id定位
   		只适用于Android，IOS不支持，id并不是唯一属性标识
   		driver.find_element_by_class_name("id属性值")#单数
   		driver.find_elements_by_class_naem("id属性值")#复数
   3.需求
   	在设置App中使用id属性定位”显示“
   """
   #1.导入appium
   import time
   from appium import webdriver
   
   #2.创建Desired capabilities对象，添加启动参数
   desired_caps = {
       "platformName":"Android",#系统名称
       "platformVersion":"7.1.2",#系统版本
       "deviceName":"127.0.0.1:62001",#设备名称
       "appPackage":"com.android.settings",#App包名
       "appActivity":".Settings"#App启动名
   }
   
   #3.启动App
   driver = webdriver.Remote("http://127.0.0.1:4723/wd/hub",desired_caps)
   
   #4.定位元素
   #在移动端，元素的id不再是唯一的，
   #我们可以在Uiautomatorviewer工具查看到，一个页面中类似元素的id都是相同的。
   #这个时候我们就要用driver.find_element_by_id来定位元素
   #对应的字段resource-id属性值。
   #4.1 定位设置ap界面中的所有标题
   class_elements= driver.find_elements_by_class_name("android.widget.TextView")
   
   #4.2 查看一共获取了多少标题title
   print("元素数量："+str(len(class_elements)))
   
   #4.3 遍历获取标题对应的文本
   for element in class_elements:
       print(element.text)#遍历打印所有元素的text值
       
   #5.关闭App
   time.sleep(3)
   driver.quit()
   ```

   运行结果：

   {% asset_img image-20230822163521210.png %}

#### 4.	**by_xpath定位**

通过xpath定位元素，这样就可以在页面中定位一个单个的元素了。

（如果一个元素的id属性或者class_name属性也是唯一的，我们也可以通过id属性或者class_name属性进行定位。）

代码如下：

```python
#单数
deriver.find_element_by_xpath("xpath")
#复数
deriver.find_elements_by_xpath("xpath")
```

在移动端xpath用法与Web Selenium中的用法一致，在移动端自动化测试中使用xpath定位元素是比较多的。

练习：

```python
"""
1.学习目标
	必须掌握appium中元素定位基本方法(这些方法我们在Selenium中学习过)
	练习目标：掌握元素定位方式id定位
2.操作步骤
	id定位
		只适用于Android，IOS不支持，id并不是唯一属性标识
		driver.find_element_by_xpath("xpath表达式")#单数
		driver.find_elements_by_xpath("xpath表达式")#复数
3.需求
	在设置App中使用xpath定位”显示“标题
"""
#1.导入appium
import time
from appium import webdriver

#2.创建Desired capabilities对象，添加启动参数
desired_caps = {
    "platformName":"Android",#系统名称
    "platformVersion":"7.1.2",#系统版本
    "deviceName":"127.0.0.1:62001",#设备名称
    "appPackage":"com.android.settings",#App包名
    "appActivity":".Settings"#App启动名
}

#3.启动App
driver = webdriver.Remote("http://127.0.0.1:4723/wd/hub",desired_caps)

#4.定位元素
#4.1 xpath定位“显示”按钮
more = driver.find_elements_by_xpath("//*[@text='显示']")

#4.2 点击“显示”元素，进入显示页面
more.click()


    
#5.关闭App
time.sleep(3)
driver.quit()
```

#### 5.	**by_accessibility_id定位**

通过accessibility_id属性查找元素。（移动端特有）

标识附加到给定元素的辅助功能或标签的字符串。

针对ios的辅助功能标识符和针对Android的内容描述。

代码如下：

```python
#单数
driver.find_element_by_accessibity_id("accessibity_id")

#复数
driver.find_element_by_accessibity_id("accessibity_id")
```

1. Android：

   如图所示：利用Uiautomatorviewer工具查看，accessility_id属性指的是content-desc属性。

   {% asset_img image-20230825111551346.png %}

   练习：

   ```python
   """
   1.学习目标
    	掌握appium中accessibility_id元素定位方法
    2.操作步骤
    	使用方法：
    	driver.find_element_by_accessibility_id("content-desc属性")
    3.需求
    	在设置App中使用accessibility_id方法定位搜索按钮
   """
   
   #1.导入appium
   import time
   from appium import webdriver
   
   #2.创建Desired capabilities对象， 添加启动参数
   desired_caps = {
       "deviceName": "真机序列号或模拟器本机地址127.0.0.1 ：+模拟器端口",#设备名称
       "platformName"： "Android",#系统名称
       "platformVersion": "12",#系统版本
       "appPackage": "com.android.setting",#包名
       "appActivity": ".Settings"#app启动名
   }
   
   #3.启动App
   driver = webdriver.Remote("http://127.0.0.1:4723/wd/hub",desired_caps)
   
   #4.定位元素
   #4.1 定位搜索按钮，通过accessibility_id方法
   search = driver.find_element_by_accessibility_id("搜索")
   
   #4.2点击搜索按钮
   serch.click()
   
   #5.关闭app
   time.sleep(3)
   driver.quit()
   ```

   

2. IOS

   如图所示：利用Appium Inspector检查器，accessibility_id指的是accessibility id属性或者name属性或label属性。

   {% asset_img image-20230825113322301.png %}

   使用方法：driver.find_element_by_accessibility_id("More Info")

#### 6.	**By_android_uiautomator定位**

通过Android Uiautomator搜索查找元素。（Android系统特有）

这允许使用Uiautomator库，使用递归元素搜索来找到Android应用程序中的元素。

代码如下：

```python
#单数
driver.find_element_by_android_uiautomator("android_uiautomator")

#复数
driver.find_element_by_android_uiautomator
```

可以利用uiautomatorviewer工具查看元素属性信息。

uiselector工具类介绍：

1. text属性的方法（text指的是text属性）

   （text指的是text属性）

   - 完全匹配。

     driver.find_element_by_android_uiautomator('new UiSelector(),text("AndroidUI")')

   - 包含匹配（模拟定位）。

     driver.find_element_by_android_uiautomator('new UiSelector().textContains("Android")')

   - 以什么内容开始匹配。

     driver.find_element_by_android_uiautomator('new UiSelector().textStartWith("AndroidUI")')

   - 正则匹配查找。

     driver.find_element_by_android_uiautomator('new UiSelector().textMatches("^Android.*")')

2. className属性的方法

   （className指的是class属性）

   - 完全匹配

     driver.find_element_by_uiautomator('new UiSelector().className("android.widget.TextView").text("AndroidUI")')

   - 正则匹配查找

     driver.find_element_by_android_uiautomator('new UiSelector().classNameMatches(".*TextViews").text(AndroidUI)')

3. xpath方法定位

   driver.find_element_by_android_uiautomator('new UiSelector().className("android,widget.TextView").childSelector(new UiSelector().text("AndroidUI")')

4. resourceId属性的方法

   （resourceId指的是resource-id属性）

   - driver.find_element_by_android_uiautomator('new UiSelector().resourceId("com.android.calculator2:id/op_add")')
   - driver.find_element_by_android_uiautomator('new UiSelector().resourceIdMatches(".*id/op_adds")')

5. description属性的方法

   （description指的是content-desc属性）

   - driver.find_element_by_android_uiautomator('new UiSelector().description("加")')
   - driver.find_element_by_android_uiautomator('new UiSelector().descriptionStartsWith("加")')
   - driver.find_element_by_android_uiautomator('new UiSelector().descriptionMatches("^加.*")')

   UiSelector类中还支持其他一些方法，比如根据控件属性是否可聚焦可长按等来缩小要定位的控件的范围，具体使用方法不一一列举了。

   练习：

   ```python
   """
   1.学习目标
   	了解appium中元素定位Android专用方法 Android_UIautomator
   2.操作步骤
   	使用方法：
   	driver.find_element_by_android_uiautomator("java代码")
   	java代码中使用UiSelector类来处理元素的定位
   		new UiSelector().text("本文内容")
   3.需求
   	在设置App中使用Android专用的定位方法
   """
   
   #1.导入appium
   import time
   from appium import webdriver
   
   #2.创建Desired capabilities对象，添加启动参数
   desired_caps = {
       "deviceName": "127.0.0.1:62001",#设备名称
       "platformName": "Android",#系统名称
       "platformVersion": "12",#系统版本
       "appPackage": "com.android.settings",#App包名
       "appActivity": ".Settings"#app启动名
   }
   
   #3.启动App
   driver = webdriver.Remote("http://127.0.0.1/wd/hub",desired_caps)
   
   #4.定位元素
   #4.1定位蓝牙，通过Android_Uiautomator方法
   #单引号里面是java代码，使用UiSelector这个类来定位元素。
   blue_tooth = driver.find_element_by_android_uiautomator("new UiSelector().text("蓝牙")")
   
   #注意：
   #上面new UiSelector().text("蓝牙")中，一定是外单引号，里边双引号
   #因为这是一段java代码，而text()方法中要传入一个string类型的参数
   #java中String类型的参数是加双引号的，所以必须是外单内双
   #否则会报错。
   
   #4.2点击搜索按钮
   blue_tooth.click()
   
   #5.关闭App
   time.sleep(3)
   driver.quit()
   ```

## Web页面自动化测试，元素定位是页面自动化测试的第一步。

## App自动化测试的第一步，也是元素的定位。

## App中的元素定位工具大致有三种：

1. ##   Uiautomatorviewer	工具

2. ##   Appium 	Inspector	工具

3. ##   Chrome	Inspect	工具

## **Uiautomatorviewer**

### 1.	Uiautomatorviewer介绍

在开始编写代码之前，需要获取待测应用的UI元素，可以通过Uiautomatorviewer工具来获取应用的界面截图并分析。

Uiautomatorviewer工具获取当前UI界面的快照，提供一个可视化的界面，来查看UI布局结构，并且可以查看各个控件的相关属性。利用这些信息来选择特定的UI组件并创建App中的自动化UI测试代码。

总结：

- Uiautomatorviewer工具是专门用来定位Android系统App中原生页面中的元素。
- Uiautomatorviewer是AndroidSDK里的一个工具，这个工具在AndroidSDK目录下的tools文件夹下。（IOS系统在tools\bin的子目录下）
- Uiautomatorviewer使用简单，速度也相对比较快。

### 2.	Uiautomatorviewer工具打开方式

在下载的Android—SDK中找到tools里的Uiautomatorviewer.bat文件双击打开工具

{% asset_img image-20230822093033212.png %}

先会出现一个黑窗口，然后就会出现一个Uiautomatorviewer工具的界面

{% asset_img image-20230822093619228.png %}

### 3.	Uiautomatorviewer布局界面介绍

{% asset_img image-20230822093814089.png %}

整个界面分四个区域:

   1. **工作栏区**

      在工作栏中共有4个按钮从左至右分别用于：

      - 打开已保存的界面快照和布局
      - 抓取当前手机屏幕截图（Device Screenshoot Uiautomator dupm）
      - 带有压缩层次结构的设备屏幕截图（Device Screenshoot with Compressed Hierarchy (Uiautomator dump -compressed)）第二按钮于第三按钮把全部布局呈现出来，而第三按钮只呈现有用的控件布局。比如存在一个Frame，但只有装饰功能，那么点击第三按钮时，可能不被呈现。
      - 保存当前屏幕界面的快照和布局

​			2.**屏幕截图区**

​				显示当前屏幕显示的布局图片

​			3.**布局区**

​				已Xml树的形式，显示控件布局

​			4.**控件属性区**

​				当点击某一控件时，将显示该控件属性信息。

### 4.Uiautomatorviewer工具的使用

1. ​	**Uiautomatorviewer工具使用的前提**
   - 打开Uiautomatorviewer工具
   - 所测试设备是开机状态（手机或者模拟器）				
   - 确保电脑与设备是链接状态，也就是cmd进入命令行终端，输入 adb connect 127.0.0.1:62001链接夜神模拟器，输入 adb devices 能够获取设备名称

如果电脑没有与设备不是链接的状态，点击Uiautomatorviewer工具栏中的第二个按钮，来抓取手机界面是抓不到的。

如下图所示：

{% asset_img image-20230822095942818.png %}

2. ​	**抓取当前手机的屏幕界面**

​					就是点击Uiautomatorviewer工具栏中的第二个按钮进行抓取:

{% asset_img image-20230822100211467.png %}

说明:点击完成后，设备上的界面就会被同步到这个工具的左侧，点击左侧需要查看的控件，在这个工具的右侧就可以看到这个控件对应的详细信息。

提示:

当Uiautomatorviewer能够不抓取手机屏幕时，可以先关闭和重启adb服务

执行命令如下：

- adb	kill-server
- adb    start-server

​		3.**定位其它页面里的元素**

​			如果需要继续定位其他页面里的元素，先在设备中操作到定位元素的页面后，再次点击工具左上角的排子按钮，就可以抓取最新的页面元素信息。

例如：

定位完设置App中界面的元素，我又想定位淘宝首页的页面元素。

首先在手机中打开淘宝App，进入到淘宝App的首页。

{% asset_img image-20230822101225757.png %}

然后点击Uiautomatorviewer工具栏中的第二个按钮，来抓取新的界面

{% asset_img image-20230822101402155.png %}

​					4.**保存**

​						点击保存按钮，可保存抓取的屏幕截屏和一个uix文件（XML格式的的页面布局，就相当于页面源码）。

{% asset_img image-20230822101641447.png %}

我们可以看到文件夹中会有上面说过的两个文件。

{% asset_img image-20230822101739507.png %}

我们也可以对保存的文件进行重命名，方便我们以后的使用和管理。

{% asset_img image-20230822101848241.png %}

​				5.打开已保存的界面快照和布局

​				点击打开文件，可以将之前保存好的页面屏幕截图和.uix文件导入进来。

{% asset_img image-20230822102038461.png %}

点击ok之后，就选显示以前保存过的界面信息了。

如下图所示:

{% asset_img image-20230822102209820.png %}

导入后即可进行元素定位操作。

## Appium Inspector

### 1.	**Appium Inspector**

Appium Server有两种启动方式

- 一种Appium Desktop有图形界面的启动方式，称之为桌面版；
- 另一种版本是通过npm安装，使用命令行参数启动的Appium Server；

而Appium Inspector工具就在Appium Desktop中，Appium Inspector是Appium Desktop附带的一个元素定位检查器，用来调试定位应用程序很方便。

Appium Inspector工具同时支持Android系统和IOS系统中原生界面的元素定位。

### 2.   **Appium Inspector打开方式**

Appium  Desktop安装完成之后，双击打开。

{% asset_img image-20230822103602071.png %}

说明：界面有3个Tab选项

- Simple：默认配置，监听本机4723端口；
- Advanced：高级设置，可以自定义Appium Server端的配置，配置好后可以保存到Presets；
- Presets：修改Advanced高级设置中的配置项。

一般我们测试直接使用Simple即可，点击Start Server按钮，启动Appium Server，并开启监听本机4723端口。开启服务后，界面跳转到服务端控制台，如下图所示：

{% asset_img image-20230822104412007.png %}

提示：

控制台显示运行的脚本中的日志信息，右上角有3个按钮，分别是：

- 第一个按钮Start  Inspector Session，开启Appium Inspector定位工具；注意：inspector会新开一个Session；
- 第二个按钮Get Raws Logs ，下载当前控制台中的log信息；
- 第三按钮Stop Server，关闭当前的Appium Server。

有两种方式可以启动Appium Inspector工具，

- 方式一：点击右上角三个按钮中的第一个（一个放大镜样子的按钮），打开Appium Inspector工具。
- 方式二：点击左上角File-->New Session Window.....Ctrl+N可以打开Appium Inspector工具

如下图：

{% asset_img image-20230822105454030.png %}

Appium  Inspector工具开启后的界面如下图：

{% asset_img image-20230822105546386.png %}

### 3.Appium Inspector布局介绍

Appium Inspector布局如下图所示：

{% asset_img image-20230822105722220.png %}

上图说明：

1. 布局1是Appium Inspector服务的设置。Automatic  Server：自动服务器  Custom  Server:定制服务器 Select  Cloud  Providers:选择云提供商我们一般使用Automatic Server即可：will use currently-running Appium Desktop server http：//localhost：4723将使用当前运行的Appium桌面服务器http：//localhost：4723
2. 布局2是高级设置。可以设置Allow Unauthorized Certificates：允许未经授权的证书。Use Proxy：使用代理服务器。初学一般我们不进行高级设置。
3. 布局3是Desired Capabilities参数设置。Desired  Capabilities：编写Desired  Capabilities参数。Saved Capability Sets：已保存的Desired Capabilities，可以进行查看和修改。Attach to Session....:附加到会话...（用到的时候再说）

### 4.Appium Inspector工具的配置

1. **Appium Inspector工具使用前提**

   - 打开Appium Desktop，开启Appium Inspector工具
   - 所测试设备是开机状态（手机或者模拟器）
   - 确保电脑与设备是链接状态，也就是从cmd进入命令行终端，输入  adb  connect 127.0.0.1:62001链接夜神模拟器，输入  adb   Devices能够获取设备名称。

2. **Appium Inspector的服务器设置和高级设置**

   - 服务器设置：选择Automator Server （一定要记得点击一下，进行选中）
   - 高级设置：不进行设置

3. **编写Desired Capabilities参数（重点）**

   可以在左侧一行一行手动添加，如下图所示：

   {% asset_img image-20230822111942478.png %}

   提示：第二列的格式是针对第三列value值而言的。

   也可以把Json格式的数据编辑好，直接粘贴在右侧Json Representation里。

   {% asset_img image-20230822112351826.png %}

   直接把Json格式的数据直接粘贴过来。

   {% asset_img image-20230822112448330.png %}

   点击保存之后，数据会同步到左侧，如下图所示：

   {% asset_img image-20230822112543774.png %}

4. **保存Desired Capabilities参数**

   如有需要，在编辑完成Desired Capabilities参数之后，可以对其进行保存，方便以后的管理和使用。

   {% asset_img image-20230822112903927.png %}

   {% asset_img image-20230822112923800.png %}

5. **查看和修改已存储的Desired Capabilities**

   点击Saved Capabilities sets标签页，可以查看和修改已存储的Desired Capabilities。

   {% asset_img image-20230822113205328.png %}

6. **开启Session，连接手机获取手机界面**

   点击start Session，开启使用Appium Inspector工具。

   如下图所示：

   {% asset_img image-20230822113541137.png %}

   说明：

   - Appium Inspector需要我们手动创建一个session，其实也就是一个客户端，和Appium Server连接，并且需要在Desired Capabilities 里面填入一些参数。
   - 所需功能是在Desired Capabilities对象中编码的键和值，当请求新的自动化会话时，由Appium客户端发送到Appium Server服务器。Desired Capabilities告诉Appium驱动程序有关您希望测试如何工作的各种重要信息。最终Desired Capabilities将作为Json对象发送到Appium。
   - 所需功能的Desired Capabilities对象可以在WebDriver测试中编写脚本，也可以在Appium Server GUI中设置（通过Inspector会话中，就是上边的介绍方式）。

   提示：

   当Appium Inspector能够不能抓取手机屏幕时，可以关闭和重启adb服务，或者重启Appium Inspector服务。

   命令如下：

   - adb kill-server
   - adb start-server

### 5.	**Appium Inspector工具的使用**

1. **Inspector定位控件界面的详细介绍**

   {% asset_img image-20230822114808809.png %}

   上图说明：

   - 布局1：截图的手机界面

     可以点击选择元素。

   - 布局2：顶部操作栏

     从左往右的按钮依次是

     Select Element：选择元素。

     Swipe By Coordinates：选择滑动的起始和结束位置。

     Tap By Coordinates：使得手机界面变换可操作状态，可以点击界面的元素。

     Back：模拟Android的返回键。

     Refresh Source & Screenshot：刷新页面，用来重新获取手机当前界面。

     Start Recording：录制操作。

     Search for element：校验定位表达式。

     Copy XML Source to Clipboard：复制XML树。

     Quit Session & Close Inspector：退出当前Session。

   - 布局3：XML树

     以XML树的形式，展示界面上的控件布局。

   - 布局4：控件属性区域

     选择某个控件，在这里可以显示该控件的所有属性和值。

   

2. **Selected  Element的介绍**

   选择元素功能：

   {% asset_img image-20230822133904939.png %}

   1. **顶部的Tap、Send Keys、Clear**

      模拟用户的操作：

      - tap：相当于点击元素。
      - Send keys：输入值、针对输入框的操作。
      - Clear：清除所有值。

      建议：不建议用这些操作，因为很容易造成断开连接（左侧界面一直loading）.....反正我这边经常这样，如果不会的话当然最好用啦！

   2. **Find By xpath**

      提供了该元素的XPATH表达式

      不推荐用，绝对路径太长了.....还是自己写吧！

   3. **那串黄色背景色的英文**

      不建议使用XPATH定位器，因为它很脆弱，建议让开发团队提供独特的可访问性定位器（即：resource-id）

   4. **Attribute-Value**

      属性列表

3. **Search for element的介绍**

   搜索元素功能，位置如下图：

   {% asset_img image-20230822134908939.png %}

   点击弹出如下界面：

   选择定位策略，如下图：

   {% asset_img image-20230822134952072.png %}

   填写对应的定位表达式，如下图：

   {% asset_img image-20230822135027681.png %}

   点击Search 就可以进行元素定位了。

   如果能找到Element的话表达式就是正确的，然后你还可以针对元素进行一些操作。

4. **在Appium Inspector中操作手机**

   当我们使用Appium Inspector定位工具获取到手机设置App界面的时候，如下图：

   {% asset_img image-20230822135321766.png %}

   点击顶部操作栏中的Tap By Coordinates按钮，使得手机界面变换可操作状态。

   然后我们在左侧的手机界面中点击显示，就可以进入到显示的界面中了。

   {% asset_img image-20230822135531324.png %}

   {% asset_img image-20230822135548543.png %}

   进入到显示之后，现在我们还是保持在可操作手机的状态。

   之后我们就可以继续操作手机，也可以点击Select Element按钮，在当前页面中进行选择的元素。

   {% asset_img image-20230822135756380.png %}

   我们也可以点击Back按钮，返回到设置App的首界面。

   {% asset_img image-20230822135855228.png %}

   提示：这一点Appium Inspector定位工具就比Uiautomatorviewer定位方便多了。

5. **Start Recording的介绍（了解）**

   {% asset_img image-20230822140101952.png %}

   - 操作步骤：点击开始录制之后，在点击Tap By Coordinates，进入界面可操作状态。
   - 然后就可以开始点击你想要的元素了，这个时候就开始录制了。
   - 最后在Recorder下面会显示对应的代码，右侧可以选择不同的语言。
   - 建议：不要过多使用该功能，可以看到录制的代码是根据坐标去定位元素的，换个手机同一个元素坐标可能就不同了，可移植性不高。

### 6.	**Uiautomatorviewer工具和Appium Inspector工具对比**

1. **Uiautomatorviewer的局限性：**

   1. 不能校验我们写的定位表达式是否正确定位到控件（类似浏览器上的F12）
   2. 连接不够稳定
   3. 不能模拟用户动作

2. **Appium Desktop的Inspector的优势：**

   1. 可以校验定位表达式（如：XPATH表达式）。
   2. 通过设置Desired Capabilities来连接手机，比较稳定。
   3. 可以模拟用户动作（如：点击、返回、滑动等操作）。
   4. 可以录制一系列操作，然后转换成代码。

   提示：

   学习或者编写脚本过程中，使用桌面版会方便一些，因为桌面版还提供了定位工具。而实际运行的时候，使用Server版本会更灵活，更容易与CI工具进行集成。

## Chrome  Inspect

### 1.	**Chrome Inspect的介绍**

Chrome Inspect定位工具是用来抓取App中Webview页面的。

为了项目的需求，为了更好的保证效果和布局跨平台，Android&H5混合开发一般是我们不错的选择。Google浏览器中的Chrome Inspect定位工具，提供了一个移动端Web页面开发调试的功能，通过它我们可以调试手机页面，可以看到页面的源码，从而进行元素的定位。

**使用chrome Inspect定位工具的前提条件**

使用Chrome开发人员工具调试原生Android应用中的WebView，Android版本应该在Android4.4（kitkat）或更高版本上，通过DevTools在原生Android应用中调试WebView页面中的内容。

### 2.	**Chrome Inspect打开方式**

打开PC端的Chrome浏览器，在访问地址栏中输入Chrome：//inspect/就可以了，就是这么简单。

如下图：

{% asset_img image-20230822142646593.png %}

### 3.	**Chrome Inspect工具的使用**

1. **Chrome Inspect工作前提**

   - 所测试设备是开机状态（手机或者模拟器）。

   - 确保电脑与设备是链接状态，也就是cmd进入命令行终端，

     输入adb connect 127.0.0.1:62001链接夜神模拟器，

     输入adb devices能够获取设备名称。

2. **Chrome Inspect操作**

   1. **在App中打开含有WebView的页面**

      例如：开百度App，进入到微博登陆的界面就是一个含有Webview的页面

      如下图：

      {% asset_img image-20230822144556161.png %}

   2. **在Chrome Inspect中识别到Webview页面**

      我们进入到PC端的Chrome浏览器中，访问地址栏中输入chrome：//inspect/（没有显示的话就点击一下刷新），就可以检测到当前应用程序界面是Webview页面了。

      如下图：

      {% asset_img image-20230822145022696.png %}

   3. 点击inspect可以进入调试视图

      点击如上图中的inspect，可以进入Chrome Inspect工具的调试视图。

      会弹出一个新窗口，显示当前页面的Webview元素信息。

      并且元素定位方法同Selenium WebDriver一致。

      {% asset_img image-20230822145325532.png %}

      就是这么简单。

4.**使用Chrome Inspect遇到的问题**

1. **Android系统版本问题**

   Android移动设备版本应该在Android4.4或更高版本上。

   从Android4.4开始，webkit是支持远程调试的。

2. **所测App的debug模式要打开**

   在使用Chrome Inspect工具调试移动端App的Webview页面的时候，需要将该App的debug模式打开。

   但其实大部分App的debug模式都是关闭的，要去找一个开启debug模式的版本还是比较麻烦的，因此需要使用借助第三方工具来强制开启任何App的Android webview debug模式，使之可以使用chrome Inspect。

   而这个工具就是Xposed。

3. **Xposed工具的安装**

   1. **将设备进行root。**

      因为涉及到root权限，因此需要将设备进行root。

      很多工具可以来root，比如kingRoot等。

      注：Android模拟器默认root

   2. **下载Xposed框架。**

      官方下载地址：http://repo.xposed.info/module/de.robv.android.xposed.installer

      点击页面下方的show older version，选择一个稳定版本进行下载。

      {% asset_img image-20230822150817186.png %}

   3. **安装Xposed框架。**

      将下载好的Xposed安装包de.robv.android.xposed.installer_v32_de4f0d.apk,直接拖入到Android模拟器中，进行安装。

      安装好后如下图：

      {% asset_img image-20230822151124832.png %}

   4. 安装/更新Xposed框架。

      打开Xposed Installer，选择“安装/更新”的最新版，然后“安装”会自动下载刷入。（过程可能会有一些慢）

      如下图：

      {% asset_img image-20230822151338617.png %}

      安装界面，如下图所示：

      {% asset_img image-20230822151416804.png %}

      安装更新完成后的界面，如下图所示：

      {% asset_img image-20230822151524633.png %}

      

4. **安装Xposed webview debugging模块。**

   打开Xposed界面中点击左上角的三条横杠，选择模块，然后启用需激活模块的复选框，正常重启后即可使用。

   如下图所示：

   {% asset_img image-20230822151841755.png %}

   如果你的手机中没有安装webViewDebugHook模块或者没有任何模块，如下图：

   {% asset_img image-20230822152006781.png %}

   可以在Xposed中进行下载安装WebViewDebugHook模块。

   如下图：

   {% asset_img image-20230822152114053.png %}

   然后按照上面的方式激活WebViewDebugHook模块即可。

5. **HTTP/1.1 404 Not Found和空白页问题**

   在chrome：//inspect/#devices中点击inspect出现的窗口中，界面是出现HTTP/1.1 404 Not Found 或者是空白页的现象。

   空白页，如下图所示：

   {% asset_img image-20230822152535217.png %}

   HTTP/1.1 404 Not Found界面，如下图所示：

   {% asset_img image-20230822152632759.png %}

   原因：

   对于国内的程序猿来说，由于无法访问https://chrome-devtools-frontend.appspot.com,就会出现出现HTTP/1.1 404 Not Found或者空白页面的现象。

   例如上面的@33f6ad690e178169a17596eeec8596751a696d1e就是移动设备中浏览器的一个版本号，当你换一个手机或模拟器的时候，版本号可能就不一样了。

   因为不同型号的手机生产商可能会打包不同版本的chrome浏览器内核，chrome Inspect定位工具就会先访问https://chrome-devtools-frontend.appspot.com,下载对应的chrome-devtools相关驱动，而国内无法访问并下载这些驱动，就出现了404和空白页。

   网上找到如下三种解决方式：

   - 方法一：下载devtools的Inspect的离线开发者调试工具包。（花钱，没有免费的）

   - 方法二：修改网络连接，修改hosts文件。

   - 方法三：使用第三方的chrome内核的浏览器，如QQ浏览器。

     （都不好使，大家也可以自己试试）

     推荐使用VPN，或者下载一个可FQ的谷歌浏览器用一下即可，不用的时候就关了

6. **补充：安卓模拟器打开开发者选项**

   1. 打开手机的“设置”，进入到“设置”页面
   2. 滑到“设置”页面的最下端，找到“关于手机”，进入到“关于手机”页面
   3. 找到“版本号”，连续点击
   4. 会弹出一段文字提醒，直到提醒次数为0后，结束点击
   5. 返回“设置”界面，开发者选项就出来了

本篇文章来源于：[博主繁花似锦](https://www.cnblogs.com/liuyuelinfighting/p/14900019.html)
