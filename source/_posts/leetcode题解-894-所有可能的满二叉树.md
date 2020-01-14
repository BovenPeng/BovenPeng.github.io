---
title: leetcode题解-894-所有可能的满二叉树
mathjax: true
date: 2019-12-16 10:53:25
categories:
tags:
- ×
---

## [题目](https://leetcode-cn.com/problems/all-possible-full-binary-trees/)

### My way

#### 记忆化搜索

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
// 使用记忆化搜索，避免每次重新计算测试用例
class Solution {
private:
unordered_map<int, vector<TreeNode*>> memo;

public:
    vector<TreeNode*> allPossibleFBT(int N) 
    {
        if (N % 2 == 0) return vector<TreeNode*>();

        //if (memo.count(N))    return memo[N];
        auto it = memo.find(N);
        if (it != memo.end())   return it->second;

        vector<TreeNode*> vt;

        if (N == 1)
        {
            vt.push_back(new TreeNode(0));
        } 
        else if (N % 2 == 1)
        {
            for (int x = 0; x < N; ++x)
            {
                int y = N - 1 - x;
                for (auto left: allPossibleFBT(x))
                {
                    for (auto right: allPossibleFBT(y))
                    {
                        TreeNode* bns = new TreeNode(0);
                        bns->left = left;
                        bns->right = right;
                        vt.push_back(bns);
                    }
                }
            }
        }
        memo.insert(make_pair(N, vt));
    
        return memo[N];
    }
};
```





