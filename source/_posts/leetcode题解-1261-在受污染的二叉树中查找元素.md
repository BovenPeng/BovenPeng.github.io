---
title: leetcode题解-1261-在受污染的二叉树中查找元素
mathjax: true
date: 2019-12-18 15:07:58
categories:
tags:
- ×
---



## [题目](https://leetcode-cn.com/problems/find-elements-in-a-contaminated-binary-tree/)

### My way

#### dfs遍历树+哈希表

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
class FindElements {
private:
    set<int> s;
    void dfs(TreeNode* root, int val)
    {
        if (!root)  return;
        root->val = val;
        s.insert(val);

        if (root->left)     dfs(root->left, val*2+1);
        if (root->right)    dfs(root->right, val*2+2);
    }

public:
    FindElements(TreeNode* root) {
        dfs(root, 0);
    }
    
    bool find(int target) {
        if (s.count(target))    return true;
        else    return false;
    }
};

/**
 * Your FindElements object will be instantiated and called as such:
 * FindElements* obj = new FindElements(root);
 * bool param_1 = obj->find(target);
 */
```

