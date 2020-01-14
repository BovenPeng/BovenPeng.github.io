---
title: leetcode题解-671-二叉树中第二小的节点
mathjax: true
date: 2019-12-18 17:41:11
categories:
tags: 
- ×
---

## [题目](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/)

### My way

#### dfs递归+3个辅助变量

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
private:
    int fir = INT_MAX;
    int sec = INT_MAX;
    int secIsChange = false;

    void dfs(TreeNode* root)
    {
        if (!root)  return;
        if ((root->val > fir && root->val < sec) || root->val == INT_MAX)  { sec = root->val;  secIsChange = true;}
        else if (root->val < fir)                                          { fir = root->val;  }
        dfs(root->left);
        dfs(root->right);
    }
public:
    int findSecondMinimumValue(TreeNode* root) 
    {
        dfs(root);

        return secIsChange?sec:-1;
    }
};
```

我这里没有利用到题目给的

- 每个节点仅有`0`个或`2`个节点数；
- 如果一个节点有`2`个节点数，那么这个节点的值不大于它子节点的值。



#### [leetcode way](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/solution/ji-bai-liao-100de-javayong-hu-by-reedfan/)

```C++
public int findSecondMinimumValue(TreeNode root) {
        if (root == null || (root.left == null && root.right == null)) {
            return -1;
        }

        int left = root.left.val;
        int right = root.right.val;

        if (left == root.val) {
            left = findSecondMinimumValue(root.left);
        }

        if (right == root.val) {
            right = findSecondMinimumValue(root.right);
        }

        if (left != -1 && right != -1) {
            return Math.min(left, right);
        }
        if (left != -1) {
            return left;
        }
        return right;

    }
```



## Reference

https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/solution/ji-bai-liao-100de-javayong-hu-by-reedfan/

