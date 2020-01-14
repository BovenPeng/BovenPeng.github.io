---
title: leetcode题解-111-二叉树的最小深度
date: 2019-12-05 10:02:21
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



## [题目](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

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
public:
    int minDepth(TreeNode* root) {      
        if (!root)  return 0;
        int l_depth, r_depth;
        l_depth = minDepth(root->left);
        r_depth = minDepth(root->right);

        return !(root->left) || !(root->right) ? l_depth + r_depth + 1 : min(l_depth, r_depth) + 1;
    }
};
```



#### 手动压栈DFS

手动压栈，存储对应树结点和其高度，然后更新左右子树为空时的树高作为最小深度。

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
    int minDepth(TreeNode* root) {
        stack<pair<TreeNode*, int>> stack;

        if (!root)  return 0;

        stack.push(make_pair(root, 1));

        int min_depth = INT_MAX;
        while(!stack.empty())
        {
            auto top = stack.top();
            stack.pop();

            root = top.first;
            int depth = top.second;
            if (!(root->left) && !(root->right))
            {
                min_depth = min(min_depth, depth);
            }

            if (root->left)
            {
                stack.push(make_pair(root->left, depth + 1));
            }

            if (root->right)
            {
                stack.push(make_pair(root->right, depth + 1));
            }
        }

        return min_depth;
    }
};
```



#### 层次遍历

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
    int minDepth(TreeNode* root) {
        if (!root)  return 0;

        deque<TreeNode*> q;

        q.push_back(root);
        int depth = 1;
        while (!q.empty())
        {
            for (int i = 0; i < q.size(); ++i)
            {
                TreeNode* top = q.front();
                q.pop_front();

                if (!(top->left) && !(top->right))    return depth;
                if (top->left)   q.push_back(top->left);
                if (top->right)  q.push_back(top->right);
            }
            ++depth;
        }
    }
};
```



## References

https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/solution/li-jie-zhe-dao-ti-de-jie-shu-tiao-jian-by-user7208/





## 总结

- 被情况$[1, 2]$卡住了，因为计算的树深度是指root到leaf(左右孩子为空)的深度，此时应该是2，这个case一开始通过不了
- 左右子树有一个为空时，则返回左右子树的深度之和 + 1即可。
  - 即，省略了对于左，右子树两个子树的两次判断为一次。
  - 一种代码优化结构。