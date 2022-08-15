---
title: unittest
date: 2021-11-08 14:48:16
categories: 
- 自动化测试
tags:
- unittest
---
---



# [unittes单元测试框架](https://docs.python.org/3/library/unittest.html)


### 什么是单元测试：

单元测试是开发人员编写的一小段代码，用于检验被测代码的一个很小的，很明确的功能是否正确。也可以理解为单元测试是用于判断摸个特定条件或者场景下某个特定函数的行为，通常由开发自测。
	


### unittest:

python自带的单元测试框架。
python 内置标准类库，跟Java的JUnit、 .net的NUnit  C++的 CppUnit很相似

### 组成：

Unittest由 test case、test suit、test fixures、test runner相关组件构成。

```
test fixtrue:测试装置，在测试之前或测之后要完成的一些操作。
    * setUp
    * setUpClass
    * tearDown
    * tearDownClass
    
test case : 编写的测试用例
test suite ：组装测试用例
test runner:  执行测试用例
```

		
### 规则：	

* 用例编写规则，类必须继承 unittest.TestCase
* case：必须以test开头
* setUp(): 准备测试环境，每执行一条case之前都会执行setUp
* tearDown(): 清里测试环境，每执行一条case都会执行tearDown
* setUpClass(): 在所有case执行前执行
* tearDownClass(): 在所有case都执行完毕后执行
* @unittest.skip(): 跳过用例

### setuo与teardown:

基于测试方法级别：setup、teardown。执行每个测试方法之前执行一次setup,执行每个测试方法之后执行teardown。

基于类级别的setupClass、teardownClass:执行测试类里所有方法之前执行steupClass,之后执行teardownClass

### 示例：

```
import unittest


class Search():
    def search_fun(self):
        print("search")
        return True


class TestSearch(unittest.TestCase):

    @classmethod
    def setUpClass(cls) -> None:
        print("setup class")
        cls.search = Search()

    @classmethod
    def tearDownClass(cls) -> None:
        print("teardown class")

    def test_search1(self):
        print("test search 1")
        # search = Search()
        assert True == self.search.search_fun()

    def test_search2(self):
        print("test search 2")
        # search = Search()
        assert True == self.search.search_fun()

    def test_search3(self):
        print("test search 3")
        # search = Search()
        assert True == self.search.search_fun()


if __name__ == '__main__':
    unittest.main()
```

### 用例管理

多条测试用例组成的集合就是测试套件，通过测试套件来管理测试用例
执行顺序： unittest执行测试用例，默认是根据ASCII码的顺序加载测试用例，数字与字母的顺序为：0-9，A-Z，a-z 

方式一：通过main方法直接运行
``` 
import unittest


class Search():

    def search_fun(self):
        print("search")
        return True


class TestSearch(unittest.TestCase):

    @classmethod
    def setUpClass(cls) -> None:
        print("setup class")
        cls.search = Search()

    @classmethod
    def tearDownClass(cls) -> None:
        print("teardown class")

    def test_search1(self):
        print("test search 1")
        # search = Search()
        assert True == self.search.search_fun()

    def test_search2(self):
        print("test search 2")
        # search = Search()
        assert True == self.search.search_fun()

    def test_search3(self):
        print("test search 3")
        # search = Search()
        assert True == self.search.search_fun()


if __name__ == '__main__':

    unittest.main()
```

方式二：创建测试集合suite,通过addTest将测试用例添加到测试集合中，最后运行测试集合(case会按照添加的前后顺序执行，可确保case的执行顺序不会改变)
``` 
import unittest


class Search():

    def search_fun(self):
        print("search")
        return True


class TestSearch(unittest.TestCase):

    @classmethod
    def setUpClass(cls) -> None:
        print("setup class")
        cls.search = Search()

    @classmethod
    def tearDownClass(cls) -> None:
        print("teardown class")

    def test_search1(self):
        print("test search 1")
        # search = Search()
        assert True == self.search.search_fun()

    def test_search2(self):
        print("test search 2")
        # search = Search()
        assert True == self.search.search_fun()

    def test_search3(self):
        print("test search 3")
        # search = Search()
        assert True == self.search.search_fun()


if __name__ == '__main__':

    suite = unittest.TestSuite()
    suite.addTest(TestSearch("test_search3"))
    suite.addTest(TestSearch("test_search2"))
    suite.addTest(TestSearch("test_search1"))
    unittest.TextTestRunner().run(suite)
   
# 注意case添加到集合的顺序，以及执行的顺序
```

方式三：同时执行多个类
```
import unittest


class Search():

    def search_fun(self):
        print("search")
        return True


class TestSearch(unittest.TestCase):

    @classmethod
    def setUpClass(cls) -> None:
        print("setup class")
        cls.search = Search()

    @classmethod
    def tearDownClass(cls) -> None:
        print("teardown class")

    def test_search1(self):
        print("test search 1")
        # search = Search()
        assert True == self.search.search_fun()

    def test_search2(self):
        print("test search 2")
        # search = Search()
        assert True == self.search.search_fun()

    def test_search3(self):
        print("test search 3")
        # search = Search()
        assert True == self.search.search_fun()


class TestSearch2(unittest.TestCase):

    @classmethod
    def setUpClass(cls) -> None:
        print("setup class")
        cls.search = Search()

    @classmethod
    def tearDownClass(cls) -> None:
        print("teardown class")

    def test_search4(self):
        print("test search 4")
        # search = Search()
        assert True == self.search.search_fun()


if __name__ == '__main__':
    #unittest.main()
    suite1 = unittest.TestLoader().loadTestsFromTestCase(TestSearch)
    suite2 = unittest.TestLoader().loadTestsFromTestCase(TestSearch2)
    suite = unittest.TestSuite([suite1, suite2])
    unittest.TextTestRunner(verbosity=2).run(suite)
```


方式四：匹配某个目录下所有的test开头的.py,自动加载py文件内符合条件的case
```
dir:case文件存放目录
pattern:case文件匹配规则，默认为 pattern='test*.py'


dir='./test_case'
discover=unittest.defaultTestLoader.discover(start_dir=dir,pattern='test*.py')
```
### 断言：用来判断case执行结果是否符合预期结果


| 断言方法 | 含义                                      |
|:--| :--------------------------------------- |
| assertEqual(a, b) |a == b。                  |
| assertNotEqual(a, b) |a ！= b。                  |
| assertTrue(x) |bool(x) is True。           |
| assertFalse(x) |bool(x) is False。               |
| assertIs(a, b) |a is b。               |
| assertIsNot(a, b)|a is not b。 |
| assertIsNone(x)|x is None。 |
| assertIsNotNone(x)|x is not None。 |
| assertIn(a, b)|a in b。 |
| assertNotIn(a, b)|a not in b。 |
| assertIsInstance(a, b)|isinstance(a, b)。 |
| assertNotIsInstance(a, b)|not isinstance(a, b)。 |


### 生成测试报告：

可以借助HTMLTestRunner生成HTML形式的测试报告。















 
        
       
    
