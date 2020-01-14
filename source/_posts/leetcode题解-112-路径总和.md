---
title: leetcode题解-112-路径总和
mathjax: true
date: 2019-12-25 23:19:16
categories:
tags:
---

## [题目](https://leetcode-cn.com/problems/path-sum/)

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
private:
    void dfs(TreeNode* root, int& pathSum, int sum, bool& res)
    {
        if (!root || res)  return ;

        pathSum += root->val;

        // 到达叶子结点时
        if (!root->left && !root->right && pathSum == sum) res = true;

        dfs(root->left, pathSum, sum, res);
        dfs(root->right, pathSum, sum, res);

        pathSum -= root->val;
    }

public:
    bool hasPathSum(TreeNode* root, int sum) 
    {
        int pathSum = 0;
        bool res = false;

        dfs(root, pathSum, sum, res);

        return res;    
    }
};
```

