---
title: leetcode题解-109-将有序数组转换为二叉搜索树
mathjax: true
date: 2019-12-17 17:28:52
categories:
tags:
- ×
---

## [题目](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

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
    TreeNode* helper(const vector<int>& nums, int l, int r)
    {
        if (l > r)  return nullptr;

        int mid = (r - l)/2 + l; 
        auto root = new TreeNode(nums[mid]);
        root->left = helper(nums, l, mid-1);
        root->right = helper(nums, mid+1, r);
        
        return root;
    }

public:
    TreeNode* sortedArrayToBST(vector<int>& nums) 
    {

        return !nums.size()? nullptr: helper(nums, 0, nums.size()-1);
    }
};
```

