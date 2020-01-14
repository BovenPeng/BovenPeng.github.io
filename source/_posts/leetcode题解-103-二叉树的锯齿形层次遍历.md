---
title: leetcode题解-103-二叉树的锯齿形层次遍历
date: 2019-12-05 21:56:57
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的遍历
- BFS
- √
---

## [题目](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

### My way

#### 手动BFS + 奇逆序

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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if (!root)  return vector<vector<int>>();

        vector<vector<int>> vvi;
        deque<TreeNode*> q;
        q.push_back(root);

        // 可以加个depth变量，depth为偶数时，[size-1, 0]来存储变量
        while(!q.empty())
        {
            vector<int> vi;

            int size = q.size();
            for (int i = 0; i < size; ++i)
            {
                auto front = q.front();
                q.pop_front();

                vi.push_back(front->val);
                if (front->left)   q.push_back(front->left);
                if (front->right)  q.push_back(front->right);
            }

            vvi.push_back(vi);
        }

        for (size_t i = 1; i < vvi.size(); ++i, ++i)
        {
            vvi[i] = vector<int>(vvi[i].rbegin(), vvi[i].rend());
        }

        return vvi;        
    }
};
```



加个depth变量，depth为偶数时，[size-1, 0]来存储变量 OR 插入后逆序，因为`vector::insert`代价是$O(N)$，所以一直从头部差，时间cost过大，或者 用`deque`，然后用`deque`中元素赋值给`vector`

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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if (!root)  return vector<vector<int>>();

        vector<vector<int>> vvi;
        deque<TreeNode*> q;
        q.push_back(root);

        // 可以加个depth变量，depth为偶数时，[size-1, 0]来存储变量
        int depth = 0;
        while(!q.empty())
        {
            vector<int> vi;
            ++depth;

            int size = q.size();
            
            for (int i = 0; i < size; ++i)
            {
                auto front = q.front();
                q.pop_front();

                vi.push_back(front->val);

                if (front->left)   q.push_back(front->left);
                if (front->right)  q.push_back(front->right);
            }

            if (!depth&(depth-1))   vi = vector<int>(vi.rbegin(), vi.rend());   

            vvi.push_back(vi);
        }

        for (size_t i = 1; i < vvi.size(); ++i, ++i)
        {
            vvi[i] = vector<int>(vvi[i].rbegin(), vvi[i].rend());
        }

        return vvi;        
    }
};
```





#### 递归 + 奇逆序

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
    void helper(vector<vector<int>>& vvi, int level, TreeNode* root)
    {
        if (!root)  return ;

        // add new vector<int> for saving new level elements
        if (vvi.size() == level)
        {
            vvi.push_back(vector<int>());
        }

        vvi[level].push_back(root->val);
        helper(vvi, level+1, root->left);
        helper(vvi, level+1, root->right);
    }
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) 
    {
        vector<vector<int>> vvi;
        helper(vvi, 0, root);

        for (size_t i = 1; i < vvi.size(); ++i, ++i)
        {
            vvi[i] = vector<int>(vvi[i].rbegin(), vvi[i].rend());
        }

        return vvi;   
    }
};
```



