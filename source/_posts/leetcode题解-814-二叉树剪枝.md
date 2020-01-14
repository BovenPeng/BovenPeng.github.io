---
title: leetcode题解-814-二叉树剪枝
mathjax: true
date: 2019-12-16 22:18:06
categories:
tags:
- ×
---

## [题目](https://leetcode-cn.com/problems/binary-tree-pruning/)

### 官方way

#### 递归

##### java实现

```java
class Solution {
    public TreeNode pruneTree(TreeNode root) {
        return containsOne(root) ? root : null;
    }

    public boolean containsOne(TreeNode node) {
        if (node == null) return false;
        boolean a1 = containsOne(node.left);
        boolean a2 = containsOne(node.right);
        if (!a1) node.left = null;
        if (!a2) node.right = null;
        return node.val == 1 || a1 || a2;
    }
}
```



##### C++实现

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
    bool helper(TreeNode* root)
    {
        if (!root)  return false;

        bool lDel = helper(root->left);
        bool lRig = helper(root->right);

        if (!lDel)  root->left = nullptr;
        if (!lRig)  root->right = nullptr;

        return root->val == 1 | lDel | lRig;
    }

public:
    TreeNode* pruneTree(TreeNode* root) 
    {
        return helper(root) ? root : nullptr;
    }
};
```



## 总结

- 自己一开始的递归思路不合理。
- 递归终止条件： 到达`nullptr`处，`return false`
- 递归状态：
  1. `当前结点值 == 1`
  2. `左子树 == 全为1`
  3. `右子树 == 全为1`

## Reference

https://leetcode-cn.com/problems/binary-tree-pruning/solution/er-cha-shu-jian-zhi-by-leetcode/