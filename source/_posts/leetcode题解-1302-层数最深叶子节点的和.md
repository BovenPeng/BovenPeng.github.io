---
title: leetcode题解-1302-层数最深叶子节点的和
mathjax: true
date: 2019-12-30 15:21:01
categories:
tags:
---

## [题目](https://leetcode-cn.com/problems/deepest-leaves-sum/)

### My way

#### dfs带层数状态遍历

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
    int sum = 0;
    int maxLevel = -1;
    void dfs(TreeNode* root, int level)
    {
        if (!root)  return;

        if (maxLevel < level)
        {
            maxLevel = level;
            sum = root->val;
        }
        else if (maxLevel == level)
        {
            sum += root->val;
        }

        dfs(root->left, level+1);
        dfs(root->right, level+1);
    }

public:
    int deepestLeavesSum(TreeNode* root) 
    {
        dfs(root, 0);

        return sum;    
    }
};
```

`sum`, `maxLevel`也可以改成`dfs`函数的引用参数。