---
title: leetcode题解-429-N叉树的层序遍历
date: 2019-12-06 21:15:23
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



## [题目](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

### My way

#### 递归实现BFS

```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {
private:
    void helper(vector<vector<int>>& vvi, int level, Node* root)
    {
        if (!root)  return ;

        // add new vector<int> for saving new level elements
        if (vvi.size() == level)
        {
            vvi.push_back(vector<int>());
        }

        vvi[level].push_back(root->val);
        for (auto it : root->children)
        {
            helper(vvi, level+1, it);
        }

    }
public:
    vector<vector<int>> levelOrder(Node* root) 
    {
        vector<vector<int>> vvi;
        helper(vvi, 0, root);

        return vvi;   
    }
};
```



#### 手动队列BFS

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        if (!root)  return vector<vector<int>>();

        vector<vector<int>> vvi;
        deque<Node*> q;
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
                
                for (auto& it : front->children)
                {
                    if (it) q.push_back(it);
                }
            }

            vvi.push_back(vi);
        }

        return vvi;
    }
};
```

