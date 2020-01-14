---
title: leetcode题解-110-平衡二叉树
date: 2019-12-05 19:22:17
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- DFS
- ×
---



## [题目](https://leetcode-cn.com/problems/balanced-binary-tree/)

```C+
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
    int helper(const TreeNode* root, int level, bool& res)
    {
        if (!root)  return level;

        int lH = helper(root->left, level + 1, res);
        if (!res)   return level;

        int rH = helper(root->right, level + 1, res);
        if (!res)   return level;

        if (abs(lH - rH) > 1)    res = false;

        return max(lH, rH);    
    }
public:
    bool isBalanced(TreeNode* root) 
    {
        bool res = true;

        helper(root, 1, res);
        return res;
    }
};
```

要保证返回当前结点的 **树高** 和 **是否平衡**。

