---
title: leetcode题解-938-二叉树搜索树的范围和
mathjax: true
date: 2019-12-12 21:12:20
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- ×
---

## [题目](https://leetcode-cn.com/problems/range-sum-of-bst/)

### My way

#### 递归解

要考虑3种情况：

1. L，R分别在root的左，右子树上, $val \in [L,\ R]$ 
2. L，R均在root的左子树上, $L< R <val$
3. L，R均在root的右子树上, $val > L > R$

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
    void helper(TreeNode* root, int L, int R)
    {
        if (!root)  return ;

        if (L <= root->val && R >= root->val)
        {
            sum += root->val;
        }
        else if (L < root->val)
        {
            helper(root->right, L, R);
        }
        else if (R > root->val)
        {
            helper(root->left, L, R);
        }
    }

public:
    int rangeSumBST(TreeNode* root, int L, int R) 
    {
        helper(root, L, R);

        return sum;
    }
};
```



#### 手动压栈

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
    int rangeSumBST(TreeNode* root, int L, int R) 
    {
        int sum = 0;
        stack<TreeNode*> s;
        s.push(root);
        
        while (!s.empty())
        {
            root = s.top(); s.pop();
            if (!root)	continue;
            if (L <= root->val && root->val <= R)	sum += root->val;
            if (L < root->val)	s.push(root->left);
            if (R > root->val)	s.push(root->right);
		}

        return sum;    
    }
};
```

