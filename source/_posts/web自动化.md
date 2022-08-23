---
title: Python Web自动化(selenium)
date: 2022-083-22 11:06:17 
categories:
- python 
tags:
- selenium

---
---

### 参考文档
### 注：本文借助pytest实现自动化case
[点我->官方文档](https://www.selenium.dev/)


## 目录

* [简介](#简介)
* [安装selenium库](#安装)
* [webDriver安装配置](#webDriver安装配置)
* [第一个selenium程序](#第一个selenium程序)
* [编写第一条测试用例](#编写第一条测试用例)
* [selenium三种等待方式](#selenium三种等待方式)
* [web控件定位与常见操作](#web控件定位与常见操作)
* [xpath定位](#xpath定位)
* [css定位](#css定位)
* [web控件的交互](#web控件的交互)
* [表单操作](#表单操作)
* [多窗口处理与网页frame](#多窗口处理与网页frame)
* [执行JavaScript脚本](#执行JavaScript脚本)
* [弹窗处理](#弹窗处理)
---

<br>

### 简介

用于web浏览器测试工具，支持的浏览器包括IE、Firefox、Safari、Chrome等。
使用简单，可以使用Java、Python等多种语言编写。
主要由三个工具构成：webdriver/ide/Grid。

<br>

### 安装

安装selenium库
``` 
pip --default-timeout=100 install selenium==3.141.0 -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
```

<br>

### webDriver安装配置

Driver介绍:
``` 
https://www.selenium.dev/zh-cn/documentation/webdriver/
```
<br>

Driver下载：
``` 
官方地址：
https://chromedriver.chromium.org/downloads

淘宝镜像（推荐）：
https://registry.npmmirror.com/binary.html?path=chromedriver/
```
<br>

### 第一个selenium程序



新建test_selenium.py文件，复制如下代码：
```sh
from selenium import webdriver
import time


def test_selenium():
    driver = webdriver.Chrome()
    driver.get("http://www.baidu.com")
    time.sleep(3)
    driver.quit()
```

执行：
``` 
pytest -vs .\test_demo.py 
```
说明：可以正常打开Chrome浏览器并访问百度及证明环境搭建完成。

<br>

### 编写第一条测试用例

测试用例的核心要素：

* 一条测试用例的最终结果只有一个，成功or失败。
* 三大核心要素为：标题、步骤、断言
  * 标题：是对测试用例的描述。
  * 步骤：对测试执行过程进行描述。
  * 断言：实际结果与预期结果对比。

新建test_search.py文件，复制如下代码：
``` 
from selenium import webdriver


def test_search():
    driver = webdriver.Chrome()
    driver.implicitly_wait(5)  # 隐式等待

    # 打开百度首页
    driver.get("http://www.baidu.com")

    # 搜索框输入selenium关键字
    driver.find_element_by_css_selector("#kw").send_keys("selenium")

    # 点击搜索
    driver.find_element_by_css_selector("#su").click()

    # 获取搜索结果
    result = driver.find_element_by_xpath("//*[@aria-label='selenium，WEB自动化工具，百度百科']").text

    # 断言
    assert result == "Selenium(WEB自动化工具) - 百度百科"
    
    driver.quit()
```

执行：
``` 
pytest -vs test_search.py
```

输出：
``` 
collected 1 item                                                                                                                                                                                                                          

test_search.py::test_search
DevTools listening on ws://127.0.0.1:4553/devtools/browser/2b223324-fc59-4739-ba32-c39188bab609
PASSED

=========================================================================================================== 1 passed in 3.37s ============================================================================================================

```
说明：能够成功打开浏览器并搜索selenium。

<br>

### selenium三种等待方式

* 直接等待：

强制等待，休眠一定时间。

``` 
time.sleep()
```

* 隐式等待：

设置一个等待时间，轮训查找(默认间隔0.5s)元素是否出现，如果时间内没有出现则抛出异常。
为全局设置
``` 
driver.implicitly_wait(5)  
```

* 显示等待：

在代码中定义等待条件，当条件发生时才继续执行代码。
'WebDriverWait'配合until()或until_not()方法，根据判断条件进行等待。
程序每隔一段时间(默认0.5s)进行条件判断，如果条件成立，则执行下一步，否则继续等待，直到超过设置的最长时间。

<br>

### web控件定位与常见操作

selenium的点击与输入
``` 
driver.find_element(By.ID,"kw").send_keys("selenium")
driver.find_element(By.ID,'su').click()
```
<br>

#### xpath定位


| 表达式                               | 描述                                        |
|:----------------------------------|:------------------------------------------|
| /booksotre/book[1]                | 选取booksotre子元素的第一个book元素                  |
| /booksotre/book[last()]           | 选取booksotre子元素的倒数第一个book元素                |
| /booksotre/book[lst()-1]          | 选取booksotre子元素的倒数第二个book元素                |
| /booksotre/book[position()<1]     | 选取最前面的两个属于booksotre子元素的book元素             |
| //title[@lang="eng"]              | 选取所有title元素，且这些元素拥有值为eng的lang属性           |
| /booksotre/book[price()>35]       | 选取booksotre元素所有book元素，且其中的price元素值必须大于35  |
| /booksotre/book[price()>35]/title | 选取booksotre元素所有title元素，且其中的price元素值必须大于35 |

---

| 表达式    | 描述                           |
|:---------|:-----------------------------|
| nodename | 选取此节点的所有子节点                  |
| /        | 从根节点选取                       |
| //       | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置 |
| .        | 选取当前节点                       |
| ..       | 选取当前节点的父节点                   |
| @        | 选取属性                         |

<br>

#### css定位

| 表达式      | 示例 |描述|
|:---------|:--|:----|
| .class  | .intro |选择class="intro"的所有元素|
| #id  | #firstname |选择id="firstname"的所有元素|
| * | * |选择所有元素|
| element | p |选择所有<p>标签元素|
| element,element | div,p |选择所有<div>标签元素和所有<p>标签元素|
| element element | div p |选择所有<div>标签元素内的<p>标签元素|
| element>element | div>p |选择父元素为<div>标签元素的<p>标签元素|
| element+element | div+p |选择紧接在<div>标签元素之后的<p>标签元素|

注意：
css selector定位是非常核心的定位方式，其它定位比如 id定位，name定位等，最终都是通过 css selector来完成的。
下面贴一段webdriver中 find_element方法的源码：
```
    def find_element(self, by=By.ID, value=None):
        """
        Find an element given a By strategy and locator. Prefer the find_element_by_* methods when
        possible.

        :Usage:
            element = driver.find_element(By.ID, 'foo')

        :rtype: WebElement
        """
        if self.w3c:
            if by == By.ID:
                by = By.CSS_SELECTOR
                value = '[id="%s"]' % value
            elif by == By.TAG_NAME:
                by = By.CSS_SELECTOR
            elif by == By.CLASS_NAME:
                by = By.CSS_SELECTOR
                value = ".%s" % value
            elif by == By.NAME:
                by = By.CSS_SELECTOR
                value = '[name="%s"]' % value
        return self.execute(Command.FIND_ELEMENT, {
            'using': by,
            'value': value})['value']
 
```

---

<br>

### web控件的交互

常用的操作事件(右键点击，页面滑动，表单操作等等)

Actions:

* ActionChains:执行PC端的鼠标点击，双击，右键，拖拽等事件。
* TouchActions:模拟PC和移动端的点击，滑动，拖拽，多点触控等多种手势操作。

<br>

ActionChains：

执行原理：
调用ActionChains方法时，不会立即执行，而是将所有的操作按顺序存放在一个队列中，当调用perform()方法时，队列中的事件会依次执行。

基本用法：
生成一个动作action=ActionChains(driver)
添加方法1： action.方法1
添加方法2： action.方法2
...
...
调用perform()方法执行(action.perform())


具体写法：

链式写法：
```
ActionChains(driver).move_to_element(element).click(element).perform()
```
<br>

分布写法：
``` 
action=ActopnChains(driver)
action.move_to_element(element)
action.clcik(element)
action.perform()
```

示例1：点击，双击,右键
``` 
action=ActionChains(driver)
action.click(element)
action.double_click(element)
action.context_click(element)
action.perform()
```

<br>

新建test_ActionChains.py文件，复制如下代码:
``` 
import pytest
from selenium import webdriver
from selenium.webdriver import ActionChains
import time


class TestActionChains():

    def setup(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(5)
        self.driver.maximize_window()

    def teardown(self):
        self.driver.quit()

    def test_case_click(self):
        self.driver.get("http://sahitest.com/demo/clicks.htm")

        # 通过 ActionChains来实现对元素的点击，右键，双击操作
        element_click = self.driver.find_element_by_xpath('//input[@value="click me"]')
        element_dbl_click = self.driver.find_element_by_xpath('//input[@value="dbl click me"]')
        element_right_click = self.driver.find_element_by_xpath('//input[@value="right click me"]')

        action = ActionChains(self.driver)
        action.click(element_click)
        action.double_click(element_dbl_click)
        action.context_click(element_right_click)

        action.perform()

        time.sleep(5)
```

<br>

执行case：
```html
pytest -vs test_ActionChains.py::TestActionChains::test_case_click
```

<br>

示例2：鼠标移动到某个元素上
``` 
action=ActionChains(driver)
action.move_to_element(element)
action.perform()
```

打开上面的test_ActionChains.py文件，添加如下代码:
```html
    def test_case_move_to(self):
        # 百度首页，将鼠标移动到"更多"按钮上
        self.driver.get("http://www.baidu.com")
        element_setting = self.driver.find_element_by_link_text("更多")
        action = ActionChains(self.driver)
        action.move_to_element(element_setting)
        action.perform()

        time.sleep(3)
```

执行case:
```html
pytest -vs test_ActionChains.py::TestActionChains::test_case_move_to
```

<br>

示例3：将一个元素拖拽到另一个元素

有三种方法实现：
``` 
action=ActionChains(driver)

# 方式1：
action.drag_and_drop(element1,element2)

# 方式2：
action.click_and_hold().release(element_drop)

# 方式3：
action.click_and_hold(element_drag).move_to_element(element_drop).release()

action.perform()
```

打开上面的test_ActionChains.py文件，添加如下代码:
```html
     def test_case_drag_drop(self):
            # 将元素拖拽到另一个元素
            self.driver.get("https://sahitest.com/demo/dragDropMooTools.htm")
            element_drag = self.driver.find_element_by_id("dragger")
            element_drop = self.driver.find_element_by_xpath("/html/body/div[2]")
            action = ActionChains(self.driver)
    
            # 方式1：
            action.drag_and_drop(element_drag, element_drop)
    
            # 方式2：
            # action.click_and_hold(element_drag).release(element_drop)
    
            # 方式3：
            # action.click_and_hold(element_drag).move_to_element(element_drop).release()
    
    
            action.perform()
            
            time.sleep(3)

```

执行case:
```html
pytest -vs test_ActionChains.py::TestActionChains::test_case_drag_drop
```

<br>

示例4：ActionChains模拟按键方法
模拟按键有多种方法，能够用win32API来实现，能用SendKeys()来实现，也可以用selenium的WebElement对象的
send_keys()方法来实现，ActionChains类也提供了几个模拟按键的方法。

```
action=ActionChains(driver)
action_send_keys(Keys.BACK_SPACE)
或者 action.key_down(Keys.CONTROL).send_keys("a").key_up(Keys.CONTROL)
action.perform()
```


打开上面的test_ActionChains.py文件，添加如下代码:
```html
    def test_case_keys(self):
        self.driver.get("https://sahitest.com/demo/label.htm")
        ele = self.driver.find_element_by_xpath("//label/input[@type='textbox']")
        ele.click()

        action = ActionChains(self.driver)
        action.send_keys("username").pause(1)
        action.send_keys(Keys.SPACE).pause(1)
        action.send_keys("tom").pause(1)
        action.send_keys(Keys.BACK_SPACE)
        action.perform()
```

执行case:
```html
pytest -vs test_ActionChains.py::TestActionChains::test_case_keys
```

TouchActions:

类似于ActionChains,ActionChains只针对于PC端程序鼠标模拟的一系列操作，对H5页面操作时无效，TouchAction可以对h5进行操作，通过
TouchAction可以实现点击、滑动、拖拽、多点触控、以及模拟手势的各种操作。

手势控制：
* tap--在指定元素上敲击。
* double_tap--在指定元素上双敲击。
* tap_and_hold--在指定元素上点击但不释放。
* move--手势移动指定偏移(未释放)。
* release--释放手势。
* scroll--手势点击并滚动。
* scroll_form_element--从某个元素位置开始手势点击并滚动(向下滚动为负数，向上滑动为正数)。
* long_press--长按元素。
* flick--手势滑动。
* flick_element--从某个元素位置开始手势滑动(向上滑动为负数，向下滑动为正数)。
* preform--执行。


新建 test_touchAction.py文件，复制如下代码
```html
import pytest
from selenium import webdriver
from selenium.webdriver import TouchActions
from selenium.webdriver.common.keys import Keys
import time


class TestTouchAciton():

    def setup(self):
        option = webdriver.ChromeOptions()
        option.add_experimental_option("w3c", False)

        self.driver = webdriver.Chrome(options=option)
        self.driver.implicitly_wait(5)
        self.driver.maximize_window()

    def teardown(self):
        self.driver.quit()

    def test_case_click(self):
        """
        1:打开Chrome浏览器
        2:打开百度首页
        3：输入selenium
        4：点击搜索按钮
        5：滑动到底部
        :return:
        """

        self.driver.get("http://www.baidu.com")

        element_input = self.driver.find_element_by_id('kw')
        element_search = self.driver.find_element_by_id('su')

        element_input.send_keys("selenium测试")
        action = TouchActions(self.driver)
        action.tap(element_search)

        action.perform()

        action.scroll_from_element(element_input, 0, 10000).perform()


```

执行case：
```html
pytest -vs test_touchAction.py::TestTouchAciton::test_case_click
```
<br>

### 表单操作

表单是一个包含表单元素的区域。
表单元素是允许用户在表单中输入信息的元素。
表单使用表单标签```<form>```定义。

操作表单步骤：
首先要定位到表单元素。
然后去操作元素(清空，输入或者点击等)。

例如登录功能等，同其它元素定位操作相同，这里不再过多介绍。

<br>

### 多窗口处理与网页frame

selenium处理多窗口场景

* 多窗口识别
* 多窗口之间切换

多窗口处理：
点击某链接，会重新打开一个窗口，对于这种情况，想在新页面上操作，就得先切换窗口了。
获取窗口的唯一标识用句柄标识，所以只需要切换句柄，就可以在多个页面灵活操作了。

处理流程：
* 先获取到当前窗口的句柄```driver.current_window_handle```
* 在获取到所有的窗口的句柄```driver.window_handles```
* 判断释放是想要操作的窗口，如果是就直接进行操作，如果不是，就切换到想要操作的窗口```driver.switch_to.window```

新建test_windows.py文件，复制如下代码：
```html
import pytest
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.keys import Keys
import time


class TestWindows():

    def setup(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(5)
        self.driver.maximize_window()

    def teardown(self):
        self.driver.quit()

    def test_case_login_window_check(self):
        '''
        打开百度首页，点击登录按钮
        展示登录弹窗，点击注册按钮
        跳转到注册页面
        输入用户名
        :return:
        '''
        self.driver.get("https://www.baidu.com")

        self.driver.find_element_by_link_text("登录").click()
        self.driver.find_element_by_link_text("立即注册").click()

        print(self.driver.current_window_handle)
        print(self.driver.window_handles)

        windows = self.driver.window_handles

        self.driver.switch_to.window(windows[-1])

        self.driver.find_element_by_id("TANGRAM__PSP_4__userName").send_keys("username")

        time.sleep(5)

```

执行case:
```html
pytest -vs test_windows.py::TestWindows::test_case_login_window_check
```

selenium处理frame

* 多个frame识别
* 多个frame切换

frame介绍：
在web自动化中，如果一个元素定位不到，那么很大可能是在iframe中。

frame是html中的框架，在html中，所谓的框架就是可以在同一个浏览器中显示不止一个页面。
基于html框架，又分为垂直框架和水平框架(cols,rows)。

Frame分类：
Frame标签包含frameset,frame,iframe三种。
frameset和普通标签一样，不会影响正常的定位，可以使用index/id/name/webelement任意种方式定位frame。
而frame与iffame妒忌selenium而言定位是一样的，selenium有一组方法对frame进行操作。

frame切换：
frame存在两种情况：
一种是嵌套的，一种是未嵌套的。

切换frame:
```
driver.switch_to.frame() # 根据元素id或者index切换frame
driver.switch_to.defalut_content() # 切换到默认的frame
driver.switch_to.parent_frame() # 切换到父级frame
```

处理未嵌套的iframe:

```html
driver.switch_to_frame("frame 的 id")
driver.switch_to_frame("frame-index") # frame无id时可以同通过index索引来处理，索引从0开始(driver.switch_to_frame(0))
```

处理嵌套的iframe:
对于嵌套的先进入到父级节点，再进到子节点，然后在对子节点里的对象进行处理操作。
```html

driver.switch_to.frame("父节点")
driver.switch_to.frame("子节点")
```

新建test_frame.py文件，复制如下代码：
```html
import pytest
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.keys import Keys
import time


class TestFrame():

    def setup(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(5)
        self.driver.maximize_window()

    def teardown(self):
        self.driver.quit()

    def test_case_frame(self):
        '''
        打开百度首页，点击登录按钮
        展示登录弹窗，点击注册按钮
        跳转到注册页面
        输入用户名
        :return:
        '''
        self.driver.get("https://www.runoob.com/try/try.php?filename=jqueryui-api-droppable")

        # 切换到frame子节点
        self.driver.switch_to.frame("iframeResult")

        print(self.driver.find_element_by_id("draggable").text)

        # 切换回父节点或切换回默认节点
        # self.driver.switch_to.parent_frame()
        self.driver.switch_to.default_content()
```

执行case：
```html
pytest -vs test_frame.py::TestFrame::test_case_frame
```

<br>

### 执行JavaScript脚本

selenium执行js:

execute_script:执行js
return:返回js执行结果

新建test_js.py,复制如下代码：
```html
import pytest
from selenium import webdriver

import time


class TestJs():

    def setup(self):
        option = webdriver.ChromeOptions()
        option.add_experimental_option("w3c", False)

        self.driver = webdriver.Chrome(options=option)
        self.driver.implicitly_wait(5)
        self.driver.maximize_window()

    def teardown(self):
        self.driver.quit()

    def test_case_js_scroll(self):
        """
        1:打开Chrome浏览器
        2:打开百度首页
        3：输入selenium
        4：点击搜索按钮
        5：滑动到底部
        :return:
        """

        self.driver.get("http://www.baidu.com")

        self.driver.find_element_by_id('kw').send_keys("selenium测试")
        self.driver.find_element_by_id('su').click()

        time.sleep(2)
        self.driver.execute_script("document.documentElement.scrollTop=10000")
        time.sleep(5)
```

执行case：
```html
pytest -vs test_js.py::TestJs::test_case_js_scroll
```

<br>

### 弹窗处理

在操作页面过程中有时会遇见JavaScript所生成的alert/confirm/prompt等弹框，可以使用switch_to.alert()方法定位，
然后使用text/accept/dismiss/send_keys等方法进行操作

alert常用方法：
```html
switch_to.alert():获取当前页面上的警告弹窗
text:返回alert/confirm/prompt中的文字信息
accept():接受现有警告框
dismiss():解散现有警告框
send_keys(KeyToSend):发送文本至警告框，keysToSend:将文本发送至警告框
```


# 持续更新ing~