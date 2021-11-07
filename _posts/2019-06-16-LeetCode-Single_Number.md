---
layout: post
title:  "【LeetCode】136. 只出现一次的数字(single-number)的解题思路"
date:   2019-06-16 15:02:36
categories: LeetCode
---

题目如下：（[题目链接戳我](https://leetcode-cn.com/problems/single-number/
)）

> 给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

>说明：
>
>你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

>示例 1:
>
>输入: [2,2,1]
>输出: 1

>示例 2:
>
>输入: [4,1,2,1,2]
>输出: 4

以下是我的解题思路：

思路一：
把所有出现的数据全部都放到一个 set 集合中，放之前判断一下，如果 set 集合中有，就移除这个数据，因为出现两次的数据会经历放入集合和移动集合，所以最后 set 集合中只剩下一个数据，就是我们需要的数据；

思路二：
这个思路是在我看了官方解题思路后整理的，先遍历数组，把数组都放入 set 集合，只增加，不移除，并顺便得出数组中所有数值的和 sum1；然后遍布这个 set 集合，求出所有数值的和 sum2，再乘以 2， 这个时候，sum2 * 2 就比 sum1 多一个数，让它们俩相减，得到的差就是我们所要的数据；

代码如下：

```java
class Solution {
	//方法一
    public int singleNumber(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num: nums){
            if(set.contains(num)){
                set.remove(num);
            }else{
                set.add(num);
            }
        }
		Iterator<Integer> iterator = set.iterator();
            return iterator.next();
        }
        return -1;
    }
	//方法二
	public int singleNumber2(int[] nums) {
        Set<Integer> set = new HashSet<>();
        int sum1 = 0, sum2 = 0;
        for (int num: nums){
            set.add(num);
            sum1 += num;
        }
        for (int num : set) {
            sum2 += num;
        }
        return sum2 * 2 - sum1;
    }
}
```