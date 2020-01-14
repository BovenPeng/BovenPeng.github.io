---
title: leetcode题解-513-找树左下角的值
mathjax: true
date: 2019-12-19 22:11:14
categories:
tags:
- ×
---

## [题目](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

### My way

#### dfs递归实现

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
    int res = -1;
    int maxHeight = -1;
    void dfs(TreeNode* root, int h)
    {
        if (!root)  return;

        if (!root->left && !root->right)
        {
            if (h > maxHeight)
            {
                maxHeight = h;
                res = root->val;
            }
        }

        dfs(root->left, h + 1);
        dfs(root->right, h + 1);
    }
public:
    int findBottomLeftValue(TreeNode* root) 
    {
        dfs(root, 0);
        return res;
    }
};
```

运用2个辅助变量：

- `res`记录要返回的结点的`val`
- `maxHeight`记录要返回的结点的高度，用来确认是最深一个结点上的值