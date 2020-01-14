---
title: leetcode题解-1008-先序遍历构造二叉树
mathjax: true
date: 2019-12-16 10:54:11
categories:
tags:
- ×
---

## [题目](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal/)

#### 递归实现

利用二叉搜索树的前序遍历结果中的左子树序列中的值都小于根节点。

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
        if (l > r) return nullptr;
        TreeNode* root = new TreeNode(nums[l]);

        if (l == r) return root;
        int j = l + 1;
        
        while(nums[j] < nums[l])
        {
            ++j;
            if (j >= nums.size())   break;
        }

        root->left = helper(nums, l+1, j-1);
        root->right = helper(nums, j, r);

        return root;
    }
public:
    TreeNode* bstFromPreorder(vector<int>& preorder) 
    {
        return helper(preorder, 0, preorder.size() - 1);   
    }
};
```



##### 手动压栈实现

```C++
class Solution {
public:
   TreeNode* bstFromPreorder(vector<int>& preorder) {
       int n = preorder.size();
       if (!n)  return nullptr; 
       
       TreeNode* root = new TreeNode(preorder[0]);
       stack<TreeNode*> s;
       s.push(root);
       for (int i = 1; i < n; i++) 
       {
           TreeNode* node = s.top();
           TreeNode* child = new TreeNode(preorder[i]);
           while (!s.empty() && s.top()->val < child->val) 
           {
               node = s.top();
               s.pop();
           }
           if (node->val < child->val)     node->right = child;
           else                            node->left = child;
           s.push(child);
       }
       return root;
   }
};
```



## References

[NoNN way](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal/solution/4ms-c-ti-jie-jie-ti-si-lu-by-nonnus/)

https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal/solution/jian-kong-er-cha-shu-by-leetcode/

