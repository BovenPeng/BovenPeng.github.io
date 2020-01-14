---
title: leetcode题解-226-翻转二叉树
mathjax: true
date: 2019-12-16 15:45:37
categories:
tags:
- ×
---

## [题目](https://leetcode-cn.com/problems/invert-binary-tree/)

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
    TreeNode* invertTree(TreeNode* root) 
    {
        if (!root)  return nullptr;

        TreeNode* tmp = invertTree(root->right);
        root->right = invertTree(root->left);
        root->left = tmp;

        return root;        
    }
};
```



#### 手动队列，层次遍历

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
    TreeNode* invertTree(TreeNode* root) 
    {
        queue<TreeNode*> q;
        q.push(root);

        while(!q.empty())
        {
            auto front = q.front(); q.pop();
            if (!front) continue;

            auto tmp = front->right;
            front->right = front->left;
            front->left = tmp;

            q.push(front->left);
            q.push(front->right);
        }    

        return root;
    }
};
```

