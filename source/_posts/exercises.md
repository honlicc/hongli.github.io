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
```sh
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
-----------------------------------------------------------------------------------------------------------------------------------------
思路:

二进制均为1或0组成，只需统计处1出现的次数便可得到结果

代码实现:
def count_bits(n:int)->int:
	return bin(n).count("1")
```
---

### 2.求最长子串的长度。
```sh
描述：给定一个字符串 s ，请找出其中不含有重复字符的最长子串的长度。

示例 1:
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。


示例 2:
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。


示例 3:
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。


伪代码:

class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
		...你的代码...
	
-----------------------------------------------------------------------------------------------------------------------------------------
思路1: 创建两个空字符串a,b="",for 循环目标字符串s获取key字符串i,判断i是否存在于a中，不存在则a+=i;存在时则获取 i在a中的索引并+1进行切片，再对
切片后的字符串a+=i操作；并通过if判断a和b字符串的长度，若len(a)>len(b)，则 b=a,最后返回 len(b)

代码实现：
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s:
            return 0

        a=""
        b=""

        for i in s:
            if i not in a:
                a+=i
            else:
                a=a[a.index(i)+1:]
                a+=i

            if len(a)>len(b):
                b=a

        return len(b)
```

---
### 3.两数之和。
```sh
描述：给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出和为目标值target的那两个整数，并返回它们的数组下标。

示例 1:
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

示例 2:
输入：nums = [3,2,4], target = 6
输出：[1,2]

示例 3:
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

伪代码:
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
		...你的代码...

-----------------------------------------------------------------------------------------------------------------------------------------
思路1:双循环遍历，返回两数相加==target 两数索引

实现：
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        sums=[]
        for i in range(len(nums)):
            for j in range(i+1,len(nums)):
                if nums[i] + nums[j] ==target:
                    sums.append(i)
                    sums.append(j)

        return sums
-----------------------------------------------------------------------------------------------------------------------------------------	
思路2：单循环遍历，判断 target-nums[i]获取到的值是否存在，不存在则以字典形式存储值和索引 d[nums[i]]=i

实现：
class Solution:
    
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d={}
        for i in range(len(nums)):
            if (target-nums[i]) in d:
                return [d.get(target-nums[i]),i]
            d[nums[i]]=i
			
执行结果：通过

执行用时：40 ms, 在所有 Python3 提交中击败了68.33%的用户
内存消耗：16.1 MB, 在所有 Python3 提交中击败了16.17%的用户
通过测试用例：57 / 57

```






























