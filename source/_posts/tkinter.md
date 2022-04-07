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
* [Options参数设置](#Options参数设置方式)
* [Entry单行文本框](#Entry单行文本框)
* [Text多行文本框](#Text多行文本框)
* [Raduibutton单选框](#Raduibutton单选框)
* [CheckButton多选框](#CheckButton多选框)
* [Canvas画布](#Canvas画布)
* [Grid布局管理器](#Grid布局管理器)
* [使用Grid实现计算器页面布局](#使用Grid实现计算器页面布局)
* [Pack布局管理器](#Pack布局管理器)
* [Place布局管理器](#Place布局管理器)


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
---
### Options参数设置方式
通过上面对Button、Label控件的学习，我们发现可以通过Options设置控件的属性，我们可以通过三种方式设置Options选项，这在其它GUI组件中用法一致。

1.创建对象时，使用关键字参数(key=value)
```sh
btn01=Button(master,fg="red",bg="black")
```

2.创建对象后，使用字典索引方式
```sh
btn02=Button(master)
btn02["fg"]="red"
btn02["bg"]="black"
```

3.创建对象后，使用config方式
```sh
btn03=Button(master)
btn03.config(fg="red",bg="black")
```

示例代码:
test_Options.py
```sh
import tkinter as tk
from tkinter import *
from tkinter import messagebox

class Application(tk.Frame):
    """GUI程序类"""

    def __init__(self, master=None):
        super().__init__(master)  # super()代表父类的定义
        self.master = master
        self.pack()
        self.createWidget()

    def createWidget(self):
        """添加组件"""

        # 关键字参数
        fred01=Button(self,fg='red',bg="blue")
        fred01.pack()

        # 创建对象后，使用字典索引方式
        fred02=Button(self)
        fred02["fg"]="red"
        fred02["bg"]="blue"
        fred02.pack

        # 创建对象后，使用config()方法
        fred03 = Button(self)
        fred03.config(fg='red',bg='blue')
        fred03.pack()

if __name__ == '__main__':

    root = tk.Tk()
    root.geometry("800x900+800+300")
    root.title("GUI程序类测试")
    app = Application(master=root)
    root.mainloop()
```
---
### Entry单行文本框
Entry 是用来接收一行字符串的控件，如果用户输入的文字长度长于Entry控件的宽度，文字会自动向后滚动，如果想输入多行文本，需要使用Text控件。

语法格式：
```sh
lab=Label(master, option, ...)
```
* master:父容器
* options: 可选项，即该按钮的可设置的属性。这些选项可以用键 = 值的形式设置，并以逗号分隔。

option说明：

|可选项|描述|
|:--|:--|
|bg|输入框背景颜色|
|bd|边框的大小，默认为 2 个像素|
|cursor|光标的形状设定，如arrow, circle, cross, plus 等|
|font|文本字体|
|exportselection|默认情况下，你如果在输入框中选中文本，默认会复制到粘贴板，如果要忽略这个功能刻工艺设置 exportselection=0。|
|fg|文字颜色。值为颜色或为颜色代码，如：'red','#ff0000'|
|highlightcolor|文本框高亮边框颜色，当文本框获取焦点时显示|
|justify|显示多行文本的时候,设置不同行之间的对齐方式，可选项包括LEFT, RIGHT, CENTER|
|relief|边框样式，设置控件3D效果，可选的有：FLAT、SUNKEN、RAISED、GROOVE、RIDGE。默认为 FLAT。|
|selectbackground|选中文字的背景颜色|
|selectborderwidth|选中文字的背景边框宽度|
|selectforeground|选中文字的颜色|
|show|指定文本框内容显示为字符，值随意，满足字符即可。如密码可以将值设为 show="*"|
|state|默认为 state=NORMAL, 文框状态，分为只读和可写，值为：normal/disabled|
|textvariable|文本框的值，是一个StringVar()对象|
|width|文本框宽度|
|xscrollcommand|设置水平方向滚动条，一般在用户输入的文本框内容宽度大于文本框显示的宽度时使用。|

方法：
下表为文本框组件常用的方法

|可选项|描述|
|:--|:--|
|delete(first,last=None)|删除文本框里直接位置值|
|get()|获取文件框的值|
|set()|设置文件框的值|
|icursor(index)|将光标移动到指定索引位置，只有当文框获取焦点后成立|
|index(index)|返回指定的索引值|
|insert(index,s)|返回指定的索引值|
|select_adjust(index)|选中指定索引和光标所在位置之前的值|
|select_clear(index)|清空文本框|
|select_from(index)|设置光标的位置，通过索引值 index 来设置|
|select_present()|如果有选中，返回 true，否则返回 false。|
|select_range(start,end)|选中指定索引位置的值，start(包含) 为开始位置，end(不包含) 为结束位置start必须比end小|
|select_to(index)|选中指定索引与光标之间的值|
|xview(index)|该方法在文本框链接到水平滚动条上很有用。|
|xview_scroll(number,what)|用于水平滚动文本框。 what 参数可以是 UNITS, 按字符宽度滚动，或者可以是 PAGES, 按文本框组件块滚动。 number 参数，正数为由左到右滚动，负数为由右到左滚动。|

模拟获取用户名+密码完成登录功能的示例代码：
test_Entry_StringVar.py
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
        self.label = Label(self, text="用户名")
        self.label.pack()

        # 定义变量接收输入 StringVar变量关联到指定的组件
        # StringVar 变量的值发生变化 组件内容也发生变化
        # 同理，组件内容发生变化，StringVar 内容也发生变化
        v1 = StringVar()
        self.entry01 = Entry(self, textvariable=v1)
        self.entry01.pack()
        # 通过set给变量设置默认值
        v1.set("admin")

        # 通过get获取Entry输入的内容
        print(v1.get())
        print(self.entry01.get())

        self.label = Label(self, text="密码")
        self.label.pack()
        v2 = StringVar()

        # show="*"  表示输入内容的时候显示为 “*”
        self.entry02 = Entry(self, textvariable=v2, show="*")
        self.entry02.pack()

        Button(self, text="登录", command=self.login).pack()

    def login(self):
        get_user_name = self.entry01.get()
        get_pass_word = self.entry02.get()

        if get_user_name == "admin" and get_pass_word == "admin123":
            messagebox.showinfo("登录", "登录成功！")
        else:
            messagebox.showinfo("登录", "登录失败！")


if __name__ == '__main__':
    root = Tk()
    root.geometry("600x530+200+300")
    app = Applicaton(master=root)
    root.mainloop()
```


### Text多行文本框
Text(多行文本框)的主要用于显示多行文本，还可以显示网页链接、图片、HTML页面、甚至CSS样式表，添加组件等。因此，也常被当做简单的文本处理器，文本编辑器，或者网页浏览器来
使用

语法格式：
```sh
text=Text(master, option, ...)
```
* master:父容器
* options: 可选项，即该按钮的可设置的属性。这些选项可以用键 = 值的形式设置，并以逗号分隔。

option说明：

|可选项|描述|
|:--|:--|
|autoseparators|默认为 True，表示执行撤销操作时是否自动插入一个“分隔符”（其作用是用于分隔操作记录）|
|exportselection|默认值为 True，表示被选中的文本是否可以被复制到剪切板，若是 False 则表示不允许。|
|insertbackground|设置插入光标的颜色，默认为 BLACK|
|insertborderwidth|设置插入光标的边框宽度，默认值为 0|
|insertofftime|该选项控制光标的闪烁频频率（灭的状态）|
|insertontime|该选项控制光标的闪烁频频率（亮的状态）|
|selectbackground|指定被选中文本的背景颜色，默认由系统决定|
|selectborderwidth|指定被选中文本的背景颜色，默认值是0|
|selectforeground|指定被选中文本的字体颜色，默认值由系统指定|
|setgrid|默认值是 False，指定一个布尔类型的值，确定是否启用网格控制|
|spacing1|指定 Text 控件文本块中每一行与上方的空白间隔，注意忽略自动换行，且默认值为 0。|
|spacing2|指定 Text 控件文本块中自动换行的各行间的空白间隔，忽略换行符，默认值为0|
|spacing3|指定 Text 组件文本中每一行与下方的空白间隔，忽略自动换行，默认值是 0|
|tabs|定制 Tag 所描述的文本块中 Tab 按键的功能，默认被定义为 8 个字符宽度，比如 tabs=('1c', '2c', '8c') 表示前 3 个 Tab 宽度分别为 1厘米，2厘米，8厘米。|
|undo|该参数默认为 False，表示关闭 Text 控件的“撤销”功能，若为 True 则表示开启|
|wrap|该参数用来设置当一行文本的长度超过 width 选项设置的宽度时，是否自动换行，参数值 none（不自动换行）、char（按字符自动换行）、word（按单词自动换行）|
|xscrollcommand|该参数与 Scrollbar 相关联，表示沿水平方向上下滑动|
|yscrollcommand|该参数与 Scrollbar 相关联，表示沿垂直方向左右滑动|

方法：
Text 中的方法有几十个之多，这里不进行一一列举，主要对常用的方法进行介绍，如下表所示：

|可选项|描述|
|:--|:--|
|bbox(index)|返回指定索引的字符的边界框，返回值是一个 4 元组，格式为(x,y,width,height)|
|edit_modified()|该方法用于查询和设置 modified 标志（该标标志用于追踪 Text 组件的内容是否发生变化）|
|edit_redo()|“恢复”上一次的“撤销”操作，如果设置 undo 选项为 False，则该方法无效。|
|edit_separator()|插入一个“分隔符”到存放操作记录的栈中，用于表示已经完成一次完整的操作，如果设置 undo 选项为 False，则该方法无效。|
|get(index1, index2)|返回特定位置的字符，或者一个范围内的文字。|
|image_cget(index, option)|返回 index 参数指定的嵌入 image 对象的 option 选项的值，如果给定的位置没有嵌入 image 对象，则抛出 TclError 异常|
|image_create()|在 index 参数指定的位置嵌入一个 image 对象，该 image 对象必须是 Tkinter 的 PhotoImage 或 BitmapImage 实例。|
|insert(index, text)|在 index 参数指定的位置插入字符串，第一个参数也可以设置为 INSERT，表示在光标处插入，END 表示在末尾处插入。|
|delete(startindex [, endindex])|删除特定位置的字符，或者一个范围内的文字。|
|see(index)|如果指定索引位置的文字是可见的，则返回 True，否则返回 False。|

示例代码:
test_Text.py
```sh
import webbrowser
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
        self.text = Text(self, width=40, height=12, bg="gray")
        self.text.pack()

        self.text.insert(1.0, "123456789\n987654321")
        self.text.insert(2.3, "锄禾日当午,汗滴禾下土.谁知盘中餐,粒粒皆辛苦.\n")

        Button(self, text="重复插入文本", command=self.insertText).pack(side="left")
        Button(self, text="返回文本", command=self.returntText).pack(side="left")
        Button(self, text="添加图片", command=self.addImg).pack(side="left")
        Button(self, text="添加组件", command=self.addWidget).pack(side="left")
        Button(self, text="通过tag控制文本", command=self.testTag).pack(side="left")

    def insertText(self):
        # INSERT索引标识在光标处插入
        self.text.insert(INSERT, "hello word!")
        # END表示在最后插入
        self.text.insert(END, "[text]")

    def returntText(self):
        # Indexes(索引)用来指向Text组件中文本的位置，Text的组件索引也是对应实际字符之间的位置
        # 行号以0开始，列号以1开始
        print(self.text.get(1.3, 1.8))
        self.text.insert(1.9, "哈哈哈")
        print("所有文本内容:\n" + self.text.get(1.0, END))

    def addImg(self):
        # global photo
        self.photo = PhotoImage(file="c.gif")
        self.text.image_create(END, image=self.photo)

    def addWidget(self):
        # 在text中创建组件
        btn = Button(self.text, text="点我")
        self.text.window_create(INSERT, window=btn)

    def testTag(self):
        self.text.delete(1.0, END)
        self.text.insert(INSERT, "good good study day day up!\n锄禾日当午,\n汗滴禾下土.\n谁知盘中餐,\n粒粒皆辛苦.\n百度一下")
        
        # 添加tag，可单独设置样式，也可以绑定事件
        self.text.tag_add("good", 1.0, 1.9)
        self.text.tag_config("good", background="yellow", foreground='red')

        self.text.tag_add("baidu", 5.0, 5.4)
        self.text.tag_config("baidu", underline=True)
        self.text.tag_bind("baidu", "<Button-1>", self.webshow)

    def webshow(self,event):
        webbrowser.open("http://www.baidu.com")


if __name__ == '__main__':
    root = Tk()
    root.geometry("600x530+200+300")
    app = Applicaton(master=root)
    root.mainloop()
```

---

### Raduibutton单选框
在同一组选项中选择一个，可以显示文本，也可以显示图像

语法格式：
```sh
radioBtn=Radiobutton(master, option, ...)
```
* master:父容器
* options: 可选项，即该按钮的可设置的属性。这些选项可以用键 = 值的形式设置，并以逗号分隔。

option说明：

|可选项|描述|
|:--|:--|
|activebackground|设置当 Radiobutton 处于活动状态（通过 state 选项设置状态）的背景色，默认值由系统指定|
|activeforeground|设置当 Radiobutton 处于活动状态（通过 state 选项设置状态）的前景色，默认值由系统指定|
|compound|默认值为 None，控制 Radiobutton 中文本和图像的混合模式，默认情况下，如果有指定位图或图片，则不显示文本 。如果该选项设置为 "center"，文本显示在图像上（文本重叠图像）。设置为 "bottom"，"left"，"right" 或 "top"，那么图像显示在文本的旁边，比如如"bottom"，则显示图像在文本的下方。|
|disabledforeground|指定当 Radiobutton 不可用的时的前景色颜色，默认由系统指定|
|indicatoron|该参数表示选项前面的小圆圈是否被绘制，默认为 True，即绘制如果设置为 False，则会改变单选按钮的样式，当点击时按钮会变成 "sunken"（凹陷），再次点击变为 "raised"（凸起）；|
|selectcolor|设置当 Radiobutton 为选中状态的时候显示的图片；如果没有指定 image 选项，该选项被忽略|
|takefocus|如果是 True，该组件接受输入焦点，默认为 False|
|variable|表示与 Radiobutton 控件关联的变量，注意同一组中的所有按钮的 variable 选项应该都指向同一个变量，通过将该变量与 value 选项值对比，可以判断用户选中了哪个按钮|

方法：
常用方法如下所示：

|可选项|描述|
|:--|:--|
|deselect()|取消该按钮的选中状态|
|flash()|刷新 Radiobutton 控件，该方法将重绘 Radiobutton控件若干次（即在"active" 和 "normal" 状态间切换）|
|invoke()| 调用 Radiobutton 中 command 参数指定的函数，并返回函数的返回值;如果 Radiobutton 控件的 state(状态) 是 "disabled" （不可用）或没有指定 command 选项，则该方法无效|
|select()|将 Radiobutton 控件设置为选中状态|

示例代码：
test_Radiobutton.py
```sh
import tkinter as tk
from tkinter import *
from tkinter import ttk, messagebox


class Applicaton(Frame):

    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.creatWidgetRadioButton()

    # 单选框
    def creatWidgetRadioButton(self):
        # 创建变量
        self.v = StringVar()
        # 设置默认值
        self.v.set("F")

        self.r01 = Radiobutton(self, text="男性", value='M', variable=self.v)
        self.r02 = Radiobutton(self, text="女性", value='F', variable=self.v)

        self.r01.pack(side="left")
        self.r02.pack(side="left")

        Button(self, text="确定", command=self.confirm).pack(side='left')

    def confirm(self):
        messagebox.showinfo("单选", "选择的性别为:" + self.v.get())

if __name__ == '__main__':
    root = Tk()
    root.geometry("400x130+200+300")
    app = Applicaton(master=root)
    root.mainloop()
```
---

### CheckButton多选框
CheckButton多选框，可以显示文本，也可以显示图像,各个选项之间属于并列的关系

语法格式：
```sh
checkBtn=CheckButton(master, option, ...)
```
* master:父容器
* options: 可选项，即该按钮的可设置的属性。这些选项可以用键 = 值的形式设置，并以逗号分隔。

option说明：

|可选项|描述|
|:--|:--|
|text|显示的文本，使用 "\n" 来对文本进行换行。|
|variable|和复选框按钮关联的变量，该变量值会随着用户选择行为来改变（选或不选），即在 onvalue 和 offvalue 设置值之间切换，这些操作由系统自动完成。在默认情况下，variable 选项设置为 1 表示选中状态，反之则为 0，表示不选中。|
|onvalue|通过设置 onvalue 的值来自定义选中状态的值。|
|offvalue|通过设置 offvalue 的值来自定义未选中状态的值。|
|indicatoron|默认为 True，表示是否绘制用来选择的选项的小方块，当设置为 False 时，会改变原有按钮的样式，与单选按钮相同。|
|selectcolor|选择框的颜色（即小方块的颜色），默认由系统指定|
|selectimage|设置当 Checkbutton 为选中状态的时候显示的图片，若如果没有指定 image 选项，该选项被忽略|
|textvariable|Checkbutton 显示 Tkinter 变量（通常是一个 StringVar 变量）的内容，如果变量被修改，Checkbutton 的文本会自动更新|
|wraplength|表示复选框文本应该被分成多少行，该选项指定每行的长度，单位是屏幕单元，默认值为 0|

方法：
常用方法如下所示：

|可选项|描述|
|:--|:--|
|desellect()|取消 Checkbutton 组件的选中状态，也就是设置 variable 为 offvalue。|
|flash()|刷新 Checkbutton 组件，对其进行重绘操作，即将前景色与背景色互换从而产生闪烁的效果。|
|invoke()|调用 Checkbutton 中 command 选项指定的函数或方法，并返回函数的返回值。如果 Checkbutton 的state(状态)"disabled"是 （不可用）或没有指定 command 选项，则该方法无效。|
|select()|将 Checkbutton 组件设置为选中状态，也就是设置 variable 为 onvalue|
|toggle()|改变复选框的状态，如果复选框现在状态是 on，就改成 off，反之亦然|


示例代码:
```sh
import tkinter as tk
from tkinter import *
from tkinter import ttk, messagebox

class Applicaton(Frame):

    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.creatWidgetCheckButton()

    # 多选框
    def creatWidgetCheckButton(self):
        self.codeHubby = IntVar()
        self.videoHobby = IntVar()

        self.c1 = Checkbutton(self, text="敲代码", variable=self.codeHubby, onvalue=1, offvalue=0)
        self.c2 = Checkbutton(self, text="看视频", variable=self.videoHobby, onvalue=1, offvalue=0)

        self.c1.pack(side="left")
        self.c2.pack(side="left")

        Button(self, text="确定", command=self.confirm).pack(side="left")

    def confirm(self):
        if self.videoHobby.get() == 0 and self.codeHubby.get() == 0:
            messagebox.showinfo("弹窗", "请选择一项后点击确定！")
        else:
            res = "看视频" if self.videoHobby.get() == 1 else ""
            res += "敲代码" if self.codeHubby.get() == 1 else ""
            messagebox.showinfo("弹窗", '今天开始:' + res)


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x130+200+300")
    app = Applicaton(master=root)
    root.mainloop()
```
---
##### Canvas画布
Canvas画布，画布是一个矩形区域，可添加图片、图像、组件等，可以绘制各种图形/图像；

语法格式：
```sh
canv=Canvas画布(master, option, ...)
```
* master:父容器
* options: 可选项，即该按钮的可设置的属性。这些选项可以用键 = 值的形式设置，并以逗号分隔。

option说明：

|可选项|描述|
|:--|:--|
|background(bg)|指定 Canvas 控件的背景颜色|
|borderwidth(bd)|指定 Canvas 控件的边框宽度|
|closeenough|指定一个距离，当鼠标与画布对象的距离小于该值时，认为鼠标位于画布对象上。该选项是一个浮点类型的值|
|confine|指定 Canvas 控件是否允许滚动超出 scrollregion 选项设置的滚动范围，默认值为 True|
|selectbackground|指定当画布对象（即在 Canvas 画布上绘制的图形）被选中时的背景色，|
|selectborderwidth|指定当画布对象被选中时的边框宽度（选中边框）|
|selectforeground|指定当画布对象被选中时的前景色|
|state|设置 Canvas 的状态："normal" 或 "disabled"，默认值是 "normal"，注意，该值不会影响画布对象的状态|
|takefocus|指定使用 Tab 键可以将焦点移动到输入框中，默认为开启，将该选项设置为 False 避免焦点在此输入框中|
|width|指定 Canvas 的宽度，单位为像素|
|xscrollcommand|与 scrollbar（滚动条）控件相关联（沿着 x 轴水平方向）|
|xscrollincrement|该选项指定 Canvas 水平滚动的“步长” ,例如 '3c' 表示 3 厘米，还可以选择的单位有 'i'（英寸），'m'（毫米）和 'p'（DPI，大约是 '1i' 等于 '72p'） 。默认为 0，表示可以水平滚动到任意位置|
|yscrollcommand|与 scrollbar 控件（滚动条）相关联（沿着 y 轴垂直方向）|
|yscrollincrement|1. 该选项指定 Canvas 垂直滚动的“步长” ,例如 '3c' 表示 3 厘米，还可以选择的单位有 'i'（英寸），'m'（毫米）和 'p'（DPI，大约是 '1i' 等于 '72p'）.默认值是 0，表示可以垂直方向滚动到任意位置|

方法：
Cansvas 控件提供了一系列绘制几何图形的常用方法，下面对这些方法做简单介绍:

|可选项|描述|
|:--|:--|
|create_line(x0, y0, x1, y1, ... , xn, yn, options)|根据给定的坐标创建一条或者多条线段； 参数 x0,y0,x1,y1,...,xn,yn 定义线条的坐标；参数 options 表示其他可选参数|
|create_oval(x0, y0, x1, y1, options)|绘制一个圆形或椭圆形；参数 x0 与 y0 定义绘图区域的左上角坐标；参数 x1 与 y1 定义绘图区域的右下角坐标；参数 options 表示其他可选参数|
|create_polygon(x0, y0, x1, y1, ... , xn, yn, options)|绘制一个至少三个点的多边形；参数 x0、y0、x1、y1、...、xn、yn 定义多边形的坐标；参数 options 表示其他可选参数|
|create_rectangle(x0, y0, x1, y1, options)|绘制一个矩形；参数 x0 与 y0 定义矩形的左上角坐标；参数 x 与 y1 定义矩形的右下角坐标； 参数 options 表示其他可选参数|
|create_text(x0, y0, text, options)|绘制一个文字字符串。其中参数 x0 与 y0 定义文字字符串的左上角坐标，参数 text 定义文字字符串的文字；参数 options 表示其他可选参数|
|create_image(x, y, image)|创建一个图片;参数 x 与 y 定义图片的左上角坐标；参数 image 定义图片的来源，必须是 tkinter 模块的 BitmapImage 类或 PhotoImage 类的实例变量。|
|create_bitmap(x, y, bitmap)|创建一个位图；参数 x 与 y 定义位图的左上角坐标；参数 bitmap 定义位图的来源，参数值可以是 gray12、gray25、gray50、gray75、hourglass、error、questhead、info、warning 或 question，或者也可以直接使用 XBM（X Bitmap）类型的文件，此时需要在 XBM 文件名称前添加一个 @ 符号，例如 bitmap=@hello.xbm|
|create_arc(coord, start, extent, fill)|绘制一个弧形；参数 coord 定义画弧形区块的左上角与右下角坐标；参数 start 定义画弧形区块的起始角度（逆时针方向）；参数 extent 定义画弧形区块的结束角度（逆时针方向）； 参数 fill 定义填充弧形区块的颜色。|

注意：上述方法都会返回一个画布对象的唯一 ID。关于 options 参数，下面会通过一个示例对经常使用的参数做相关介绍。（但由于可选参数较多，并且每个方法中的参数作用大同小异，因此对它们不再逐一列举）

从上述表格不难看出，Canvas 控件采用了坐标系的方式来确定画布中的每一点。一般情况下，默认主窗口的左上角为坐标原点，这种坐标系被称作为“窗口坐标系”，但也会存在另外一种情况，即画布的大小可能大于主窗口，当发生这种情况的时，可以采用带滚动条的 Canvas 控件，此时会以画布的左上角为坐标原点，我们将这种坐标系称为“画布坐标系”。

示例代码:
test_Canvas.py
```sh
import random
import tkinter as tk
from tkinter import *
from tkinter import ttk, messagebox

class Applicaton(Frame):

    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.creatWidgetCanvas()

    # canvas 画布
    def creatWidgetCanvas(self):
        self.canvas = Canvas(self, width=300, height=200, bg="green")
        self.canvas.pack()

        # 画一条线
        line = self.canvas.create_line(10, 10, 50, 50, 20, 100, 100, 20)
        # 画一个矩形
        rect = self.canvas.create_rectangle(30, 30, 120, 20)

        # 画一个圆
        oval = self.canvas.create_oval(60, 60, 100, 100)

        Button(self, text="画10个矩形", command=self.drawOval).pack(side="left")

    def drawOval(self):
        for i in range(0, 10):
            x1 = random.randrange(int(self.canvas["width"]) / 2)
            y1 = random.randrange(int(self.canvas["height"]) / 2)
            x2 = x1 + random.randrange(int(self.canvas["width"]) / 2)
            y2 = y1 + random.randrange(int(self.canvas["height"]) / 2)
            self.canvas.create_rectangle(x1, x2, y1, y2)


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x330+200+300")
    app = Applicaton(master=root)
    root.mainloop() 
```
---
### Grid布局管理器
一个完整的软件必定有许多的组件组成，这些组件的排布则通过布局管理器来进行管理；tkinter 提供了三种布局管理器，分别是
1. pack:垂直/水平排列，此方法灵活性较差
2. grid:表格布局管，采用表格结构管理组件，子组件的位置由行和列的单元格来确定，并且可以跨越行和列，从而实现复杂的页面布局，此种方法使用起来较为灵活
3. place:位置管理器，通过像素位置控制组件，可以指定组件大小以及摆放位置，三个方法中最为灵活的布局方法

Grid布局介绍：

|可选项|描述|
|:--|:--|
|column|指定组件插入的列（0 表示第 1 列）,默认值是 0|
|columnspan|指定用多少列（跨列）显示该组件
|ipadx|指定水平方向上的内边距|
|ipady|指定垂直方向上的内边距|
|padx|指定水平方向上的外边距|
|pady|指定垂直方向上的外边距|
|row|指定组件插入的行（0 表示第 1 行）,指定用多少行（跨行）显示该组件|
|sticky|控制组件在 grid 分配的空间中的位置;|可以使用 "n", "e", "s", "w" 以及它们的组合来定位（ewsn代表东西南北，上北下南左西右东）;使用加号（+）表示拉长填充，例如 "n" + "s" 表示将组件垂直拉长填充网格，"n" + "s" + "w" + "e" 表示填充整个网格;不指定该值则居中显示|

示例代码1:
test_Grid.py
```sh
import random
import tkinter as tk
from tkinter import *
from tkinter import ttk, messagebox

class Applicaton(Frame):

    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.creatWidgetGrid()

    # Grid
    def creatWidgetGrid(self):
        self.label01 = Label(self, text="用户名：")
        self.label01.grid(row=0, column=0)

        self.entry01 = Entry(self)
        self.entry01.grid(row=0, column=1)

        Label(self, text="用户名为手机号").grid(row=0, column=2)

        Label(self, text="密码").grid(row=1, column=0)
        Entry(self, text="*").grid(row=1, column=1)

        Button(self, text="登录").grid(row=2, column=1, sticky=EW)
        Button(self, text="取消").grid(row=2, column=2, sticky=E)


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x330+200+300")
    app = Applicaton(master=root)
    root.mainloop()
```

行/列展示代码2：
test_Grid2.py
```sh
import random
import tkinter as tk
from tkinter import *
from tkinter import ttk, messagebox

class Applicaton(Frame):

    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.creatWidgetGrid()

    # Grid
    def creatWidgetGrid(self):
        # 在窗口内创建按钮，以表格的形式依次排列
        for i in range(10):
            for j in range(10):
                Button(self, text=" (" + str(i) + "," + str(j) + ")", bg='#D1EEEE').grid(row=i, column=j)


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x330+200+300")
    app = Applicaton(master=root)
    root.mainloop()
```
效果展示：

![](https://s2.loli.net/2022/04/07/R5blZe8JyfspaYG.png)
---
### 使用Grid实现计算器页面布局
示例代码：
```sh
import random
import tkinter as tk
from tkinter import *
from tkinter import ttk, messagebox

"""
通过 Grid 实现计算器实布局实现  7x4布局
"""


class Applicaton(Frame):

    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.creatWidget()

    # 计算器软件界面设计
    def creatWidget(self):
        buttonText = (("CE", "C", "<-", "÷"),
                      ("7", "8", "9", "X"),
                      ("4", "5", "6", "-"),
                      ("1", "2", "3", "+"),
                      ("±", "0", ".", "="))

        Entry(self).grid(row=0, column=0, columnspan=4, pady=10)

        for rindex, r in enumerate(buttonText):
            for cindex, c in enumerate(r):
                Button(self, text=c, width=3).grid(row=rindex + 1, column=cindex,sticky=NSEW)


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x330+200+300")
    app = Applicaton(master=root)
    root.mainloop()

```

效果展示:

![](https://s2.loli.net/2022/04/07/L7a9b34qrtfWMIV.png)
---

### Pack布局管理器
pack() 是一种较为简单的布局方法，在不使用任何参数的情况下，它会将控件以添加时的先后顺序，自上而下，一行一行的进行排列，并且默认居中显示。pack() 方法的常用参数如下所示：

|可选项|描述|
|:--|:--|
|anchor|组件在窗口中的对齐方式，有 9 个方位参数值，比如"n"/"w"/"s"/"e"/"ne"，以及 "center" 等（这里的 e w s n分别代表，东西南北）|
|expand|是否可扩展窗口，参数值为 True（扩展）或者 False（不扩展），默认为 False，若设置为 True，则控件的位置始终位于窗口的中央位置|
|fill|参数值为 X/Y/BOTH/NONE，表示允许控件在水平/垂直/同时在两个方向上进行拉伸，比如当 fill = X 时，控件会占满水平方向上的所有剩余的空间。|
|ipadx,ipady|需要与 fill 参数值共同使用，表示组件与内容和组件边框的距离（内边距），比如文本内容和组件边框的距离，单位为像素(p)，或者厘米(c)、英寸(i)|
|padx,pady|用于控制组件之间的上下、左右的距离（外边距），单位为像素(p)，或者厘米(c)、英寸(i)|
|side|组件放置在窗口的哪个位置上，参数值 'top','bottom','left','right'。注意，单词小写时需要使用字符串格式，若为大写单词则不必使用字符串格式|

示例代码：
```sh
import random
import tkinter as tk
from tkinter import *
from tkinter import ttk, messagebox

class Applicaton(Frame):

    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.creatWidgetPack()

    # pack
    def creatWidgetPack(self):
        lb_red = Label(self, text="红色", bg="Red", fg='#ffffff', relief=GROOVE)
        # 默认以top方式放置
        lb_red.pack()
        lb_blue = Label(self, text="蓝色", bg="blue", fg='#ffffff', relief=GROOVE)
        # 沿着水平方向填充，使用 pady 控制蓝色标签与其他标签的上下距离为 5 个像素
        lb_blue.pack(fill=X, pady='5px')
        lb_green = Label(self, text="绿色", bg="green", fg='#ffffff', relief=RAISED)
        # 将 黄色标签所在区域都填充为黄色，当使用 fill 参数时，必须设置 expand = 1，否则不能生效
        lb_green.pack(side=LEFT, expand=1, fill=BOTH)


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x330+200+300")
    app = Applicaton(master=root)
    root.mainloop()
```
效果展示:
![](https://s2.loli.net/2022/04/07/lARIaiDs9y4KF7L.png)
---

### Place布局管理器
与前两种布局方法相比，采用 place() 方法进行布局管理要更加精细化，通过 place() 布局管理器可以直接指定控件在窗体内的绝对位置，或者相对于其他控件定位的相对位置。

|可选项|描述|
|:--|:--|
|anchor|定义控件在窗体内的方位，参数值N/NE/E/SE/S/SW/W/NW 或 CENTER，默认值是 NW|
|bordermode|定义控件的坐标是否要考虑边界的宽度，参数值为 OUTSIDE（排除边界） 或 INSIDE（包含边界），默认值 INSIDE。|
|x、y|定义控件在根窗体中水平和垂直方向上的起始绝对位置|
|relx、rely|定义控件相对于根窗口（或其他控件）在水平和垂直方向上的相对位置（即位移比例），取值范围再 0.0~1.0 之间,可设置 in_ 参数项，相对于某个其他控件的位置|
|height、width|控件自身的高度和宽度（单位为像素）|
|relheight、relwidth|控件高度和宽度相对于根窗体高度和宽度的比例，取值也在 0.0~1.0 之间|

示例代码:
```sh
import random
import tkinter as tk
from tkinter import *
from tkinter import ttk, messagebox


class Applicaton(Frame):

    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.creatWidgetPlace()

    # Place
    def creatWidgetPlace(self):
        # 创建一个frame窗体对象，用来包裹标签
        frame = Frame(self, relief=SUNKEN, borderwidth=2, width=450, height=250)
        # 在水平、垂直方向上填充窗体
        frame.pack(side=TOP, fill=BOTH, expand=1)
        # 创建 "位置1"
        Label1 = Label(frame, text="位置1", bg='blue', fg='white')
        # 使用 place,设置第一个标签位于距离窗体左上角的位置（40,40）和其大小（width，height）
        # 注意这里（x,y）位置坐标指的是标签左上角的位置（以NW左上角进行绝对定位，默认为NW）
        Label1.place(x=40, y=40, width=60, height=30)
        # 设置标签2
        Label2 = Label(frame, text="位置2", bg='purple', fg='white')
        # 以右上角进行绝对值定位，anchor=NE，第二个标签的位置在距离窗体左上角的(180，80)
        Label2.place(x=180, y=80, anchor=NE, width=60, height=30)
        # 设置标签3
        Label3 = Label(frame, text="位置3", bg='green', fg='white')
        # 设置水平起始位置相对于窗体水平距离的0.6倍，垂直的绝对距离为80，大小为60，30
        Label3.place(relx=0.6, y=80, width=60, height=30)
        # 设置标签4
        Label4 = Label(frame, text="位置4", bg='gray', fg='white')
        # 设置水平起始位置相对于窗体水平距离的0.01倍，垂直的绝对距离为80，并设置高度为窗体高度比例的0.5倍，宽度为80
        Label4.place(relx=0.01, y=80, relheight=0.4, width=80)


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x330+200+300")
    app = Applicaton(master=root)
    root.mainloop()

```

效果展示:
![](https://s2.loli.net/2022/04/07/742MtG3yxHiWTCg.png)
---
未完待续 ...






