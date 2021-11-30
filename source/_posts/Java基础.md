---
title: Java
date: 2021-11-30 22:39:39
categories: 
- 测试开发
tags:
- Java
---


##  [Java基础 ](https://www.bilibili.com/video/BV1Lf4y1U7Cz?spm_id_from=333.999.0.0)


### 基础

1.  基础语法
**Helloword入门程序**
```sh
// 这是一行注释
public class Helloword { // 这是一行注释
  public static void main(String[] args){
		/*
		 这是一行注释
		 这是一行注释
		 这是一行注释
		*/
		  System.out.println("Hello word");
	  }
}
```
	
编译：javac  Helloword.java ,生成 Helloword.class 文件
执行：java Helloword.class
输出：
```sh
	Hello World
```
**注释**
 -  // 表示但阿訇注释
 -  /*  */ 表示多行注释

**关键字**
- 特点：
	-  完全小写的字母
	-  编译器中有特殊颜色

**标识符**
- 是指在程序中，我们自己定义的内容，比如类的名字，方法名字，变量名字等等都是标识符
- 命名规则：
	- 可以包含英文字母(区分大小写)，数字，$和下划线_
	- 标识符不可以以数字开头
	-  标识符不能是关键字
- 命名规范:
	- 类名规范
		- 首字母大写，后面每个字母大写(大驼峰式)
	- 变量名规范
		- 首字母小写，后面每个单词字母大写(小驼峰)
	- 方法名称规范:
		- 同变量名
			
**常量**
-  常量：在程序运行期间，固定不变的量称为常量
-  分类
	1. 字符串常量：凡是用双引号引起来的部分，叫做字符串常量		// "abc"   "123"
	2. 整数常量： 直接写的数字，没有小数点
	3. 浮点数常量：直接写上的数字，但有小数
	4. 字符常量：凡是用单引号引起来的单个字符，有且仅有一个字符，没有不行, 叫做字符常量  // ‘A’   '9' ，
	5. 布尔常量：true / false
	6. 空常量：null ,没有是任何数据
	```sh 
	public class Demo01Const {
    public static void main(String []args) {
		
		//字符串常量
       System.out.println("Hello World");
       System.out.println("abc");
       System.out.println("");
       System.out.println("123");
	   
	   //整数常量
	   System.out.println(30);
	   System.out.println(-530);
	   
	   //浮点数常量
	   System.out.println(3.14);
	   System.out.println(-2.5);

		//字符常量
		System.out.println('a');
		System.out.println('6');
		//System.out.println('');  两个单引号之间必须有且仅有一个字符，没有不行，两个也不行
		//System.out.println('ab');
		
		//布尔常量
		System.out.println(true);
		System.out.println(false);
		
		//空常量
		//System.out.println(null); 空常量不能直接用来打印输出
	
    }
}

	```
 输出：
 ```sh
 Hello World
	abc
	
	123
	30
	-530
	3.14
	-2.5
	a
	6
	true
	false
 ```

**基本数据类型   P19**


2. 面向对象

3. 继承与多态

4. 常用API

### 进阶

1.  JDK新特性
2.  集合
3.  异常
4.  多线程

### File类与IO流

### 网络编程

# 持续更新中~
