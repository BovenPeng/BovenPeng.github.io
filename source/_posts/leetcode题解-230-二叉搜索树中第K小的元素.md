---
title: leetcode题解-230-二叉搜索树中第K小的元素
mathjax: true
date: 2019-12-18 15:26:38
categories:
tags:
- ×
---

## [题目](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

### My way

#### 递归中序遍历，输出对应元素

```C++
/**
 * Definition for a binary tree node.
 * strzuct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
private:
    vector<int> vi;
    void dfs(TreeNode* root)
    {
        if (!root)  return ;

        dfs(root->left);
        vi.push_back(root->val);
        dfs(root->right);
    }
public:
    int kthSmallest(TreeNode* root, int k) {
        dfs(root);

        return vi[k-1];
    }
};
```

因为二叉搜索树的中序遍历结果是由小到大的，所以输出第`k-1`个元素即可



### [大力王way](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/solution/c-zhong-xu-bian-li-ti-jie-by-da-li-wang/)

#### 递归，控制中序遍历到第`k-1`个元素

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
    void dfs(TreeNode* root,int& res, bool& find, int& i, int k) {
        if (root == NULL || find) return;
        dfs(root->left, res, find, i, k);
        if (++i == k) {
            res = root->val;
            find = true;
            return;
        }
        dfs(root->right, res, find, i, k);
    }
    int kthSmallest(TreeNode* root, int k) {
        bool find = false;
        int res = -1;
        int i = 0;
        dfs(root, res, find, i, k);
        return res;
    }
};
```



## References

https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/solution/c-zhong-xu-bian-li-ti-jie-by-da-li-wang/