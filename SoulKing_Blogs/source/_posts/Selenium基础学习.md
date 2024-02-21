---
title: 1.Selenium基础学习
date: 2023-09-07 09:17:41
tags:
   - Selenium
categories:
   - Selenium学习
keywords: "Selenium"
abbrlink: ""
decription:
top_img:
cover: https://s2.loli.net/2023/09/07/FrGsiH8PfTSD2jg.png
---

### **Selenium框架---**

### （Web自动化测试工具）参考[安装Selenium类库 | Selenium](https://www.selenium.dev/zh-cn/documentation/webdriver/getting_started/install_library/)

#### 1.	简介

Selenium通过使用WebDriver支持市场上所有主流浏览器的自动化。

web Driver是一个API和协议，它定义了一个语言中立的接口，用于控制web浏览器的行为。

每个浏览器都有一个特定的WebDriver实现，称为驱动程序。

驱动程序是负责委派给浏览器的组件，并处理与Selenium和浏览器之间的通信。

Selenium在可能的情况下使用这些第三方驱动程序，但是在这些驱动程序不存在的情况下，它也提供了由项目自己维护的驱动程序。

Selenium框架通过一个面向用户的界面将这些部分连接在一起，该界面允许透明地使用不同的浏览器后端，从而实现跨浏览器和跨平台自动化。

##### 1.1	安装Selenium类库（配置自动化的浏览器）

###### 首先，你需要为自动化项目安装Selenium绑定库。库的安装过程取决于您选择使用的语言。

###### Selenium安装方式：

###### Pip

###### pip 	install 	selenium

##### 1.2	第一个Selenium脚本

###### **八大基本组成部分**

Selenium所做的一切，就是发送给浏览器命令，用以执行某些操作或为信息发送请求。您将使用Selenium执行的大部分操作，都是以下基本命令的组合：

1. ###### 	使用驱动实例开启会话

   ```python
   driver = webdriver.Chrome()
   ```

2. ​      **在浏览器上执行操作**

   ```python
   #在本例中，我们导航到一个网页
   driver.get(url)
   ```

   

3. ​     **请求浏览器信息**

   ```python
   #您可以请求一系列关于浏览器的信息，包括窗口句柄、浏览器尺寸/位置、cookie、警报等
   title = driver.title
   ```

   

4. ​        **建立等待策略**

   ```python
   #将代码与浏览器的当前状态同步是Selenium面临的最大挑战之一，做好它是一个高级主题。
   #基本上，您希望在尝试定位元素之前，确保该元素位于页面上，并且在尝试与该元素交互之前，该元素处于可交互状态
   #隐式等待很少是最好的解决方案，但在这里最容易演示，所有我们将使用它作为占位符
   driver.implicitly_wait(0.5)
   ```

   

5. ​       **发送命令查找元素**

   ```python
   #大多数Selenium会话中主要命令都与元素相关，如果不先找到元素，就无法与之交互
   text_box = driver.find_element(by=By.NAME, Value="my-text")
   submit_button = driver.find_element(by=By.CSS_SELECTOR, value="button")
   ```

   

6. ​       **操作元素**

   ```python
   #对于一个元素，只有少数几个操作可以执行，但您将经常使用它们
   text_box.send_keys("Selenium")
   submit_button.click()
   ```

   

7. ​       **获取元素信息**

   ```python
   #元素存储了很多被请求的信息
   value = message.text
   ```

   

8. ​       **结束会话**

   ```python
   #这将结束驱动程序进程，默认情况下，该进程也会关闭浏览器。无法向此驱动程序实例发送更多命令
   driver.quit()
   ```

   ##### **组合所有事情**

   ```python
   from selenium import webdriver
   from selenium.webdriver.common.by import By
   import time
   
   def test_eight_components()
   	#创建谷歌浏览器对象
   	driver = webdriver.Chrome()
       #打开web测试网站
   	driver.get(url)
       #窗口最大化
       driver.maximize_window()
       #请求浏览器信息
       title = driver.title
       assert title == "Web form"
       #建立等待策略
       driver.implicitly_wait(0.5)
       #搜索定位网页元素
       text_box = driver.find_element(by=By.NAME, value="my-text")
       submit_button = driver.find_element(by=By.CSS_SELECTOR, value="button")
       #对定位的网页元素进行交互
       text_box.send_keys("Selenium")
       submit_button.click()
       message = driver.find_element(by=By.ID, value="message")
       value = message.text
       assert value == "Received!"
       #延迟
       time.sleep(10)
       #关闭浏览器
       driver.quit()
   
   ```


##### 1.3	元素选择策略

在webDriver中有8种不同的内置元素定位策略：

![image-20230815164033500](D:\AppData\Typora\typora-user-images\image-20230815164033500.png)

###### 下拉框定位使用

导包：

```python
from selenium.webdriver.support.select import Select
```

使用：

```
Select(WebElement).select_by_index(int)
```

##### 1.4	Actions接口

用于向Web浏览器提供虚化设备输入操作的低级接口

```python
clickable = driver.find_element(By.ID, "clickable")
    ActionChains(driver)\
        .move_to_element(clickable)\
        .pause(1)\
        .click_and_hold()\
        .pause(1)\
        .send_keys("abc")\
        .perform()
```

###### 释放所有Actions

需要注意的重要一点是, 驱动程序会记住整个会话中所有输入项的状态. 即使创建actions类的新实例, 按下的键和指针的位置 也将处于以前执行的操作离开它们的任何状态.

有一种特殊的方法来释放所有当前按下的键和指针按钮. 此方法在每种语言中的实现方式不同, 因为它不会使用perform方法执行.

```python
ActionBuilder(driver).clear_actions()
```

##### 1.5	等待

所有导航命令都等待特定值 基于[页面加载策略](https://www.selenium.dev/zh-cn/documentation/webdriver/drivers/options/#pageloadstrategy)（ 在驱动程序将控制权返回给代码之前，等待的默认值为 ）。 唯一关心的是加载 HTML 中定义的资产， 但是加载的 JavaScript 资产通常会导致网站发生变化， 并且需要与之交互的元素可能尚未出现在页面上 当代码准备好执行下一个 Selenium 命令时。`readyState``"complete"``readyState`

同样，在许多单页应用程序中，元素会动态获取 添加到页面或根据点击更改公开范围。 元素必须同时存在并[显示在](https://www.selenium.dev/zh-cn/documentation/webdriver/elements/information/#is-displayed)页面上 为了让硒与它相互作用。

**隐式等待**

Selenium有一种内置的方式来自动等待称为*隐式等待*的元素。 可以使用浏览器选项中的[超时](https://www.selenium.dev/zh-cn/documentation/webdriver/drivers/options/#timeouts)功能或使用驱动程序方法（如下所示）设置隐式等待值。

这是一个全局设置，适用于整个会话的每个元素位置调用。 默认值为 ，这意味着如果未找到该元素，它将 立即返回错误。如果设置了隐式等待，驱动程序将等待 返回错误之前所提供值的持续时间。请注意，只要 元素定位，驱动程序将返回元素引用，代码将继续执行， 因此，较大的隐式等待值不一定会增加会话的持续时间。`0`

*警告：*不要混合隐式和显式等待。 这样做可能会导致不可预测的等待时间。 例如，将隐式等待设置为 10 秒 并显式等待 15 秒 可能会导致 20 秒后发生超时。

使用隐式等待解决我们的示例如下所示：

```python
 driver.implicitly_wait(2)
    driver.get('https://www.selenium.dev/selenium/web/dynamic.html')
    driver.find_element(By.ID, "adder").click()

    added = driver.find_element(By.ID, "box0")
```

**显示等待**

*显式等待*是添加到轮询应用程序的代码中的循环 使特定条件在退出循环之前评估为 true，并且 继续执行代码中的下一个命令。如果在指定的超时值之前不满足条件， 该代码将给出超时错误。由于应用程序有多种方法不处于所需状态， 因此，显式等待是指定要等待的确切条件的绝佳选择 在每个地方都需要它。 另一个不错的功能是，默认情况下，Selenium Wait 类会自动等待指定的元素存在。

```python
    revealed = driver.find_element(By.ID, "revealed")
    wait = WebDriverWait(driver, timeout=2)

    driver.find_element(By.ID, "reveal").click()
    wait.until(lambda d : revealed.is_displayed())

    revealed.send_keys("Displayed")
```

**定制**

可以使用各种参数实例化 Wait 类，这些参数将更改条件的计算方式。

这可能包括：

- 更改代码的计算频率（轮询间隔）
- 指定应自动处理哪些异常
- 更改总超时长度
- 自定义超时消息

例如，如果默认情况下重试*元素不可交互*错误，那么我们可以 在要执行的代码中对方法添加一个操作（我们只需要 确保代码在成功时返回）：`true`

```python
    revealed = driver.find_element(By.ID, "revealed")
    errors = [NoSuchElementException, ElementNotInteractableException]
    wait = WebDriverWait(driver, timeout=2, poll_frequency=.2, ignored_exceptions=errors)

    driver.find_element(By.ID, "reveal").click()
    wait.until(lambda d : revealed.send_keys("Displayed") or True)
```

本篇文章来源于：[博主繁花似锦](https://www.cnblogs.com/liuyuelinfighting/p/14900019.html)
