---
title: leetcode题解-144-二叉树的前序遍历
date: 2019-12-08 20:48:45
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的遍历
- √
---

## [题目](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

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
    vector<int> vi;
    void helper(TreeNode* root)
    {
        if (!root)  return ;
        vi.push_back(root->val);
        helper(root->left); 
        helper(root->right);
    }
public:
    vector<int> preorderTraversal(TreeNode* root) {
        helper(root);

        return vi;
    }
};
```



#### 手动压栈实现

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
    vector<int> vi;

public:
    vector<int> preorderTraversal(TreeNode* root) 
    {
        if (root) 
        {
            stack<TreeNode*> s;
            s.push(root);

            while (!s.empty())
            {
                auto top = s.top(); s.pop();
                vi.push_back(top->val);

                if (top->right)    s.push(top->right);
                if (top->left)    s.push(top->left); 
            }
        }   

        return vi;
    }
};
```





### 总结

- 因为前序遍历是 ”**根左右**“，所以手动入栈的时候是 **先右孩子，再左孩子**