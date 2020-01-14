---
title: leetcode题解-897-递增顺序查找树
mathjax: true
date: 2020-01-02 14:53:59
categories:
tags:
---

## [题目](https://leetcode-cn.com/problems/increasing-order-search-tree/)

### My way

#### 中序遍历用vector存储结果 + 按vector中结果依次构建树

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
    void inOrder(TreeNode* root, vector<int>& vi)
    {
        if (!root)  return ;

        inOrder(root->left, vi);
        vi.push_back(root->val);
        inOrder(root->right, vi);
    }
public:
    TreeNode* increasingBST(TreeNode* root) 
    {
        vector<int> vi;
        inOrder(root, vi);

        TreeNode* res = new TreeNode(0);
        TreeNode* cur = res;

        for (auto& it: vi)
        {
            cur->right = new TreeNode(it);
            cur = cur->right;
        }

        return res->right;
    }
};
```

####  

#### 就地拼接 + 不使用额外空间复杂度

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
    TreeNode* cur;
    void inOrder(TreeNode* node)
    {
        if (!node)  return;

        inOrder(node->left);
        
        node->left = nullptr;
        cur->right = node;
        cur = node;

        inOrder(node->right);
    }

public:
    TreeNode* increasingBST(TreeNode* root) 
    {
        TreeNode* res = new TreeNode(0);
        cur = res;
        inOrder(root);

        return res->right;
    }
};
```



## Reference

https://leetcode-cn.com/problems/increasing-order-search-tree/solution/di-zeng-shun-xu-cha-zhao-shu-by-leetcode/