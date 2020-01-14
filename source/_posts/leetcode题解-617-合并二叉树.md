---
title: leetcode题解-617-合并二叉树
mathjax: true
date: 2019-12-12 22:31:06
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- ×
---

## [题目](https://leetcode-cn.com/problems/merge-two-binary-trees/)

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
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if (!t1)    return t2;
        if (!t2)    return t1;

        t1->val += t2->val;
        t1->left = mergeTrees(t1->left, t2->left);
        t1->right = mergeTrees(t1->right, t2->right);

        return t1;
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
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if (!t1)    return t2;
        if (!t2)    return t1;

        stack<pair<TreeNode*, TreeNode*>> s;
        s.push(make_pair(t1, t2));

        while (!s.empty())
        {
            auto root = s.top(); s.pop();

            if (!root.first|| !root.second)    continue;

            root.first->val += root.second->val;
            if (!root.first->left)
            {
                root.first->left = root.second->left;
            }
            else
            {
                s.push(make_pair(root.first->left, root.second->left));
            }

            if (!root.first->right)
            {
                root.first->right = root.second->right;
            }
            else
            {
                s.push(make_pair(root.first->right, root.second->right));
            }
        }

        return t1;
    }
};
```



## 总结

- 可以用`t1`, `t2`的根节点来代替为新的根节点，实现起来会很方便。
  
  - 当你遍历到一个结点，其中只有一棵树有这样的结点的时候，递归实现直接返回另外一棵树的该节点即可。
  
    

## Reference

https://leetcode-cn.com/problems/merge-two-binary-trees/solution/he-bing-er-cha-shu-by-leetcode/