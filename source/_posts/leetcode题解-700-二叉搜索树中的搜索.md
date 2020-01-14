---
title: leetcode题解-700-二叉搜索树中的搜索
mathjax: true
date: 2019-12-16 23:01:32
categories:
tags:
- 
---

## [题目](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

### My way

### 递归

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
public:
    TreeNode* searchBST(TreeNode* root, int val) 
    {
        while (root)
        {
            if (root->val == val)   return root;
            if (root->val > val)    root = root->left;
            else    root = root->right;
        }

        return root;
    }
};
```

很简单的题目，就不写非递归实现了