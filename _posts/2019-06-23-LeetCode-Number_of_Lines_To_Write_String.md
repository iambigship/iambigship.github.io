---
layout: post
title:  "【LeetCode】806. 写字符串需要的行数(Number of Lines To Write String)的解题思路"
date:   2019-06-23 15:02:36
categories: LeetCode
---

题目如下：（[题目链接戳我](https://leetcode-cn.com/problems/number-of-lines-to-write-string/
)）

> 我们要把给定的字符串 S 从左到右写到每一行上，每一行的最大宽度为100个单位，如果我们在写某个字母的时候会使这行超过了100 个单位，那么我们应该把这个字母写到下一行。我们给定了一个数组 widths ，这个数组 widths[0] 代表 'a' 需要的单位， widths[1] 代表 'b' 需要的单位，...， widths[25] 代表 'z' 需要的单位。
>
> 现在回答两个问题：至少多少行能放下S，以及最后一行使用的宽度是多少个单位？将你的答案作为长度为2的整数列表返回。

> 示例 1:
> 输入: 
> widths = [10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10]
> S = "abcdefghijklmnopqrstuvwxyz"
> 输出: [3, 60]
> 解释: 
> 所有的字符拥有相同的占用单位10。所以书写所有的26个字母，
> 我们需要2个整行和占用60个单位的一行。

> 示例 2:
> 输入: 
> widths = [4,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10]
> S = "bbbcccdddaaa"
> 输出: [2, 4]
> 解释: 
> 除去字母'a'所有的字符都是相同的单位10，并且字符串 "bbbcccdddaa" 将会覆盖 9 * 10 + 2 * 4 = 98 个单位.
> 最后一个字母 'a' 将会被写到第二行，因为第一行只剩下2个单位了。
> 所以，这个答案是2行，第二行有4个单位宽度。

```java
class Solution {
    public int[] numberOfLines(int[] widths, String S) {
        
    }
}
```

# 以下是我的解题思路：

1. 将 `S` 转换为字符数组 `chars`；
2. 将字符数组`chars`强转为`int`数组`letters`；
3. 将`letters`中每个元素都减去`97`，这样就变成可以匹配数组下标的`int`数组了；
4. 循环`letters`，用`letters`中的元素作为下标，取出`widths`中的每个数字，并求它们的和`sum`，当`sum`大于`100`时，表明该换行了，将当前数字赋值给`sum`，并将行数`line`加`1`；
5. 最后将行数`line`和`sum`赋值给结果数组`result`，并返回；

代码如下：

```java
class Solution {
    public int[] numberOfLines(int[] widths, String S) {
        //将 S 转换为字符数组
        char[] chars = S.toCharArray();
        //将字符数组转换为 int 数组
        int[] letters = new int[chars.length];
        for (int i = 0; i < chars.length; i++) {
            //将 int 数组调整为与数组下标对应的 int 数组
            letters[i] = chars[i] - 97;
        }
        int sum = 0;
        int line = 1;
        for (int letter : letters) {
            //用 letters 数组元素作为下标，依次取出每个字母占用的单位数；
            //数量累加到一个和 sum 中
            sum += widths[letter];
            //当 sum 大于 100 时，sum 赋值为当前的单位数，并且行数加 1，继续下轮循环；
            if (sum > 100) {
                sum = widths[letter];
                line++;
            }
        }
        int[] result = new int[2];
        result[0] = line;
        result[1] = sum;
        return result;
    }
}
```

