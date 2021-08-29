---
title: UnitTest
date: 2021-08-29 22:59:45
categories: 
- Python
tags:
- 自动化测试
---


# [unittes单元测试框架](https://docs.python.org/3/library/unittest.html)


<br/>

## 什么是单元测试：
	单元测试是开发人员编写的一小段代码，用于检验被测代码的一个很小的，很明确的功能是否正确。也可以理解为单元测试是用于判断摸个特定条件或者场景下某个特定函数的行为，通常由开发自测。
	


## unittes:

### python 内置标准类库，跟Java的JUnit、 .net的NUnit  C++的 CppUnit很相似
	组成：
		test fixtrue:测试装置，在测试之前或测之后要完成哪些操作
			* setUp
			* setUpClass
			* tearDown
			* tearDownClass
		test case : 编写的测试用例
		test suite ：组装测试用例
		test runner:  执行测试用例
		
	- 规则：	
		用例编写规则，类必须继承 unittest.TestCase
		case：必须以test开头
		setUp(): 准备测试环境，每执行一条case之前都会执行setUp
		tearDown(): 清里测试环境，每执行一条case都会执行tearDown
		setUpClass(): 在所有case执行前执行
		tearDownClass(): 在所有case都执行完毕后执行
		@unittest.skip(): 跳过用例

### 示例：
    import unittest
    class TestDemo(unittest.TestCase): 
        @classmethod
        def setUpClass(cls):
            print("这是测试整个类之前要执行的方法 setUpClass\n")

        def setUp(self):
            print('这是每一个测试方法前面运行的方法 setUp\n')

        def test_first(self):
            print('这是测试方法1\n')

        @unittest.skip('跳过')
        def test_second(self):
            print('这是测试方法2\n')

        def tearDown(self):
            print('这是每一个测试方法后面运行的方法 tearDown\n')

        @classmethod
        def tearDownClass(cls):
            print("这是测试整个类之后要执行的方法 tearDownClass\n")

    if __name__ == '__main__':
        #执行全部case
        unittest.main()
        
        
    - print：
        这是测试整个类之前要执行的方法 setUpClass
        这是测试整个类之后要执行的方法 tearDownClass
        这是每一个测试方法前面运行的方法 setUp
        这是测试方法1
        这是每一个测试方法后面运行的方法 tearDown
 
### 用例管理
    1. 执行顺序： unittest执行测试用例，默认是根据ASCII码的顺序加载测试用例，数字与字母的顺序为：0-9，A-Z，a-z 
        import unittest
        class TestDemo(unittest.TestCase): 

            def test_first(self):
                print('这是测试方法1\n')

            
            def test_second(self):
                print('这是测试方法2\n')
                
            def test_abc(self):
                print('这是测试方法3\n')
                
            def test_abc_02(self):
                print('这是测试方法4\n')
                
            def test_abc_01(self):
                print('这是测试方法5\n')

        if __name__ == '__main__':
            #执行全部case
            unittest.main()
        - print：
            这是测试方法3
            这是测试方法5
            这是测试方法4
            这是测试方法1
            这是测试方法2
            
    2. 用例管理: 
<br/>

# 未完待续，敬请期待~





















 
        
       
    