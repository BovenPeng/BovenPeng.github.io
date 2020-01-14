---
title: leetcode题解-113-路径总和Ⅱ
mathjax: true
date: 2019-12-19 22:21:49
categories:
tags:
---



## [题目](https://leetcode-cn.com/problems/path-sum-ii/)

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
    vector<vector<int>> res;

    void dfs(TreeNode* root, int& pathSum, int sum, vector<int>& path)
    {
        if (!root)  return;

        pathSum += root->val;
        path.push_back(root->val);

        if (!root->left && !root->right && pathSum==sum)
        {
            res.push_back(path);
        }

        dfs(root->left, pathSum, sum, path);
        dfs(root->right, pathSum, sum, path);

        pathSum -= root->val;
        path.pop_back();
    }

public:
    vector<vector<int>> pathSum(TreeNode* root, int sum) 
    {
        int pathSum = 0;
        vector<int> path;
        dfs(root, pathSum, sum, path);

        return res;    
    }
};
```

- 简单递归即可实现
- 使用`pathSum`变量来记录当前`root`结点上的路径和

