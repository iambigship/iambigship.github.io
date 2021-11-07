---
layout: post
title:  "【LeetCode】521. 最长特殊序列 Ⅰ（Longest Uncommon Subsequence I ）解题思路"
date:   2019-06-01 15:02:36
categories: LeetCode
---

题目如下：

> 给定两个字符串，你需要从这两个字符串中找出最长的特殊序列。最长特殊序列定义如下：该序列为某字符串独有的最长子序列（即不能是其他字符串的子序列）。
>
> 子序列可以通过删去字符串中的某些字符实现，但不能改变剩余字符的相对顺序。空序列为所有字符串的子序列，任何字符串为其自身的子序列。
>
> 输入为两个字符串，输出最长特殊序列的长度。如果不存在，则返回 -1。
>
> 示例 :
>
> 输入: "aba", "cdc"
> 输出: 3
> 解析: 最长特殊序列可为 "aba" (或 "cdc")



这道题乍看有点让人懵，看了几遍也没搞明白是什么意思，后来结合了别人的评论才发现这是一道送分题；

解题思路就是：

1. 如果两个字符串相同（equals），那么它们互相都有对方的每个字符序列，没有特殊的字符序列，这种情况返回 -1；
2. 如果两个字符串长度相等，但内容不相同，纵然它们中某一个或多个字符序列相同，但作为一个整体，这两个字符串是不等的，这种情况随便返回其中任意一个字符串的长度即可；
3. 如果两个字符串长度都不相等，那没得说，直接返回那个长的字符串的长度；

思路有了，代码就顺理成章了。

```java
class Solution {
    public int findLUSlength(String a, String b) {
        int aLen = a.length();
		int bLen = b.length();
		if(aLen != bLen){
			return Math.max(aLen, bLen);
		}else{
			if(a.equals(b)){
				return -1;
			}else{
				return aLen;
			}
		}
    }
}
```

执行结果：

> 执行用时 : 1 ms, 在Longest Uncommon Subsequence I 的Java提交中击败了76.94% 的用户
>
> 内存消耗 : 33.4 MB, 在Longest Uncommon Subsequence I 的Java提交中击败了93.53% 的用户