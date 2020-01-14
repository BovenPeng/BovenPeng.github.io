---
title: 'leetcode题解-107-二叉树的层次遍历Ⅱ '
date: 2019-12-05 20:44:57
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的遍历
- BFS
- ×
---



## [题目](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

### My way

#### 手动队列**BFS** + reverse结果

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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        if (!root)  return vector<vector<int>>();

        vector<vector<int>> vvi;
        deque<TreeNode*> q;
        q.push_back(root);

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

        return vector<vector<int>>(vvi.rbegin(), vvi.rend());
    }
};

```



#### 递归BFS + 逆序

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
    vector<vector<int>> levelOrderBottom(TreeNode* root) 
    {
        vector<vector<int>> vvi;
        helper(vvi, 0, root);

        return vector<vector<int>> (vvi.rbegin(), vvi.rend());   
    }
};

```

有没有办法遍历的时候直接逆序插入呢？？



## 总结

- 递归方式BFS实现层次遍历时，需要在`vector<vector<int>>`的对应层数上存储，所以在`helper`函数上，需要传参



## References

https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/er-cha-shu-ceng-ci-bian-li-c-by-huang-fu-mai-yan/