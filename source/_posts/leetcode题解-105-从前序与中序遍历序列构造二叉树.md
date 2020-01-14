---
title: leetcode题解-105-从前序与中序遍历序列构造二叉树
mathjax: true
date: 2019-12-10 20:13:54
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的遍历
- 栈
- ×
---



## [题目](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

### [当当喵 way](https://leetcode-cn.com/problems/sum-of-left-leaves/solution/chao-ji-rong-yi-li-jie-qi-shi-he-qiu-quan-lu-jing-/)

#### dfs遍历 + isLeft来标注当前节点状态即可

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
    int sum = 0;
    void getSum(TreeNode* root, bool isLeft)
    {
        if (!root)  return;

        if (isLeft && !root->left && !root->right)
        {
            sum += root->val;
        }
        else
        {
            getSum(root->left, true);
            getSum(root->right, false);
        }
    }
public:
    int sumOfLeftLeaves(TreeNode* root) 
    {
        getSum(root, false);

        return sum;    
    }
};
```



## References

https://leetcode-cn.com/problems/sum-of-left-leaves/solution/chao-ji-rong-yi-li-jie-qi-shi-he-qiu-quan-lu-jing-/