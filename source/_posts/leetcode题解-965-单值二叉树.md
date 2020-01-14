---
title: leetcode题解-965-单值二叉树
mathjax: true
date: 2019-12-30 22:36:59
categories:
- 树的遍历
tags:
---

## [题目](https://leetcode-cn.com/problems/univalued-binary-tree/)

### My way

#### dfs遍历

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
    int val;
    bool isUT = true;

    void dfs(TreeNode* root)
    {
        if (!root || !isUT)  return;

        isUT = (val == root->val ? true : false);

        dfs(root->left);
        dfs(root->right);
    }

public:
    bool isUnivalTree(TreeNode* root) 
    {
        val = root->val;

        dfs(root);

        return isUT;    
    }
};
```

