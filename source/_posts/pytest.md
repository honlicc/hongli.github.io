---
title: pytest
categories: 
- pytest
date: 2021-11-04 19:38:24
tags:
- 自动化测试
---

## Pytest：

**注：本文内容较长，建议收藏/订阅后慢慢学习**

### 前言：

* 自动化测试前：需要提前准备好测试数据，测试完成后，需要自动清里测试脏数据，有没有更好用的框架？
* 自动化测试中：需要使用多套测试数据实现用例的参数化，有没有更便捷的方式？
* 自动化测试后：需要自动生成优雅，简洁的测试报告，有没有更好的生成办法？

### Pytest:

* pytest能够支持简单的单元测试和复杂的功能测试；
* pytest可以结合requests实现接口测试，结合Selenium,appium实现自动化功能测试。
* 使用pytest结合Allure集成到jekins中可以实现持续集成。
* pytest支持315种以上插件。

### 特点：
pytest为python的第三方单元测试库，需额外安装。它是一个非常成熟的全功能的python测试框架，主要有以下几个特点：

* 简单灵活，容易上手。
* 支持参数化。
* 能够支持简单的单元测试和复杂的功能测试。
* pytest具有很多第三方插件，并且可以自定义扩展。
* 执行测试过程中可以将某些测试跳过（skip），或者对某些预期失败的case标记成失败。
* 支持重复执行(rerun)失败的case。
* 方便的和持续集成工具jenkins集成。
* 可支持执行部分用例

### 安装:
```sh
pip install -U pytest
```

### 查看版本:
```sh
pytest --version
```

### 第一个脚本:
新建一个test_demo.py文件，写入如下代码：
```sh
def test_answer():
    assert 3==5
```
<br> 
打开cmd  进入 test_demo.py 文件所在文件夹,输入 pytest 或者 pytest test_demo.py,结果如下：
```sh
================================================= test session starts =================================================
platform win32 -- Python 3.7.7, pytest-6.2.5, py-1.10.0, pluggy-1.0.0
rootdir: E:\test1\aabb\case
collected 1 item

test_sample.py F                                                                                                 [100%]

====================================================== FAILURES =======================================================
_____________________________________________________ test_answer _____________________________________________________

    def test_answer():
>       assert 3==5
E       assert 3 == 5

test_sample.py:3: AssertionError
=============================================== short test summary info ===============================================
FAILED test_sample.py::test_answer - assert 3 == 5
================================================== 1 failed in 0.14s ==================================================
```
<br> 

### pytest运行规则：
pytest运行规则：查找当前目录及其子目录下以test_*.py或*_test.py文件，找到文件后，在文件中找到以test开头函数并执行

### 以类的形式编写case:

上面是一个测试函数，当case较多的情况就不太合适了，我们可以把case写在类里面。
新建一个 test_demoClass.py文件，编写如下的代码：
```sh
import pytest
class TestClass:
    def test_one(self):
        x = "this"
        assert 'h' in x

    def test_two(self):
        x = "hello"
        assert hasattr(x, 'check')

    def test_three(self):
        a = "hello"
        b = "hello world"
        assert a in b

if __name__ == "__main__":
    pytest.main('-q test_class.py')
```
<br> 

运行该类，结果如下：
```sh
Testing started at 18:42 ...
Launching pytest with arguments E:/test1/aabb/case/test_class.py --no-header --no-summary -q in E:\test1\aabb\case

============================= test session starts =============================
collecting ... collected 3 items

test_class.py::TestClass::test_one PASSED                                [ 33%]
test_class.py::TestClass::test_two FAILED                                [ 66%]
test_class.py:8 (TestClass.test_two)
self = <aabb.case.test_class.TestClass object at 0x0000019378686E48>

    def test_two(self):
        x = "hello"
>       assert hasattr(x, 'check')
E       AssertionError: assert False
E        +  where False = hasattr('hello', 'check')

test_class.py:11: AssertionError



test_class.py::TestClass::test_three PASSED                              [100%]

========================= 1 failed, 2 passed in 0.07s =========================
```

### 命名规则：


| 类型       | 规则                   |
|:------|:----------------|
| 文件名         |test_开头或者_test结尾|
| 方法         |test_开头|
| 类         |Test开头|


**注：测试勒种不可用__init__构造函数**




### 运行多条用例：

* 运行包下所有用例：pytest/py.test [包名]
* 单独运行一个pytest模块：pytest 文件名.py
* 运行某个模块里的某个类，pytest 文件夹.py::类名
* 运行某个模块里面某个类的某个方法 pytest 文件名.py::类名::方法名:

示例:
我们在项目目录中分别创建两个py文件并复制如下代码：
test_sample.py
```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

def test_one():
    print("正在执行----test_one")
    x = "this"
    assert 'h' in x


def test_two():
    print("正在执行----test_two")
    x = "hello"
    assert hasattr(x, 'check')


class TestCase():


    def test_three(self):
        print("正在执行----test_three")
        x = "this"
        assert 'h' in x


    def test_four(self):
        print("正在执行----test_four")
        x = "hello"
        assert hasattr(x, 'check')


```

test_sample2.py
```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

def test_samples():
    print("正在执行----test_samples")

```
打开cmd,进入项目目录中执行命令

命令： (运行包下所有用例：pytest/py.test [包名])
```
pytest
```

输出：
```
:\study\test20\test_pytest_demo>pytest
=================================== test session starts ====================================
platform win32 -- Python 3.10.1, pytest-6.2.5, py-1.11.0, pluggy-1.0.0
rootdir: E:\study\test20\test_pytest_demo
plugins: allure-pytest-2.9.45
collected 5 items                                                                           

test_sample.py .F.F                                                                   [ 80%]
test_sample2.py .                                                                     [100%]

========================================= FAILURES =========================================
_________________________________________ test_two _________________________________________

    def test_two():
        print("正在执行----test_two")
        x = "hello"
>       assert hasattr(x, 'check')
E       AssertionError: assert False
E        +  where False = hasattr('hello', 'check')

test_sample.py:16: AssertionError
----------------------------------- Captured stdout call -----------------------------------
正在执行----test_two
____________________________________ TestCase.test_four ____________________________________

self = <test_pytest_demo.test_sample.TestCase object at 0x0000014AAC862380>

    def test_four(self):
        print("正在执行----test_four")
        x = "hello"
>       assert hasattr(x, 'check')
E       AssertionError: assert False
E        +  where False = hasattr('hello', 'check')

test_sample.py:31: AssertionError
----------------------------------- Captured stdout call -----------------------------------
正在执行----test_four
================================= short test summary info ==================================
FAILED test_sample.py::test_two - AssertionError: assert False
FAILED test_sample.py::TestCase::test_four - AssertionError: assert False
=============================== 2 failed, 3 passed in 0.20s ================================

```

可以看到里面有一段这样的内容
```
test_sample.py .F.F                                                                   [ 80%]
test_sample2.py .                                                                     [100%]
```
当我们执行pytest命令时，test_sample.py 和 test_sample2.py文件都被执行了

命令:(单独运行一个pytest模块：pytest 文件名.py)
```
pytest test_sample2.py
```

输出：
```angular2html
=============================== test session starts ===============================
platform win32 -- Python 3.10.1, pytest-6.2.5, py-1.11.0, pluggy-1.0.0
rootdir: E:\study\test20\test_pytest_demo
plugins: allure-pytest-2.9.45
collected 1 item                                                                                                                                                                                          

test_sample2.py .                                                                                                                                                                                   [100%]

================================ 1 passed in 0.01s ==================================

```
可以看到只执行了test_sample2.py 文件中的case

命令：(运行某个模块里的某个类，pytest 文件夹.py::类名)
```angular2html
pytest test_sample.py::TestCase
```
输出：
```angular2html
================================= test session starts =================================
platform win32 -- Python 3.10.1, pytest-6.2.5, py-1.11.0, pluggy-1.0.0
rootdir: E:\study\test20\test_pytest_demo
plugins: allure-pytest-2.9.45
collected 2 items                                                                                                                                                                                         

test_sample.py .F                                                                                                                                                                                   [100%]

======================================= FAILURES =======================================
____________________________________ TestCase.test_four ________________________________

self = <test_pytest_demo.test_sample.TestCase object at 0x000002B272F95AE0>

    def test_four(self):
        print("正在执行----test_four")
        x = "hello"
>       assert hasattr(x, 'check')
E       AssertionError: assert False
E        +  where False = hasattr('hello', 'check')

test_sample.py:31: AssertionError
------------------------------------- Captured stdout call -------------------------------
正在执行----test_four
==================================== short test summary info =============================
FAILED test_sample.py::TestCase::test_four - AssertionError: assert False
=================================== 1 failed, 1 passed in 0.05s ==========================

```
可以看到只运行了test_samples.py的TestCase类里边的case;test_three运行通过，test_four运行失败

命令：(运行某个模块里面某个类的某个方法 pytest 文件名.py::类名::方法名:)
```angular2html
pytest test_sample.py::TestCase::test_four
```

输出：
```angular2html
===================================== test session starts ================================
platform win32 -- Python 3.10.1, pytest-6.2.5, py-1.11.0, pluggy-1.0.0
rootdir: E:\study\test20\test_pytest_demo
plugins: allure-pytest-2.9.45
collected 1 item                                                                                                                                                                                          

test_sample.py F                                                                                                                                                                                    [100%]

============================================= FAILURES ====================================
________________________________________ TestCase.test_four _______________________________

self = <test_pytest_demo.test_sample.TestCase object at 0x0000020DF29659C0>

    def test_four(self):
        print("正在执行----test_four")
        x = "hello"
>       assert hasattr(x, 'check')
E       AssertionError: assert False
E        +  where False = hasattr('hello', 'check')

test_sample.py:31: AssertionError
-------------------------------------------- Captured stdout call ---------------------------
正在执行----test_four
=========================================== short test summary info =========================
FAILED test_sample.py::TestCase::test_four - AssertionError: assert False
============================================== 1 failed in 0.05s ============================

```
可以看到只执行了 test_sample.py->TestClass->test_four 这一条case

**持续补充ing~**
。。。

。。。
### fixture：



