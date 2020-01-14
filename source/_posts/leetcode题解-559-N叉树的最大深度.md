---
title: leetcode题解-559-N叉树的最大深度
date: 2019-12-05 11:42:34
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- DFS
- ×
---



## [题目](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)

### My way

#### 递归

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
public:
    int maxDepth(Node* root) {
        if (!root)  return 0;
        if (root->children.empty())  return 1;

        vector<int> Height;
        for (auto it : root->children)
        {
            Height.push_back(maxDepth(it));
        }

        return *max_element(Height.begin(), Height.end()) + 1;
    }
};
```



#### 手动压栈DFS

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
public:
    int maxDepth(Node* root) {
        if (!root)      return 0;
        if (root->children.empty()) return 1;

        stack<pair<Node*, int>> stack;
        stack.push(make_pair(root, 1));

        int max_depth = -1;
        while(!stack.empty())
        {
            auto top = stack.top().first;
            int depth = stack.top().second;
            max_depth = max(max_depth, depth);
            stack.pop();

            for (auto it : top->children)
            {
                stack.push(make_pair(it, depth+1));
            }
        } 

        return max_depth;
    }
};
```



#### 层次遍历

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
public:
int maxDepth(Node* root) 
{
    if (!root) return 0;
    queue<Node*> queue;
    queue.push(root);
    int max_depth = 0;
    while (!queue.empty()) 
    {
        max_depth++;			
        for (int size = queue.size(); size; size--) 
        {
            Node* curr = queue.front(); 
            queue.pop();

            for (Node* it : curr->children)
            {
                queue.push(it);
            }
        }
    }
    return max_depth;
}
};
```

