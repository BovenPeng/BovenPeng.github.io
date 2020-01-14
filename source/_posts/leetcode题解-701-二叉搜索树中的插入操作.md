---
title: leetcode题解-701-二叉搜索树中的插入操作
mathjax: true
date: 2019-12-16 23:01:54
categories:
tags:
- 
---

## [题目](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

### My way

#### 递归实现

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
    TreeNode* insertIntoBST(TreeNode* root, int val) 
    {
        auto node = root;
        TreeNode* tmp;
        while (node)
        {
            tmp = node;
            if (node->val == val)   return root;
            if (node->val > val)    node = node->left;
            else                    node = node->right;    
        }
        if (tmp->val > val)    tmp->left = new TreeNode(val);
        else                   tmp->right = new TreeNode(val);

        return root;
    }
};
```

很简单的题目，就不写非递归实现了。