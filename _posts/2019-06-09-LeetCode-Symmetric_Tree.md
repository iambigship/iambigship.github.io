---
layout: post
title:  "【LeetCode】101. 对称二叉树（Symmetric Tree）解题思路"
date:   2019-06-09 15:02:36
categories: LeetCode
---

题目如下：（[题目链接戳我](<https://leetcode-cn.com/problems/symmetric-tree/>)）

![对称二叉树](https://upload-images.jianshu.io/upload_images/782269-fbb04505367969a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以下是我的解题思路：
我接触过的二叉树的题目，大多都可以用递归方式来解题，所以只需要找到规律，然后方法内部再调用一次自己就可以。

我们就拿这个模型来分解：

![](https://upload-images.jianshu.io/upload_images/782269-dc0f37c5c1a7aa23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 左边的2和右边的2比较
> 左边的3和右边的3比较
> 左边的4和右边的4比较

抽象出来就是：
> 1 的左子(2) **VS** 1 的右子(2)；
> 1 的左子(2)的左子(3) **VS** 1 的右子(2)的右子(3)
> 1 的左子(2)的右子(4) **VS** 1 的右子(2)的左子(4)；

再加一层呢。

![](https://upload-images.jianshu.io/upload_images/782269-4a2e5f5798de1599.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

继续抽象，这里容易搞晕，要再之前的基础上递归，而不能偏到树的某一侧了，比如下边这样就走错了：
![](https://upload-images.jianshu.io/upload_images/782269-7f28762e36b33127.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

刚才我们走到了：
> 1 的左子(2)的左子(3) **VS** 1 的右子(2)的右子(3)
> 1 的左子(2)的右子(4) **VS** 1 的右子(2)的左子(4)；
 
继续前进，就是：

> 1 的左子(2)的左子(3)的左子(5) **VS** 1 的右子(2)的右子(3)的左子(5)
> 1 的左子(2)的左子(3)的右子(6) **VS** 1 的右子(2)的右子(3)的左子(6)
> 1 的左子(2)的右子(4)的左子(7) **VS** 1 的右子(2)的左子(4)的右子(7)
> 1 的左子(2)的右子(4)的右子(8) **VS** 1 的右子(2)的左子(4)的左子(8)

不能再向下吧，有点晕了，还是上代码吧。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return isSame(root, root);
		//下边的写法，没有上边巧妙；
//		if(root == null){
//			return true;
//		}else{
//			return isSame(root.left, root.right);
//		}
	}
	
	public boolean isSame(TreeNode node1, TreeNode node2){
		if(node1 == null && node2 == null){
			return true;
		}else if(node1 == null || node2 == null){
			return false;
		}else{
			return node1.val == node2.val && isSame(node1.left, node2.right) && isSame(node1.right, node2.left);
		}
    }
}
```

代码中的之前抽象的基础上又增加了一些特殊情况的处理。

