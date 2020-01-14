---
title: leetcode题解-1305-两棵二叉搜索树中的所有元素
mathjax: true
date: 2019-12-30 15:34:11
categories:

tags:
---

## [题目](https://leetcode-cn.com/problems/all-elements-in-two-binary-search-trees/)

### My way

#### dfs中序遍历 + 归并排序

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
    vector<int> v1, v2;
    void dfs(TreeNode* root, vector<int>& v)
    {
        if (!root)  return;

        dfs(root->left, v);
        v.push_back(root->val);
        dfs(root->right, v);
    }

public:
    vector<int> getAllElements(TreeNode* root1, TreeNode* root2) 
    {
        dfs(root1, v1);
        dfs(root2, v2);     

        vector<int> res;

        size_t i = 0, j = 0;
        while (i < v1.size() && j < v2.size())
        {
            if (v1[i] <= v2[j])
            {
                res.push_back(v1[i++]);  
            }
            else
            {                 
                res.push_back(v2[j++]); 
            }
        }

        while (i < v1.size())
        {
            res.push_back(v1[i++]);
        }
        while (j < v2.size())
        {
            res.push_back(v2[j++]);
        }

        return res;
    }
};
```

