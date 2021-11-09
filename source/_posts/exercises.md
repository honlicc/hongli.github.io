---
title: 每日一题
date: 2021-11-09 21:56:17
categories: 
- python
tags:
- 每日一题
---
---

### 1. 比特求和：
```
描述： 给定一个正整数 n,请编写一个函数，计算它的二进制形式的所有数字的和

示例：
输入：1234
输出：5  # 1234--转二进制--> 10011010010，5个1 和为 5

def count_bits(n:int)->int:

	...你的代码...
	
assert count_bits(4)==1
assert count_bits(9)==2
assert count_bits(10)==2
assert count_bits(1234)==5
---------------------------------------------------------------------------------
思路:

二进制均为1或0组成，只需统计处1出现的次数便可得到结果

def count_bits(n:int)->int:
	return bin(n).count("1")
```

