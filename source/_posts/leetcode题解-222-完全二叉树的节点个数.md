---
title: leetcode题解-222-完全二叉树的节点个数
mathjax: true
date: 2020-01-02 10:56:24
categories:
tags:
---

## [题目](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

### [Wanglihao way](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

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
    public int countNodes(TreeNode root) {
        if(root == null){
           return 0;
        } 
        int left = countLevel(root.left);
        int right = countLevel(root.right);
        if(left == right){
            return countNodes(root.right) + (1<<left);
        }else{
            return countNodes(root.left) + (1<<right);
        }
    }
    private int countLevel(TreeNode root){
        int level = 0;
        while(root != null){
            level++;
            root = root.left;
        }
        return level;
    }
}
```

利用了完全二叉树定义：

- **它是一棵空树或者它的叶子节点只出在最后两层，若最后一层不满则叶子节点只在最左侧。**
  1. `left == right`. 说明左子树一定是满二叉树，因为节点已经填充到右子树了，左子树必然填满了。所以左子树的节点总数我们可以直接得到，是$2^{left }- 1$，加上当前这个root节点，则正好是$2^{left}$。再对右子树进行递归统计。
  2. `left != right`. 说明此时到了树的倒数第二层了，且倒数第二层已经满了，可以直接得到右子树的节点个数。同理，右子树节点+root节点，总数为$2^{right}$. 再对左子树进行递归查找。



## 总结

- 完全二叉树的定义引申出通过对左右孩子的判断，来得到对于节点个数的计算：

  - `left == right`. 

    说明左子树一定是满二叉树，因为节点已经填充到右子树了，左子树必然填满了。所以左子树的节点总数我们可以直接得到，是$2^{left }- 1$，加上当前这个root节点，则正好是$2^{left}$。再对右子树进行递归统计。

  - `left != right`.

    说明此时到了树的倒数第二层了，且倒数第二层已经满了，可以直接得到右子树的节点个数。同理，右子树节点+root节点，总数为$2^{right}$. 再对左子树进行递归查找。

  - **完全二叉树的高度**：

    `树的高度 = 树的最左节点的深度`

- 简单的位运算：

  - $2^{left} = (1  < <  left)$

## References

https://leetcode-cn.com/problems/count-complete-tree-nodes/