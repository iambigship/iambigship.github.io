---
layout: post
title:  "【LeetCode】118. 杨辉三角（Pascal's Triangle）解题思路"
date:   2019-05-25 15:02:36
categories: LeetCode
---

题目如下（[题目链接戳我](https://leetcode-cn.com/problems/pascals-triangle/)）：

```
给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。
备注：在杨辉三角中，每个数是它左上方和右上方的数的和。

示例：
输入: 5
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

也给出了杨辉三角的示例图：

![杨辉三角](http://upload-images.jianshu.io/upload_images/782269-abadc6d6ed0609f5.gif?imageMogr2/auto-orient/strip)

以下是我的解题思路：

我首先整理了前 5 行杨辉三角的数据；

```
    1
   1 1
  1 2 1
 1 3 3 1
1 4 6 4 1
```

根据题目的返回值要求（ List < List < Integer >> ）（我在中括号和字母之间故意加了空格），我把每一行看作一个集合，每个元素对应一个下标索引，我把索引也整理了一下；

```
    0
   0 1
  0 1 2
 0 1 2 3
0 1 2 3 4
```

然后得出了以下几条规律和方法：

1. 第一个元素为1（这个其实不算规律，就是事实，只是为了一会写代码时方便写限制）
2. 每一行数据集合 List< Integer > 的第一个元素为 1（也是事实）；
3. 增加一个集合，用于记录上一行的数值；
4. 每个元素等于上一个集合同索引的数值和前一个索引的数值的和（这也是个事实，不过要加个限制，见下条）；
5. 每行集合中最后一个元素（索引为index）只等于上一行集合中索引为 index-1 的值；

思路整理完了，剩下写代码就很简单了。

```java
public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> list = new ArrayList<>();
        List<Integer> item;//当前行的集合
        List<Integer> last = null; //上一行的集合
        int index = 1; //行号，每行元素个数和行号一致
        while (index <= numRows) {
            item = new ArrayList<>();
            //循环index次，向item中添加元素
            a:
            for (int i = 0; i < index; i++) {
                if(last == null){
                    item.add(1);
                    break a;
                }else if(i == 0){
                    item.add(1);
                }else if(i == index - 1){
                    item.add(last.get(i - 1));
                }else{
                    item.add(last.get(i - 1) + last.get(i));
                }
            }
            last = item;
            list.add(item);
            index++;
        }
        return list;
    }
```

提交结果：

```
执行用时 : 1 ms, 在Pascal's Triangle的Java提交中击败了97.86% 的用户
内存消耗 : 33.6 MB, 在Pascal's Triangle的Java提交中击败了39.51% 的用户
```
