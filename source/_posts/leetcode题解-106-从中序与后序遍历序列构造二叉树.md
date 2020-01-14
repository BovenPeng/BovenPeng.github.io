---
title: leetcode题解-106-从中序与后序遍历序列构造二叉树
mathjax: true
date: 2019-12-10 20:17:02
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

## [题目](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

### [hareyukai way](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/solution/-cong-zhong-xu-yu-hou-xu-bian-li-xu-lie-gou-zao-er/)

#### 递归实现 + 参数传迭代器

```C++
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {        
        return buildTree(rbegin(inorder), rend(inorder),
                         rbegin(postorder), rend(postorder));
    }
    
    template<typename RandomIt>
    TreeNode* buildTree(RandomIt in_rfirst, RandomIt in_rlast,
                        RandomIt post_rfirst, RandomIt post_rlast) {
        if (in_rfirst == in_rlast) return nullptr;
        if (post_rfirst == post_rlast) return nullptr;
        
        auto root = new TreeNode(*post_rfirst);
        
        auto inRootRPos = find(in_rfirst, in_rlast, *post_rfirst);
        auto RightSize = distance(in_rfirst, inRootRPos);
        
        root->right = buildTree(in_rfirst,
                                next(in_rfirst, RightSize),
                                next(post_rfirst),
                                next(post_rfirst, RightSize + 1));
        root->left = buildTree(next(inRootRPos),
                               in_rlast,
                               next(post_rfirst, RightSize + 1),
                               post_rlast);
        return root;
    }
};
```



简单修改了一下

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
using It = vector<int>::reverse_iterator;

class Solution {
private:
    TreeNode* buildTree(It inFir, It inLast, It postFir, It postLast)
    {
        if (inFir == inLast || postFir == postLast) return nullptr;

        auto root = new TreeNode(*postFir);

        auto inRootPos = find(inFir, inLast, *postFir);
        auto RightSize = distance(inFir, inRootPos);

        root->right = buildTree(inFir, next(inFir, RightSize), next(postFir), next(postFir, RightSize + 1));
        root->left = buildTree(next(inRootPos), inLast, next(postFir, RightSize + 1), postLast);

        return root;
    }
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) 
    {
        return buildTree(inorder.rbegin(), inorder.rend(), postorder.rbegin(), postorder.rend());
    }
};
```

