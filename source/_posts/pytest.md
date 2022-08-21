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

目录：

* [前言](#前言)
* [Pytest介绍](#Pytest)
* [特点](#特点)
* [安装](#安装)
* [查看版本](#查看版本)
* [第一个脚本](#第一个脚本)
* [pytest运行规则](#pytest运行规则)
* [以类的形式编写case](#以类的形式编写case)
* [命名规则](#命名规则)
* [运行多条用例](#运行多条用例)
* [fixTure:测试装置介绍(前置、后置条件、装饰器)](#fixTure)
* [常用命令行参数](#常用命令行参数)
* [Mark:标记测试用例](#Mark标记测试用例)
* [Mark:跳过(skip)及预期失败(xFail)](#Mark跳过测试用例以及预期失败)
* [Mark:参数化](#参数化)

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

打开cmd  进入 test_demo.py 文件所在文件夹,输入 pytest 或者 pytest test_demo.py,结果如下：
```
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


**注：测试类种不可用__init__构造函数**




### 运行多条用例：

* 运行包下所有用例：pytest/py.test [包名]
* 单独运行一个pytest模块：pytest 文件名.py
* 运行某个模块里的某个类：pytest 文件夹.py::类名
* 运行某个模块里面某个类的某个方法： pytest 文件名.py::类名::方法名:

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



### fixTure

测试装置介绍(前置、后置条件、装饰器)

| 类型       | 规则                   |
|:------|:----------------|
| setup_module/teardown_module         |全局模块级,模块始末，全局的(优先级最高)|
| setup_function/teardown_function |函数级/在类外,只对函数用例生效(不在类中)|
| setup_class/teardown_class          |类级，只在类中前后运行一次(在类中)|
| setup_method/teardown_method         | 方法级、类中的每个方法执行前后,开始于方法始末(在类中)|
| setup/teardown         |在类中，运行在调用方法前后(重点)，只在类中前后运行一次(在类中)|


* 先看下 **setup_function/teardown_function** 与case的执行关系

创建测试文件test_setup_teardown.py，粘贴如下代码：

```angular2html
#!/usr/bin/env python
# -*- coding: utf-8 -*-


def test_case_first():
    print("test case first")


def test_case_second():
    print("test case second")


def setup_function():
    print("资源准备：setup function")


def teardown_function():
    print("资源消毁：teardown function")


```

命令：(运行模块里面所有case, 注：[-vs] 参数,展示详细的执行过程)
```angular2html
pytest test_steup_teardown.py -vs
```

输出：
```angular2html
================================================= test session starts =================================================
platform win32 -- Python 3.10.1, pytest-6.2.5, py-1.11.0, pluggy-1.0.0 -- C:\Users\Administrator\AppData\Local\Programs\Python\Python310\python.exe
cachedir: .pytest_cache
rootdir: E:\study\test20\test_pytest, configfile: pytest.ini
plugins: allure-pytest-2.9.45
collected 2 items

test_steup_teardown.py::test_case_first 资源准备：setup function
test case first
PASSED资源消毁：teardown function

test_steup_teardown.py::test_case_second 资源准备：setup function
test case second
PASSED资源消毁：teardown function


================================================== 2 passed in 0.03s ==================================================
```

分析：
可以看到，当执行 case **test_case_first** 之前，首先执行了 **setup_function**，输出了 **资源准备：setup function**，随后执行 case **test_case_first**，输出了 **test case first** ，最后执行了 **teardown_function**，输出了 **资源消毁：teardown function**。
接着又开始执行case **test_case_second**,和执行 **test_case_first**一样，先执行**setup_function**，其次执行case **test_case_second**,最后执行**teardown_function**

结论：
setup_function：在每一条case执行之前调用
teardown_function：在每一条case执行之后调用

* 再看下 **setup_module/teardown_module** 与**setup_function/teardown_function** 和case的执行关系

在测试文件中test_setup_teardown.py，粘贴如下代码：

```angular2html
#!/usr/bin/env python
# -*- coding: utf-8 -*-


# 模块级别,只被调用一次
def setup_module():
    print("资源准备：setup module")


def teardown_module():
    print("资源准备：teardown module")


def test_case_first():
    print("test case first")


def test_case_second():
    print("test case second")


def setup_function():
    print("资源准备：setup function")


def teardown_function():
    print("资源消毁：teardown function")
```


命令：(运行模块里面所有case, 注：[-vs] 参数,展示详细的执行过程)
```angular2html
pytest test_steup_teardown.py -vs
```

输出：
```angular2html
====================================================== test session starts ======================================================
platform win32 -- Python 3.10.1, pytest-6.2.5, py-1.11.0, pluggy-1.0.0 -- C:\Users\Administrator\AppData\Local\Programs\Python\Pyt
hon310\python.exe
cachedir: .pytest_cache
rootdir: E:\study\test20\test_pytest, configfile: pytest.ini
plugins: allure-pytest-2.9.45
collected 2 items                                                                                                                

test_steup_teardown.py::test_case_first 资源准备：setup module
资源准备：setup function
test case first
PASSED资源消毁：teardown function

test_steup_teardown.py::test_case_second 资源准备：setup function
test case second
PASSED资源消毁：teardown function
资源准备：teardown module


======================================================= 2 passed in 0.01s =======================================================

```

分析：
可以看到执行顺序为 **setup_module**->**setup_function**->**test_case_first**->**teardown_function**->**setup_function**->**test_case_second**->**teardown_function**->**teardown_module**
结论：
setup_module：全局模块级,模块最开始，全局的(优先级最高)，类似于 unittest的 setUpClass
setup_function：在每一条case执行之前调用，类似于 unittest的 setUp
teardown_function：在每一条case执行之后调用，类似于 unittest的 tearDown
teardown_module：全局模块级,模块最末尾，全局的(优先级最高)，类似于 unittest的 tearDownClass



### Fixture在自动化中的应用场景

场景1：
测试用例执行时，有的用例需要登录才能执行，有的不需要登录，setup和teardown无法满足，fixture则可以，默认scope function

步骤：

* 导入pytest
* 在登录函数上面加上@pytest.fixture()
* 在需要使用的测试发方法中传入(登录函数名称)，就先登录
* 不传入就不登录直接执行测试方法

演示：
新建test_fixture1.py文件，复制如下代码

``` 
import pytest


def test_start_app():
    print("启动app")


@pytest.fixture()
def login():
    print("登录功能，登录成功")


def test_search():
    print("搜索功能")


def test_cart(login):
    print("添加购物车")


def test_order(login):
    # 需要提前登录
    print("下单功能")

```

示例：
执行如下命令
``` 
pytest -vs .\test_fixture1.py::test_order
```

输出：
```
collected 1 item                                                                                                                                                                                                                          

test_fixture1.py::test_order 登录功能，登录成功
下单功能
PASSED

=========================================================================================================== 1 passed in 0.04s ============================================================================================================
```

说明：可以看到，在代码中给 login方法添加了@pytest.fixture()装饰器，
在测试用例test_order中添加参数login(与login名称要一致)，执行test_order用例时自动先执行了login方法



场景2：
当与其他开发人员共同开发项目时，公共的模块要在大家都访问的到的地方

解决：
使用conftest.py模块进行共享，并且他可以放在不同的范围共享

前提：

* conftest文件名不能换
* 放在项目下是全局共享的地方

演示：
新建conftest.py文件，复制如下代码
```
@pytest.fixure()
def login():
    print("登录") 
```

新建test_fixure2.py,复制如下代码
```
import pytest

def test_order(login):
    print("订单case")
```

执行如下命令：
``` 
pytest --vs test_fixure2.py
```


输出：
``` 
collected 1 item                                                                                                                                                                                                                        

test_fixure2.py::test_order 登录成功
登录模块
PASSED

========================================================================================================== 1 passed in 0.01s ===========================================================================================================
```

说明：可以看到，case仍旧可以执行  


Fixture用法总结：

* 模拟setup/teardown(一个用例可以引用d多个fixture)
* yield用法
* 作用域(session,module,class,function)
``` 
 取值	    范围	    说明
function	函数级	每一个函数或方法都会调用
class	    类级别	每个测试类只运行一次
module	    模块级	每一个.py文件调用一次
session	    会话级	每次会话只需要运行一次，会话内所有方法及类，模块都共享这个方法
```
* 自动执行(autouse=True)
* conftest.py用法，一般会把fixture写在conftest.py文件中(名字不能改，必须为conftest)



### 常用命令行参数：

| 参数                   | 含义                           |
|:---------------------|:-----------------------------|
| --help               | 帮助，用来查看参数                    |
| -x                   | 用例一旦失败(fail/error) 就立即停止执行   |
| --maxfail=num        | 用例用例失败数达到多少时停止执行             |
| -m                   | 标记用例(重要)                     |
| -k                   | 执行包含某个关键字的测试用例               |
| -v                   | 打印详细日志                       |
| -s                   | 打印输出日志(通常-vs一起使用)            |
| --collect-only       | 收集用例不执行(测试平台，pytest自动导入功能)   |
| --lf(--last-failed)  | 只重新运行故障用例                    |
| --ff(--failed-first) | 先运行故障用例 再运行其它用例测试            |

演示：
新建 test_pytest.py文件，复制下面代码:
```sh
#!/usr/bin/env python
# -*- coding: utf-8 -*-

def inc(x):
    return x + 1


def test_case_1():
    assert inc(3) == 3


def test_case_2():
    assert inc(3) == 1


def test_case_3():
    assert inc(3) == 4


def test_case_sum_5():
    assert inc(4) == 4


def test_case_sum_6():
    assert inc(6) == 7

```

* help:帮助

命令行执行如下命令
示例：
```sh
pytest --help
```

输出：
``` 
usage: pytest [options] [file_or_dir] [file_or_dir] [...]

positional arguments:
  file_or_dir

general:
  -k EXPRESSION         only run tests which match the given substring expression. An expression is a python evaluatable expression where all names are substring-matched against test names and their parent classes. Example: -k
                        'test_method or test_other' matches all test functions and classes whose name contains 'test_method' or 'test_other', while -k 'not test_method' matches those that don't contain 'test_method' in their names. -k
                        'not test_method and not test_other' will eliminate the matches. Additionally keywords are matched to classes and functions containing extra names in their 'extra_keyword_matches' set, as well as functions which

                        have names assigned directly to them. The matching is case-insensitive.
  -m MARKEXPR           only run tests matching given mark expression.
                        For example: -m 'mark1 and not mark2'.
  --markers             show markers (builtin, plugin and per-project ones).
  -x, --exitfirst       exit instantly on first error or failed test.
  --fixtures, --funcargs
                        show available fixtures, sorted by plugin appearance (fixtures with leading '_' are only shown with '-v')
  --fixtures-per-test   show fixtures per test
  --pdb                 start the interactive Python debugger on errors or KeyboardInterrupt.
  --pdbcls=modulename:classname
                        specify a custom interactive Python debugger for use with --pdb.For example: --pdbcls=IPython.terminal.debugger:TerminalPdb
  --trace               Immediately break when running each test.
  --capture=method      per-test capturing method: one of fd|sys|no|tee-sys.
  -s                    shortcut for --capture=no.
  --runxfail            report the results of xfail tests as if they were not marked
  --lf, --last-failed   rerun only the tests that failed at the last run (or all if none failed)
  --ff, --failed-first  run all tests, but run the last failures first.
                        This may re-order tests and thus lead to repeated fixture setup/teardown.
  --nf, --new-first     run tests from new files first, then the rest of the tests sorted by file mtime
  --cache-show=[CACHESHOW]
                        show cache contents, don't perform collection or tests. Optional argument: glob (default: '*').
  --cache-clear         remove all cache contents at start of test run.
  --lfnf={all,none}, --last-failed-no-failures={all,none}
                        which tests to run with no previously (known) failures.
  --sw, --stepwise      exit on test failure and continue from last failing test next time
  --sw-skip, --stepwise-skip
                        ignore the first failing test but stop on the next failing test.
                        implicitly enables --stepwise.

reporting:
  --durations=N         show N slowest setup/test durations (N=0 for all).
  --durations-min=N     Minimal duration in seconds for inclusion in slowest list. Default 0.005
  -v, --verbose         increase verbosity.
  --no-header           disable header
  --no-summary          disable summary
  -q, --quiet           decrease verbosity.
  --verbosity=VERBOSE   set verbosity. Default is 0.
  -r chars              show extra test summary info as specified by chars: (f)ailed, (E)rror, (s)kipped, (x)failed, (X)passed, (p)assed, (P)assed with output, (a)ll except passed (p/P), or (A)ll. (w)arnings are enabled by default (see

                        --disable-warnings), 'N' can be used to reset the list. (default: 'fE').
  --disable-warnings, --disable-pytest-warnings
                        disable warnings summary
  -l, --showlocals      show locals in tracebacks (disabled by default).
  --tb=style            traceback print mode (auto/long/short/line/native/no).
  --show-capture={no,stdout,stderr,log,all}
                        Controls how captured stdout/stderr/log is shown on failed tests. Default is 'all'.
  --full-trace          don't cut any tracebacks (default is to cut).
  --color=color         color terminal output (yes/no/auto).
  --code-highlight={yes,no}
                        Whether code should be highlighted (only if --color is also enabled)
  --pastebin=mode       send failed|all info to bpaste.net pastebin service.
  --junit-xml=path      create junit-xml style report file at given path.
  --junit-prefix=str    prepend prefix to classnames in junit-xml output

pytest-warnings:
  -W PYTHONWARNINGS, --pythonwarnings=PYTHONWARNINGS
                        set which warnings to report, see -W option of python itself.
  --maxfail=num         exit after first num failures or errors.
  --strict-config       any warnings encountered while parsing the `pytest` section of the configuration file raise errors.
  --strict-markers      markers not registered in the `markers` section of the configuration file raise errors.
  --strict              (deprecated) alias to --strict-markers.
  -c file               load configuration from `file` instead of trying to locate one of the implicit configuration files.
  --continue-on-collection-errors
                        Force test execution even if collection errors occur.
  --rootdir=ROOTDIR     Define root directory for tests. Can be relative path: 'root_dir', './root_dir', 'root_dir/another_dir/'; absolute path: '/home/user/root_dir'; path with variables: '$HOME/root_dir'.

collection:
  --collect-only, --co  only collect tests, don't execute them.
  --pyargs              try to interpret all arguments as python packages.
  --ignore=path         ignore path during collection (multi-allowed).
  --ignore-glob=path    ignore path pattern during collection (multi-allowed).
  --deselect=nodeid_prefix
                        deselect item (via node id prefix) during collection (multi-allowed).
  --confcutdir=dir      only load conftest.py's relative to specified dir.
  --noconftest          Don't load any conftest.py files.
  --keep-duplicates     Keep duplicate tests.
  --collect-in-virtualenv
                        Don't ignore tests in a local virtualenv directory
  --import-mode={prepend,append,importlib}
                        prepend/append to sys.path when importing test modules and conftest files, default is to prepend.
  --doctest-modules     run doctests in all .py modules
  --doctest-report={none,cdiff,ndiff,udiff,only_first_failure}
                        choose another output format for diffs on doctest failure
  --doctest-glob=pat    doctests file matching pattern, default: test*.txt
  --doctest-ignore-import-errors
                        ignore doctest ImportErrors
  --doctest-continue-on-failure
                        for a given doctest, continue to run after the first failure

test session debugging and configuration:
  --basetemp=dir        base temporary directory for this test run.(warning: this directory is removed if it exists)
  -V, --version         display pytest version and information about plugins. When given twice, also display information about plugins.
  -h, --help            show help message and configuration info
  -p name               early-load given plugin module name or entry point (multi-allowed).
                        To avoid loading of plugins, use the `no:` prefix, e.g. `no:doctest`.
  --trace-config        trace considerations of conftest.py files.
  --debug=[DEBUG_FILE_NAME]
                        store internal tracing debug information in this log file.
                        This file is opened with 'w' and truncated as a result, care advised.
                        Defaults to 'pytestdebug.log'.
  -o OVERRIDE_INI, --override-ini=OVERRIDE_INI
                        override ini option with "option=value" style, e.g. `-o xfail_strict=True -o cache_dir=cache`.
  --assert=MODE         Control assertion debugging tools.
                        'plain' performs no assertion debugging.
                        'rewrite' (the default) rewrites assert statements in test modules on import to provide assert expression information.
  --setup-only          only setup fixtures, do not execute tests.
  --setup-show          show setup of fixtures while executing tests.
  --setup-plan          show what fixtures and tests would be executed but don't execute anything.

logging:
  --log-level=LEVEL     level of messages to catch/display.
                        Not set by default, so it depends on the root/parent log handler's effective level, where it is "WARNING" by default.
  --log-format=LOG_FORMAT
                        log format as used by the logging module.
  --log-date-format=LOG_DATE_FORMAT
                        log date format as used by the logging module.
  --log-cli-level=LOG_CLI_LEVEL
                        cli logging level.
  --log-cli-format=LOG_CLI_FORMAT
                        log format as used by the logging module.
  --log-cli-date-format=LOG_CLI_DATE_FORMAT
                        log date format as used by the logging module.
  --log-file=LOG_FILE   path to a file when logging will be written to.
  --log-file-level=LOG_FILE_LEVEL
                        log file logging level.
  --log-file-format=LOG_FILE_FORMAT
                        log format as used by the logging module.
  --log-file-date-format=LOG_FILE_DATE_FORMAT
                        log date format as used by the logging module.
  --log-auto-indent=LOG_AUTO_INDENT
                        Auto-indent multiline messages passed to the logging module. Accepts true|on, false|off or an integer.

[pytest] ini-options in the first pytest.ini|tox.ini|setup.cfg file found:

  markers (linelist):   markers for test functions
  empty_parameter_set_mark (string):
                        default marker for empty parametersets
  norecursedirs (args): directory patterns to avoid for recursion
  testpaths (args):     directories to search for tests when no files or directories are given in the command line.
  filterwarnings (linelist):
                        Each line specifies a pattern for warnings.filterwarnings. Processed after -W/--pythonwarnings.
  usefixtures (args):   list of default fixtures to be used with this project
  python_files (args):  glob-style file patterns for Python test module discovery
  python_classes (args):
                        prefixes or glob names for Python test class discovery
  python_functions (args):
                        prefixes or glob names for Python test function and method discovery
  disable_test_id_escaping_and_forfeit_all_rights_to_community_support (bool):
                        disable string escape non-ascii characters, might cause unwanted side effects(use at your own risk)
  console_output_style (string):
                        console output: "classic", or with additional progress information ("progress" (percentage) | "count").
  xfail_strict (bool):  default for the strict parameter of xfail markers when not given explicitly (default: False)
  enable_assertion_pass_hook (bool):
                        Enables the pytest_assertion_pass hook.Make sure to delete any previously generated pyc cache files.
  junit_suite_name (string):
                        Test suite name for JUnit report
  junit_logging (string):
                        Write captured log messages to JUnit report: one of no|log|system-out|system-err|out-err|all
  junit_log_passing_tests (bool):
                        Capture log information for passing tests to JUnit report:
  junit_duration_report (string):
                        Duration time to report: one of total|call
  junit_family (string):
                        Emit XML for schema: one of legacy|xunit1|xunit2
  doctest_optionflags (args):
                        option flags for doctests
  doctest_encoding (string):
                        encoding used for doctest files
  cache_dir (string):   cache directory path.
  log_level (string):   default value for --log-level
  log_format (string):  default value for --log-format
  log_date_format (string):
                        default value for --log-date-format
  log_cli (bool):       enable log display during test run (also known as "live logging").
  log_cli_level (string):
                        default value for --log-cli-level
  log_cli_format (string):
                        default value for --log-cli-format
  log_cli_date_format (string):
                        default value for --log-cli-date-format
  log_file (string):    default value for --log-file
  log_file_level (string):
                        default value for --log-file-level
  log_file_format (string):
                        default value for --log-file-format
  log_file_date_format (string):
                        default value for --log-file-date-format
  log_auto_indent (string):
                        default value for --log-auto-indent
  pythonpath (paths):   Add paths to sys.path
  faulthandler_timeout (string):
                        Dump the traceback of all threads if a test takes more than TIMEOUT seconds to finish.
  addopts (args):       extra command line options
  minversion (string):  minimally required pytest version
  required_plugins (args):
                        plugins that must be present for pytest to run

environment variables:
  PYTEST_ADDOPTS           extra command line options
  PYTEST_PLUGINS           comma-separated plugins to load during startup
  PYTEST_DISABLE_PLUGIN_AUTOLOAD set to disable plugin auto-loading
  PYTEST_DEBUG             set to enable debug tracing of pytest's internals


to see available markers type: pytest --markers
to see available fixtures type: pytest --fixtures
(shown according to specified file_or_dir or current dir if not specified; fixtures with leading '_' are only shown with the '-v' option

```


* x:用例一旦失败(fail/error) 就立即停止执行
命令行执行首先执行如下命令,运行所有case
示例：
```sh
pytest -vs .\test_pytest.py
```

输出：
``` 
collected 5 items                                                                                                                                                                                                                         

test_pytest.py::test_case_1 FAILED
test_pytest.py::test_case_2 FAILED
test_pytest.py::test_case_3 PASSED
test_pytest.py::test_case_sum_5 FAILED
test_pytest.py::test_case_sum_6 PASSED

================================================================================================================ FAILURES ================================================================================================================
______________________________________________________________________________________________________________ test_case_1 _______________________________________________________________________________________________________________

    def test_case_1():
>       assert inc(3) == 3
E       assert 4 == 3
E        +  where 4 = inc(3)

test_pytest.py:9: AssertionError
______________________________________________________________________________________________________________ test_case_2 _______________________________________________________________________________________________________________

    def test_case_2():
>       assert inc(3) == 1
E       assert 4 == 1
E        +  where 4 = inc(3)

test_pytest.py:13: AssertionError
____________________________________________________________________________________________________________ test_case_sum_5 _____________________________________________________________________________________________________________

    def test_case_sum_5():
>       assert inc(4) == 4
E       assert 5 == 4
E        +  where 5 = inc(4)

test_pytest.py:21: AssertionError
======================================================================================================== short test summary info =========================================================================================================
FAILED test_pytest.py::test_case_1 - assert 4 == 3
FAILED test_pytest.py::test_case_2 - assert 4 == 1
FAILED test_pytest.py::test_case_sum_5 - assert 5 == 4
====================================================================================================== 3 failed, 2 passed in 0.05s =======================================================================================================

```

说明：可以看到执行了模块内所有测试用例，3 failed, 2 passed。

* --lf:只重新运行故障用例
命令行执行执行如下命令,运行所有case
示例：
```sh
pytest -vs .\test_pytest.py --lf
```

输出：
``` 
collected 5 items / 2 deselected / 3 selected                                                                                                                                                                                             
run-last-failure: rerun previous 3 failures

test_pytest.py::test_case_1 FAILED
test_pytest.py::test_case_2 FAILED
test_pytest.py::test_case_sum_5 FAILED

================================================================================================================ FAILURES ================================================================================================================
______________________________________________________________________________________________________________ test_case_1 _______________________________________________________________________________________________________________

    def test_case_1():
>       assert inc(3) == 3
E       assert 4 == 3
E        +  where 4 = inc(3)

test_pytest.py:9: AssertionError
______________________________________________________________________________________________________________ test_case_2 _______________________________________________________________________________________________________________

    def test_case_2():
>       assert inc(3) == 1
E       assert 4 == 1
E        +  where 4 = inc(3)

test_pytest.py:13: AssertionError
____________________________________________________________________________________________________________ test_case_sum_5 _____________________________________________________________________________________________________________

    def test_case_sum_5():
>       assert inc(4) == 4
E       assert 5 == 4
E        +  where 5 = inc(4)

test_pytest.py:21: AssertionError
======================================================================================================== short test summary info =========================================================================================================
FAILED test_pytest.py::test_case_1 - assert 4 == 3
FAILED test_pytest.py::test_case_2 - assert 4 == 1
FAILED test_pytest.py::test_case_sum_5 - assert 5 == 4
==================================================================================================== 3 failed, 2 deselected in 0.04s =====================================================================================================

```

说明：可以看到只运行了失败的用例

* --ff:先运行故障用例 再运行其它用例测试

命令行执行执行如下命令,运行所有case
示例：
```sh
pytest -vs .\test_pytest.py --ff
```

输出：
```
collected 5 items                                                                                                                                                                                                                         
run-last-failure: rerun previous 3 failures first

test_pytest.py::test_case_1 FAILED
test_pytest.py::test_case_2 FAILED
test_pytest.py::test_case_sum_5 FAILED
test_pytest.py::test_case_3 PASSED
test_pytest.py::test_case_sum_6 PASSED

================================================================================================================ FAILURES ================================================================================================================
______________________________________________________________________________________________________________ test_case_1 _______________________________________________________________________________________________________________

    def test_case_1():
>       assert inc(3) == 3
E       assert 4 == 3
E        +  where 4 = inc(3)

test_pytest.py:9: AssertionError
______________________________________________________________________________________________________________ test_case_2 _______________________________________________________________________________________________________________

    def test_case_2():
>       assert inc(3) == 1
E       assert 4 == 1
E        +  where 4 = inc(3)

test_pytest.py:13: AssertionError
____________________________________________________________________________________________________________ test_case_sum_5 _____________________________________________________________________________________________________________

    def test_case_sum_5():
>       assert inc(4) == 4
E       assert 5 == 4
E        +  where 5 = inc(4)

test_pytest.py:21: AssertionError
======================================================================================================== short test summary info =========================================================================================================
FAILED test_pytest.py::test_case_1 - assert 4 == 3
FAILED test_pytest.py::test_case_2 - assert 4 == 1
FAILED test_pytest.py::test_case_sum_5 - assert 5 == 4
====================================================================================================== 3 failed, 2 passed in 0.05s =======================================================================================================

```

说明：可以看到先运行了故障用例，再运行了其它用例

命令行执行执行如下命令,运行所有case
示例：
```sh
pytest -vs .\test_pytest.py -x
```

输出：
``` 
ollected 5 items                                                                                                                                                                                                                         

test_pytest.py::test_case_1 FAILED

================================================================================================================ FAILURES ================================================================================================================
______________________________________________________________________________________________________________ test_case_1 _______________________________________________________________________________________________________________

    def test_case_1():
>       assert inc(3) == 3
E       assert 4 == 3
E        +  where 4 = inc(3)

test_pytest.py:9: AssertionError
======================================================================================================== short test summary info =========================================================================================================
FAILED test_pytest.py::test_case_1 - assert 4 == 3
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! stopping after 1 failures !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
=========================================================================================================== 1 failed in 0.04s ============================================================================================================
```

说明：可以看到，当运行到第一条错误case时便停止继续运行了，stopping after 1 failures/1 failed in 0.04s


* –maxfail=num:用例用例失败数达到多少时停止执行(num：失败的用例数)


命令行执行执行如下命令,运行所有case
示例：
```sh
pytest -vs .\test_pytest.py --maxfail=2
```

输出：
``` 
collected 5 items                                                                                                                                                                                                                         

test_pytest.py::test_case_1 FAILED
test_pytest.py::test_case_2 FAILED

================================================================================================================ FAILURES ================================================================================================================
______________________________________________________________________________________________________________ test_case_1 _______________________________________________________________________________________________________________

    def test_case_1():
>       assert inc(3) == 3
E       assert 4 == 3
E        +  where 4 = inc(3)

test_pytest.py:9: AssertionError
______________________________________________________________________________________________________________ test_case_2 _______________________________________________________________________________________________________________

    def test_case_2():
>       assert inc(3) == 1
E       assert 4 == 1
E        +  where 4 = inc(3)

test_pytest.py:13: AssertionError
======================================================================================================== short test summary info =========================================================================================================
FAILED test_pytest.py::test_case_1 - assert 4 == 3
FAILED test_pytest.py::test_case_2 - assert 4 == 1
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! stopping after 2 failures !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
=========================================================================================================== 2 failed in 0.04s ============================================================================================================

```

说明：--maxfail=2,参数设置了错误case数为4，可以看到当错误case数达到2条时便停止了执行(stopping after 2 failures /2 failed in 0.04s)


* -m:标记用例（重点）
该参数用法较复杂，后面单独介绍

* -k:执行包含某个关键字的测试用例
命令行执行执行如下命令,运行所有case
示例：
```sh
pytest -vs .\test_pytest.py -k "sum"
```

输出：
``` 
collected 5 items / 3 deselected / 2 selected                                                                                                                                                                                             

test_pytest.py::test_case_sum_5 FAILED
test_pytest.py::test_case_sum_6 PASSED

================================================================================================================ FAILURES ================================================================================================================
____________________________________________________________________________________________________________ test_case_sum_5 _____________________________________________________________________________________________________________

    def test_case_sum_5():
>       assert inc(4) == 4
E       assert 5 == 4
E        +  where 5 = inc(4)

test_pytest.py:21: AssertionError
======================================================================================================== short test summary info =========================================================================================================
FAILED test_pytest.py::test_case_sum_5 - assert 5 == 4
=============================================================================================== 1 failed, 1 passed, 3 deselected in 0.04s ================================================================================================

```

说明：可以看到 只执行了带有"sum"关键字的case

### Mark标记测试用例

* 场景：只执行 符合要求的某一部分用例，可以把一个web项目划分多个模块，然后制定模块名称执行
* 解决：在测试用例方法上添加@pytest.mark.标签名
* 执行：-m 参数执行自定义标签的相关用例

``` 
pytest -s test_mark_demo.py -m=int
pytest -s test_mark_demo.py -m str
pytest -s test_mark_demo.py -m "not str"
```

示例：
新建test_mark_demo.py文件，复制如下代码
``` 
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import pytest


def double(a):
    return a * 2


# 测试数据：整型
@pytest.mark.int
def test_double_int():
    print("double_int")
    assert 2 == double(1)


# 测试数据：负数
@pytest.mark.minus
def test_double_minus():
    print("double_minus")
    assert -2 == double(-1)


# 测试数据：浮点数
@pytest.mark.float
def test_double_float():
    print("double_float")
    assert 0.2 == double(0.1)


# 测试数据：浮点数
@pytest.mark.float
def test_double2_float():
    print("double2_float")
    assert -10.2 == double(-0.1)


@pytest.mark.zero
def test_double_zero():
    print("double_zero")
    assert 10 == double(0)


@pytest.mark.bignum
def test_double_bignum():
    print("double_bignum")
    assert 200 == double(100)


@pytest.mark.str
def test_double_str():
    print("double_str")
    assert "aa" == double("a")


@pytest.mark.str
def test_double2_str():
    print("double2_str")
    assert "a$a$" == double("a$")

```

示例：执行如下命令
``` 
pytest -vs .\test_mark_demo.py -m=str
```

输出：
``` 
collected 8 items / 6 deselected / 2 selected                                                                                                                                                                                             

test_mark_demo.py::test_double_str double_str
PASSED
test_mark_demo.py::test_double2_str double2_str
PASSED

============================================================================================================ warnings summary ============================================================================================================
test_mark_demo.py:12
  E:\WorkSpace\hogwarts\test_pytest\test_mark_demo.py:12: PytestUnknownMarkWarning: Unknown pytest.mark.int - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/how
-to/mark.html
    @pytest.mark.int

test_mark_demo.py:19
  E:\WorkSpace\hogwarts\test_pytest\test_mark_demo.py:19: PytestUnknownMarkWarning: Unknown pytest.mark.minus - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/h
ow-to/mark.html
    @pytest.mark.minus

test_mark_demo.py:26
  E:\WorkSpace\hogwarts\test_pytest\test_mark_demo.py:26: PytestUnknownMarkWarning: Unknown pytest.mark.float - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/h
ow-to/mark.html
    @pytest.mark.float

test_mark_demo.py:33
  E:\WorkSpace\hogwarts\test_pytest\test_mark_demo.py:33: PytestUnknownMarkWarning: Unknown pytest.mark.float - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/h
ow-to/mark.html
    @pytest.mark.float

test_mark_demo.py:39
  E:\WorkSpace\hogwarts\test_pytest\test_mark_demo.py:39: PytestUnknownMarkWarning: Unknown pytest.mark.zero - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/ho
w-to/mark.html
    @pytest.mark.zero

test_mark_demo.py:45
  E:\WorkSpace\hogwarts\test_pytest\test_mark_demo.py:45: PytestUnknownMarkWarning: Unknown pytest.mark.bignum - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/
how-to/mark.html
    @pytest.mark.bignum

test_mark_demo.py:51
  E:\WorkSpace\hogwarts\test_pytest\test_mark_demo.py:51: PytestUnknownMarkWarning: Unknown pytest.mark.str - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/how
-to/mark.html
    @pytest.mark.str

test_mark_demo.py:57
  E:\WorkSpace\hogwarts\test_pytest\test_mark_demo.py:57: PytestUnknownMarkWarning: Unknown pytest.mark.str - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/how
-to/mark.html
    @pytest.mark.str

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
============================================================================================== 2 passed, 6 deselected, 8 warnings in 0.01s ===============================================================================================
```

说明：可以看到只执行了2条标记有 str 关键字的case；除此之外还存在8条警告，出现警告的原因是没有注册标签；
新建一个pytest.ini文件，复制如下内容注册标签，然后再执行，就不会再出现警告
``` 
[pytest]
markers = str
          bignum
          float
          int
          zero
          minus
```

### Mark跳过测试用例以及预期失败

* 这是pytest的内置标签，可以处理一些特殊的测试用例，不能成功的测试用例
* skip:始终跳过该测试用例
* skipif:遇见特定情况跳过该测试用例
* xFail:遇见特定情况，产生一个"期望失败"输出

解决1：添加装饰器

``` 
@pytest.mark.skip
@pytest.mark.skipif
@pytest.mark.xfail
```

解决2：代码中添加跳过代码
``` 
@pytest.skip(reason)
```

示例：
新建test_skip.py文件，复制如下代码
``` 
import pytest
import sys


# 跳过case
@pytest.mark.skip
def test_aaa():
    assert True


@pytest.mark.skip(reason="代码未实现")
def test_bbb():
    assert False


# 根据条件判断跳过
print(sys.platform)


@pytest.mark.skipif(sys.platform == "darwn", reason="dose not run on mac")
def test_caase1():
    assert True


@pytest.mark.skipif(sys.platform == "win32", reason="dose not run on windows")
def test_case2():
    assert True


# 代码中跳过
def check_login():
    return False


def test_function():
    print("start")
    if not check_login():
        pytest.skip("登录失败，后续不用在执行")
    print("end")


# 预期错误
@pytest.mark.xfail
def test_case_xfail():
    print("test_xfail 方法执行1")
    assert 2 == 2


@pytest.mark.xfail
def test_case_xfail2():
    print("test_xfail 方法执行2")
    assert 1 == 2


xfail = pytest.mark.xfail


@xfail(reason="bug 110")
def test_case_hello():
    assert 0


def test_xfail():
    print("start")
    pytest.mark.xfail(reason="功能暂未完成")
    print("测试过程")
    assert 1 == 1

```

示例：执行如下命令

``` 
 pytest -vs .\test_skip.py
```

输出：
``` 
collecting ... win32
collected 9 items                                                                                                                                                                                                                         

test_skip.py::test_aaa SKIPPED (unconditional skip)
test_skip.py::test_bbb SKIPPED (代码未实现)
test_skip.py::test_caase1 PASSED
test_skip.py::test_case2 SKIPPED (dose not run on windows)
test_skip.py::test_function start
SKIPPED (登录失败，后续不用在执行)
test_skip.py::test_case_xfail test_xfail 方法执行1
XPASS
test_skip.py::test_case_xfail2 test_xfail 方法执行2
XFAIL
test_skip.py::test_case_hello XFAIL (bug 110)
test_skip.py::test_xfail start
测试过程
PASSED

=========================================================================================== 2 passed, 4 skipped, 2 xfailed, 1 xpassed in 0.07s ===========================================================================================

```


### 参数化

参数化设计方法就是将模型中的定量信息变量化，使之成为任意调整的参数。
对于变量化参数赋予不同数值，就可得到不同大小和形状的零件模型。

mark参数化测试函数的使用
语法：
``` 
@pytest.mark.parametrize(argnames,argvalues)
argnames:要参数化的变量，string(逗号分割),list,tuple
argvalue:参数化的值，list,list[tuple]
```

* 单参数
* 多参数
* 用例重命名:通过ids参数实现
* 笛卡尔积：适用于数据组合情况

演示：新建test_mark_parmes.py,复制如下代码

``` 
#!/usr/bin/env python3
# -*- coding: utf-8 -*-


import pytest

search_list = ["appium", "selenium", "pytest"]


# 1.参数化的名字，要与方法中的参数名一一对应
# 2.如果传递多个参数的话，要放在列表中，列表中嵌套列表/元组
# 3.ids 用于给case命名，ids个数要与参数个数相对应

# 单参数
@pytest.mark.parametrize("name", search_list)
def test_search(name):
    assert name in search_list


# 多参数/case改名
@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+5", 7), ("7+5", 12)], ids=["case1", "case2", "case3"])
def test_mark_more(test_input, expected):
    assert eval(test_input) == expected


# 笛卡尔积
@pytest.mark.parametrize("wd", ["appium", "selenium", "pytest"])
@pytest.mark.parametrize("code", ["utf8", "gbk", "gbk2312"])
def test_dkej(wd, code):
    print("wd:{},code:{}".format(wd, code))


```

示例1：
运行单参数case，执行如下命令
``` 
 pytest -vs .\test_mark_parames.py::test_search
```

输出：
```
collected 3 items                                                                                                                                                                                                                         

test_mark_parames.py::test_search[appium] PASSED
test_mark_parames.py::test_search[selenium] PASSED
test_mark_parames.py::test_search[pytest] PASSED

=========================================================================================================== 3 passed in 0.01s ============================================================================================================
```

示例2：
运行多参数case并为case重命名，执行如下命令
``` 
pytest -vs .\test_mark_parames.py::test_mark_more
```

输出：
```
collected 3 items                                                                                                                                                                                                                         

test_mark_parames.py::test_mark_more[case1] PASSED
test_mark_parames.py::test_mark_more[case2] PASSED
test_mark_parames.py::test_mark_more[case3] PASSED

=========================================================================================================== 3 passed in 0.01s ============================================================================================================
```

示例2：
运行笛卡尔积case，执行如下命令
``` 
pytest -vs .\test_mark_parames.py::test_dkej
```

输出：
``` 
collected 9 items                                                                                                                                                                                                                         

test_mark_parames.py::test_dkej[utf8-appium] wd:appium,code:utf8
PASSED
test_mark_parames.py::test_dkej[utf8-selenium] wd:selenium,code:utf8
PASSED
test_mark_parames.py::test_dkej[utf8-pytest] wd:pytest,code:utf8
PASSED
test_mark_parames.py::test_dkej[gbk-appium] wd:appium,code:gbk
PASSED
test_mark_parames.py::test_dkej[gbk-selenium] wd:selenium,code:gbk
PASSED
test_mark_parames.py::test_dkej[gbk-pytest] wd:pytest,code:gbk
PASSED
test_mark_parames.py::test_dkej[gbk2312-appium] wd:appium,code:gbk2312
PASSED
test_mark_parames.py::test_dkej[gbk2312-selenium] wd:selenium,code:gbk2312
PASSED
test_mark_parames.py::test_dkej[gbk2312-pytest] wd:pytest,code:gbk2312
PASSED

=========================================================================================================== 9 passed in 0.01s ============================================================================================================
```

### 使用yaml文件进行参数化

yaml安装：

pycharm编译器打开settings->project->python interpreter,点击依赖包旁的'+'加号，搜索pyyaml
选择"PyYAML"（注意字母大小写，别装错了），点击下方install package

yaml基本用法：

* yaml实现list
``` 
list:
    - 10
    - 20
    - 30 
```

* yaml实现字典
``` 
dict:
    by:id
    locator:name
    action:click 
```

* yaml实现嵌套

``` 
-
    - by:id
    - locator:name
    - action:click 
```

* 加载yaml文件
``` 
yaml.safe_load(open(filepath))
```

代码演示：
创建data.yaml文件，复制如下内容

``` 
-
  - 10
  - 20

-
  - 20
  - 30

-
  - 40
  - 50
```

创建test_yaml.py文件，复制如下代码
``` 
import pytest
import yaml

class TestData:

    @pytest.mark.parametrize(("a","b"),yaml.safe_load(open("./data.yaml")))
    def test_data(self,a,b):
        print(a+b)
```

示例：
执行如下命令，运行case
``` 
pytest -vs test_yaml.py
```

输出：
``` 
collected 3 items                                                                                                                                                                                                                         

test_yaml.py::TestData::test_data[10-20] 30
PASSED
test_yaml.py::TestData::test_data[20-30] 50
PASSED
test_yaml.py::TestData::test_data[40-50] 90
PASSED

=========================================================================================================== 3 passed in 0.07s ============================================================================================================

```

### 异常处理

* try...except...
* pytest.raaises()

示例代码:
新建test_raises.py,复制如下代码

``` 
import pytest

# try方法

try:
    a = int(input("请输入被除数："))
    b = int(input("请输入除数："))
    c = a / b
    print("您输入的两个数相除结果为：", c)

except(ValueError, AttributeError):
    print("程序发生错误，数字格式异常")
except:
    print("未知错误")

print("继续运行")


# pytest.raises
def test_raise():
    with pytest.raises(ValueError) as exc_info:
        raise ValueError("value must be 42")
    assert exc_info.type in ValueError
    assert exc_info.value.args[0] == "value must be 42"
```

### 数据驱动

数据驱动就是数据的改变从而驱动自动化测试的执行，最终引起测试结果的改变。简单来说，就是参数化的应用。
数据量小的测试用例可以使用代码的参数化来实现数据驱动，数据量大的情况下建议大家使用一种结构化的文件(如yaml,json等)来
对数据进行存储，然后再测试用例中读取这些数据。

应用场景：

* App/Web/接口自动化测试
* 测试步骤的数据驱动
* 测试数据的数据驱动
* 配置的数据驱动

### Allure测试框架

* allure介绍
* allure安装
* Allure特性分析
* Allure运行
* Allure报告中嵌入文本、图片、视频等资源


介绍：
allure是一个轻量级、灵活的，支持多语言的测试报告工具
多平台的，奢华的report框架
可以为dev/qa提供详尽的测试报告，测试步骤，log等
也可以为管理层提供high level统计报告
Java语言开发的，支持pytest,JavaScript,PHP,ruby等
可以集成到Jenkins


allure常用特性：

场景：
希望在报告中看到测试功能，子功能或场景，测试步骤，包括测试附加信息

解决：
``` 
@Feature,@story,@step,@attach
```

步骤：
``` 
import allure

功能上加 @allure.feature('功能名称')
子功能上加@alure.story('子功能')
步骤上加 @allure.step('步骤细节')
@allure.attach('具体文本信息'),需要附加的信息可以是数据、文本、图片、视频、网页
如果只测试登录功能运行的时候可以加限制过滤：

pytest 文件名 --allure.ffeatures '购物车功能' --allure-stories '加入购物车'(注意这里 --allure-feature)
```




安装：
``` 
pip install allure-pytest
```

运行：
在执行测试期间收集结果
``` 
pytest [测试文件] -s -q --alluredir=./result/(指定存储目录)
```

查看测试报告：
方式一：测试完后查看实际报告，在线报告，会直接打开默认浏览器展示当前报告
``` 
allure serve ./result/
```

方式二：从结果生成报告，启动tomact服务，需要两个步骤，生成报告，打开报告

* 生成报告：
``` 
allure generate ./result/ -o ./report/ --clean(注意：覆盖路径加 --clean)
 
```

* 打开报告：
``` 
allure open -h 127.0.0.1 -p 8883 ./report/ 
```


### pytest插件开发

pytest插件分类

* 外部插件：
``` 
pip install 安装的插件 
```

* 本地插件：
``` 
pytest自动模块发现机制(conftest.py存放的)
```

* 内置插件：
``` 
代码内部的 _pytest目录加载 
```

pytest插件介绍
常用插件
```
pip install pytest-ordering  控制用例执行顺序(重点) 
pip install pytest-xdist  分布式并发执行测试用例(重点) 
pip install pytest-dependency 控制用例的依赖(了解) 
pip install pytest-rerunfailures 失败重跑(了解) 
pip install pytest-assume 多重校验(了解) 
pip install pytest-random-order 用例随机执行(了解) 
pip install pytest-html 测试报告(了解) 
```

pytest-ordering  控制用例执行顺序:

场景：
对于集成测试，经常会有上下文依赖关系的测试用例，比如10个步骤，拆成10条case，这时候需要能知道到底执行到
哪步报错
用例默认执行顺序：自上二而下执行

解决：
可以通过 setup/teardown 和 fixture解决，也可以使用插件解决

安装：
```
pip install pytest-ordering 
```
用法：

``` 
@pytest.mark.run(order=2)
```

注意：多个插件装饰器(>2)时，有可能会发生冲突




pytest-xdist  分布式并发执行测试用例:

场景1：
测试用例1000条，1个用例执行1分钟，一个测试人员需要1000分钟。
通常我们会用人力成本换取时间成本，加几个人一起执行，时间就会缩短。
如果10个人执行只需要100分钟，这就是一种分布式场景

场景2：
假设有个报名系统，对报名总数统计，数据同时进行修改操作时可能出现问题，
需要模拟这个场景，需要多用户并发请求数据。

解决：

使用分布式并发执行测试用例，分布式插件，pytest-xdist

安装：
```
pip install pytest-xdist
```

注意：用例多的时候效果明显，多进程并发执行，同时支持allure

使用：
``` 
pytest -n 4
```


pytest hook执行顺序

待补充

### pytest配置文件

待补充

### log日志模块

待补充

**持续补充ing~**
。。。

。。。



