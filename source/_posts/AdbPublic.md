---
title: AdbPublic
categories: 
- 测试开发
tags:
- adb
---

##  简介

在通过uiautomator2做UI自动化测试的过程中，发现一些元素 u2无法获取，这里通

过adb来实现对元素的操作

### 思路

通过 adb dump 获取页面的xml结构树并pull发送到本地目录进行解析，在解析过程中发现note 节点下可以找到这些属性：text，resource-id，class，bounds，而 bounds 表示的是 元素的坐标(x,y),有了坐标之后就可以通过 adb shell input tap x,y来实现对元素的点击操作，其它操作动作动作同理。这里只封装了点击事件。

###  实现代码

```sh
#!/usr/bin/python
# -*- coding: utf-8 -*-
# @Emailn : honlicc@163.com

'''
封装了adb的部分操作，用来处理uiautomatow2处理不了的元素事件
'''
import os
import re

from conf.base_config import base_path #项目根目录路径
import xml.etree.cElementTree as ET


class FindElementsDemo(object):

    def __init__(self):
	   #xml存放到 apk目录下 
        self.tempFile=os.path.join(base_path,"apk")
        self.pattern = re.compile(r"\d+")

    def __uidump(self):
        """
        获取当前Activity控件树并pull到本地目录
        """
        os.popen("adb shell uiautomator dump --compressed /data/local/tmp/uidump.xml")
        os.popen("adb pull /data/local/tmp/uidump.xml " + self.tempFile)

    def __element(self, attrib, name):
        """
        同属性单个元素，返回单个坐标元组
        """
        self.__uidump()
        tree = ET.ElementTree(file=self.tempFile + "\\uidump.xml")
        treeIter = tree.iter(tag="node")

        for elem in treeIter:
            if elem.attrib[attrib] == name:
                bounds = elem.attrib["bounds"]
                coord = self.pattern.findall(bounds)
                Xpoint = (int(coord[2]) - int(coord[0])) / 2.0 + int(coord[0])
                Ypoint = (int(coord[3]) - int(coord[1])) / 2.0 + int(coord[1])

                return Xpoint, Ypoint
				
	def __elements(self, attrib, name):
        """
        同属性多个元素，返回坐标元组列表
        """
        list = []
        self.__uidump()
        tree = ET.ElementTree(file=self.tempFile + "\\uidump.xml")
        treeIter = tree.iter(tag="node")
        for elem in treeIter:
            if elem.attrib[attrib] == name:
                bounds = elem.attrib["bounds"]
                coord = self.pattern.findall(bounds)
                Xpoint = (int(coord[2]) - int(coord[0])) / 2.0 + int(coord[0])
                Ypoint = (int(coord[3]) - int(coord[1])) / 2.0 + int(coord[1])
                list.append((Xpoint, Ypoint))
        return list

    def findElementByName(self, name):
        """
        通过元素名称定位
        usage: findElementByName(u"text")
        """
        return self.__element("text", name)


class AdbEvent(object):
    def __init__(self):
        os.popen("adb wait-for-device")

    def touch(self, dx, dy):
        """
        触摸事件
        usage: touch(500, 500)
        """
        os.popen("adb shell input tap " + str(dx) + " " + str(dy))

if __name__ == '__main__':
    test=FindElementsDemo()
    x,y=test.findElementByName('通讯录')

    ev=AdbEvent()
    ev.touch(x,y)
```

### 待补充

目前项目中只需要一个点击事件即可满足，所以未过多封装其它事件方法，欢迎大家补充