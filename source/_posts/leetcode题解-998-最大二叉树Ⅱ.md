---
title: leetcode题解-998-最大二叉树Ⅱ
mathjax: true
date: 2019-12-11 23:06:07
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的构造
- ×
---



## [题目](https://leetcode-cn.com/problems/maximum-binary-tree-ii/)

没能理解题意。。。

我感觉是给了一个已经构造好的最大二叉树，然后新插入一个值为`val`的结点进来。

[这个解答](https://leetcode-cn.com/problems/maximum-binary-tree-ii/solution/go-0ms-wu-di-gui-by-yuhhen/)告诉了这个题是要做啥。然后发现是对比三幅例子的树图可以得到插入`val`结点进来的要求。

分成如下2种情况：

1. 大于`root`，则把 `root`作为`val节点`的左子树；
2. 小于`root`，则把 `val节点`作为`root`的右子树；

### [secretname way](https://leetcode-cn.com/problems/maximum-binary-tree-ii/solution/c-di-gui-jian-ji-dai-ma-by-secretname/)

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
    TreeNode* insertIntoMaxTree(TreeNode* root, int val)
    {
        if (!root || root->val < val)
        {
            TreeNode* node = new TreeNode(val);
            node->left = root;
            return node;
        }

        root->right = insertIntoMaxTree(root->right, val);
        return root;
    }
};
```



## References

https://leetcode-cn.com/problems/maximum-binary-tree-ii/solution/go-0ms-wu-di-gui-by-yuhhen/

https://leetcode-cn.com/problems/maximum-binary-tree-ii/solution/c-di-gui-jian-ji-dai-ma-by-secretname/