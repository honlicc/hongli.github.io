---
title: Python GUI编程(Tkinter)
date: 2022-03-22 11:06:17
categories: 
- python
tags:
- GUI
---
---

### 参考文档
[点我->Tkinter官方文档地址](https://docs.python.org/zh-cn/3.10/library/tk.html)
[点我->菜鸟编程](https://www.runoob.com/python/python-gui-tkinter.html)


## 目录
* [简介](#简介)
* [第一个Tkinter程序](#第一个Tkinter程序)
* [主窗口设置](#主窗口设置)
* [通过类来实现GUI程序](#通过类来实现GUI程序)
* [常用控件列表](#Tkinter中常用的15个控件)
* [Button控件](#Button控件)
* [Label控件](#Label控件)

### 简介
GUI编程：图形用户界面编程
Tkinter库：是 Python 的标准 GUI 库。Python 使用 Tkinter 可以快速的创建 GUI 应用程序。支持跨平台的GUI程序开发，适合小型的GUI编程，也是特别适合初学者学习的GUI编程。


### 第一个Tkinter程序
tkinter模块创建GUI程序主要包含以下4个步骤：
1.创建主窗口程序:
```sh
from tkinter import *
root = Tk()
```

2.在主窗口中添加各种组件:
文本框(Label)、按钮(Button)等。
```sh
btn=Button(root)
```

3.通过布局管理器，来管理组件的位置、大小。
```sh
btn.pack()
```

4.事件处理:
* 通过绑定事件处理程序，响应用户操作(单击、双击等)
* 调用组件的mainloop,进入事件循环


示例代码：
test01.py
```sh
from tkinter import *
from tkinter import messagebox

# root：主窗口
root = Tk()

# 组件 button
btn = Button(root)
btn["text"] = "hello word"

# 布局管理器
btn.pack()


# 事件  e:代表事件对象
def test(e):
    messagebox.showinfo("Message", "这是一个按钮")
    print("点击按钮")


# 事件绑定
btn.bind("<Button-1>", test)

# 调用组件的mainloop,进入事件循环。
root.mainloop()
```

### 主窗口设置
主窗口的大小、位置等，通过geometry("wxh ± x ± y")进行设置，w表示宽度，h表示高度.  (注：宽x高,注,此处不能为 "*",必须使用 "x")
+x表示距屏幕左边的距离，-x表示距屏幕右边的距离。
+y表示距屏幕上边的距离，-h表示距屏幕下边的距离。

示例代码：
test02.py
```sh
from tkinter import *
from tkinter import messagebox

# root：主窗口
root = Tk()

# 设置程序窗口title
root.title("第一个GUI程序")
# 设置窗口大小以及在屏幕上的位置 ，宽x高,注,此处不能为 "*",必须使用 "x"
root.geometry("600x400+400+200")
# 更改左上角窗口的的icon图标
root.iconbitmap('C:/Users/Administrator/Desktop/test.ico')
# 设置主窗口的背景颜色,颜色值可以是英文单词，或者颜色值的16进制数
root["background"] = "#C9C9C9"

# 组件 button
btn = Button(root)
btn["text"] = "hello word"

# 布局管理器
btn.pack()


# 事件  e:代表事件对象
def test(e):
    messagebox.showinfo("Message", "这是一个按钮")
    print("点击按钮")


# 事件绑定
btn.bind("<Button-1>", test)

# 调用组件的mainloop,进入事件循环。
root.mainloop()
```

### 通过类来实现GUI程序
通过Application组织整个GUI程序，类Application继承自Farme，通过继承拥有了父类的特性，通过构造函数初始化窗口中的对象，通过 createWidget()方法创建窗口中的对象。
Frame 是框架控件,表示一个矩形区域。一般作为容器使用。

我们重写一下上面的代码
示例代码：
test03.py
```sh
from tkinter import *
from tkinter import messagebox


class Applicaton(Frame):

    def __init__(self, master=None):
        # super代表的是父类的定义，而不是父类对象
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()

   
    def createWidget(self):
        self.btn = Button(self)
        self.btn["text"] = "hello word"
        self.btn.pack()

        # command 绑定事件，功能同bind
        self.btn["command"] = self.test

    def test(self):
        messagebox.showinfo("Message", "这是一个按钮")
        print("点击按钮")


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x330+200+300")
    app = Applicaton(master=root)
    root.mainloop()
```
---

### Tkinter中常用的15个控件

| 控件   | 描述|
|:-----|:----|
|Button|按钮控件：用于增加各种按钮|
|Canvas|画布控件：用于在窗口上绘制图形，显示图形元素如线条或文本。|
|Checkbutton|多选框：用于显示多选框的工具|
|Entry|输入控件：显示单行文本域。它一般用于接受用户值|
|Frame|框架控件：在屏幕上显示一个矩形区域，多用来作为容器， 另外的窗件可以加入进来|
|Label|标签控件；可以显示文本和位图|
|Listbox|列表框控件；在Listbox窗口小部件是用来显示一个字符串列表给用户|
|Menubutton|菜单按钮控件，用于显示菜单项。|
|Menu|菜单控件；显示菜单栏,下拉菜单和弹出菜单|
|Message|消息控件；用来显示多行文本，与label比较类似|
|Radiobutton|单选按钮控件；显示一个单选的按钮状态|
|Scale|范围控件；显示一个数值刻度，为输出限定范围的数字区间|
|Scrollbar|滚动条控件，当内容超过可视化区域时使用，如列表框|
|Text|文本控件；用于显示多行文本|
|Toplevel|容器控件；用来提供一个单独的对话框，和Frame比较类似|
|Spinbox|输入控件；与Entry类似，但是可以指定输入范围值|
|PanedWindow|PanedWindow是一个窗口布局管理的插件，可以包含一个或者多个子控件|
|LabelFrame|labelframe 是一个简单的容器控件。常用与复杂的窗口布局|
|tkMessageBox|用于显示你应用程序的消息框|

---
### Button控件:
Button: 用于增加各种按钮

语法格式：
```sh
btn=Button(master, option, ...)
```
* master:父容器
* option:可选项，即该按钮可设置的属性，通过key-value形式设置,以逗号分隔。

option说明：

|可选项|描述|
|:--|:--|
|activebackground|当鼠标放上去时，按钮的背景色|
|activeforeground|当鼠标放上去时，按钮的前景色|
|bd|按钮边框的大小，默认为 2 个像素|
|bg|按钮的背景色|
|command|按钮关联的函数，当按钮被点击时，执行该函数|
|fg|按钮的前景色（按钮文本的颜色）|
|font|文本字体|
|height|按钮的高度|
|highlightcolor|要高亮的颜色|
|image|按钮上要显示的图片|
|justify|显示多行文本的时候,设置不同行之间的对齐方式，可选项包括LEFT, RIGHT, CENTER|
|padx|按钮在x轴方向上的内边距(padding)，是指按钮的内容与按钮边缘的距离|
|pady|按钮在y轴方向上的内边距(padding)|
|relief|边框样式，设置控件3D效果，可选的有：FLAT、SUNKEN、RAISED、GROOVE、RIDGE。默认为 FLAT。|
|state|设置按钮组件状态,可选的有NORMAL、ACTIVE、 DISABLED。默认 NORMAL。|
|underline|下划线。默认按钮上的文本都不带下划线。取值就是带下划线的字符串索引，为 0 时，第一个字符带下划线，为 1 时，前两个字符带下划线，以此类推|
|width|按钮的宽度，如未设置此项，其大小以适应按钮的内容（文本或图片的大小）|
|wraplength|限制按钮每行显示的字符的数量|
|text|按钮的文本内容|
|anchor|锚选项，控制文本的位置，默认为中心|


anchor:控制文本的显示位置,示意图如下：
![](https://s2.loli.net/2022/03/24/5bBOQEr9jnCoivF.png)

示例代码:
testButton.py
```sh
from tkinter import *
from tkinter import messagebox


class Applicaton(Frame):

    def __init__(self, master=None):
        # super代表的是父类的定义，而不是父类对象
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()

    def createWidget(self):
        self.btn = Button(self, text="登录", command=self.login)
        self.btn.pack()

        # 注: PhotoImage 只能添加gif
        global img
        img = PhotoImage(file="c.gif")
        self.btn02 = Button(self, image=img, command=self.login)
        self.btn02.pack()

        # anchor 文字在button中的位置
        self.btn03 = Button(self, text="登录2", command=self.login, anchor=N,height=10,width=20)
        self.btn03.pack()

    def login(self):
        messagebox.showinfo("Message", "登录成功！")


if __name__ == '__main__':
    root = Tk()
    root.geometry("800x530+200+300")
    app = Applicaton(master=root)
    root.mainloop()

```
---


### Label控件:
Label：主要用于显示文本信息，也可以显示图像信息

语法格式：
```sh
lab=Label(master, option, ...)
```
* master:父容器
* option:可选项，即该标签可设置的属性，通过key-value形式设置,以逗号分隔。

option说明：

|可选项|描述|
|:--|:--|
|anchor|文本或图像在背景内容区的位置，默认为 center，可选值为（n,s,w,e,ne,nw,sw,se,center）eswn 是东南西北英文的首字母，表示：上北下南左西右东。|
|bg|标签背景颜色|
|bd|标签的大小，默认为 2 个像素|
|bitmap|指定标签上的位图，如果指定了图片，则该选项忽略|
|cursor|鼠标移动到标签时，光标的形状，可以设置为 arrow, circle, cross, plus 等。|
|font|设置字体。|
|fg|设置前景色。|
|height|标签的高度，默认值是 0。|
|image|设置标签图像。|
|justify|定义对齐方式，可选值有：LEFT,RIGHT,CENTER，默认为 CENTER。|
|padx|x 轴间距，以像素计，默认 1。|
|pady|y 轴间距，以像素计，默认 1。|
|relief|边框样式，可选的有：FLAT、SUNKEN、RAISED、GROOVE、RIDGE、SOLID。默认为 FLAT。|
|text|设置文本，可以包含换行符(\n)。|
|textvariable|标签显示 Tkinter 变量，StringVar。如果变量被修改，标签文本将自动更新。|
|underline|设置下划线，默认 -1，如果设置 1，则是从第二个字符开始画下划线。|
|width|设置标签宽度，默认值是 0，自动计算，单位以像素计。|
|wraplength|设置标签文本为多少行显示，默认为 0。|

Label控件构成
一个控件主要由背景和前景两部分组成。其中背景由三部分构成分别是内容区域、填充区、边框，这三个区域的大小通过以下属性进行控制，如下所示：
* width/height
* padx/pady
* borderwidth
![](https://s2.loli.net/2022/03/24/Ex25UOJPwoSXFuh.png)

示例代码:
test_Label.py
```sh
from tkinter import *
from tkinter import messagebox

class Applicaton(Frame):

    def __init__(self, master=None):
        # super代表的是父类的定义，而不是父类对象
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()

    def createWidget(self):
        self.label = Label(self, text="这是一个label", width=10, height=2, bg="black", fg="white")
        self.label.pack()

        # font:设置字体
        self.label02 = Label(self, text="这是一个label02", width=30, height=2, bg="black", fg="white", font=("黑体", 15))
        self.label02.pack()

        # image:显示图片，只支持gif(PNG貌似也可以)
        global photo  # 注：mainloop()属于循环执行，需要声明全局变量，否则为局部变量图片会消失
        photo = PhotoImage(file="c.gif")
        self.label03 = Label(self, text="这是一个label03", bg="black", fg="white", font=("黑体", 15), image=photo)
        self.label03.pack()

        # justify:对齐方式 可选值有 LEFT,RIGHT,CENTER，默认为 CENTER。
        self.label04 = Label(self, text="这是一个label04\n1234567890\nqwertyuio", borderwidth=1, relief="solid",
                             justify="right")
        self.label04.pack()


if __name__ == '__main__':
    root = Tk()
    root.geometry("600x530+200+300")
    app = Applicaton(master=root)
    root.mainloop()
```










