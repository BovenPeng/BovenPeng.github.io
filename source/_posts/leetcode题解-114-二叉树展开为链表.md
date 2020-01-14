---
title: leetcode题解-114-二叉树展开为链表
mathjax: true
date: 2019-12-30 21:37:22
categories:
- 后序遍历
tags:
---

## [题目](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

### [windliang way](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by--26/)

#### 对左右子树进行拼接的过程

```java
    1
   / \
  2   5
 / \   \
3   4   6

//将 1 的左子树插入到右子树的地方
    1
     \
      2         5
     / \         \
    3   4         6        
//将原来的右子树接到左子树的最右边节点
    1
     \
      2          
     / \          
    3   4  
         \
          5
           \
            6
            
 //将 2 的左子树插入到右子树的地方
    1
     \
      2          
       \          
        3       4  
                 \
                  5
                   \
                    6   
        
 //将原来的右子树接到左子树的最右边节点
    1
     \
      2          
       \          
        3      
         \
          4  
           \
            5
             \
              6         
  
  ......
```



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
    void flatten(TreeNode* root) 
    {
        while (root)
        {
            // root->left is nullptr
            if (!root->left)
            {
                root = root->right;
            }
            else
            {
                // find the righest node of leftSubTree
                TreeNode* pre = root->left;
                while (pre->right)
                {
                    pre = pre->right;
                }
                
                // make 'pre' node 'right' pointed to the origin rightSubTree
                pre->right = root->right;

                // make leftSubTree insert to rightSubTree
                root->right = root->left;
                root->left = nullptr;

                // consider next right node
                root = root->right;
            }
        }
    }
};
```



#### 后序遍历

二叉树展开成为链表的结果就是 **先序遍历** 的结果。

但是按 **先序遍历** 结果来处理的话，更新当前结点的右指针的时候，当前结点的右指针就没有保存下来了。

```java
    1
   / \
  2   5
 / \   \
3   4   6
   
// preOrder 
1 -> 2 -> 3 -> 4 -> 5 -> 6
```

所以思考 **反过来，通过后序遍历** 来处理。

```java
// postOrder 
6 <- 5 <- 4 <- 3 <- 2 <- 1
```

即，每遍历一个节点，考虑将当前节点的 **右指针** 指向上一个节点。

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
    TreeNode* pre = nullptr;

public:
    void flatten(TreeNode* root) {
        if (!root)  return;

        flatten(root->right);
        flatten(root->left);

        root->right = pre;
        root->left = nullptr;
        pre = root;
    }
};
```

#### 非递归先序遍历

利用栈保存上一个节点的右指针，并将其更新为当前节点。

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
    void flatten(TreeNode* root) {
        if (!root)  return;

        stack<TreeNode*> s;
        s.push(root);
        TreeNode* pre = nullptr;

        while (!s.empty())
        {
            auto tmp = s.top(); s.pop();

            // binary tree expend to linked list
            if (pre)
            {
                pre->right = tmp;
                pre->left = nullptr;
            }

            if (tmp->right) s.push(tmp->right);
            if (tmp->left)  s.push(tmp->left);
			
            // update 'pre' node to 'tmp'
            pre = tmp;
        }
    }
};
```



## Reference

https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by--26/

