---
title: leetcode题解-94-二叉树的中序遍历
date: 2019-12-08 22:34:10
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的遍历
- √
---

## [题目](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

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
    vector<int> vi;
    void helper(TreeNode* root)
    {
        if (!root)  return ;
        helper(root->left); 
        vi.push_back(root->val);
        helper(root->right);
    }

public:
    vector<int> inorderTraversal(TreeNode* root) {
        helper(root);

        return vi;    
    }
};
```



#### 手动压栈实现

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
    vector<int> vi;

public:
    vector<int> inorderTraversal(TreeNode* root)     
    {
        if (root) 
        {
            stack<TreeNode*> s;

            while(!s.empty() || root)
            {
                if (root)   
                {
                    s.push(root);
                    root = root->left;
                }
                else 
                {
                    root = s.top(); s.pop();
                    vi.push_back(root->val);
                    root = root->right;
                }
            }
            
        }   

        return vi;
    }
};
```



## 总结

- 因为中序遍历是 "**左根右**"，所以得
  - 先遍历到最左的结点，则这个结点可能是 "**左根右**"中的 **左** 或者 **根** 或者 **null**
  - 若为 **左**/**根**， 则 都需保存它的右孩子(为**空**/**不空**)，然后继续考察他的 ”**左根右**“结构。
  - 若**不为null**， 则 保存当前结点，且继续往左孩子遍历
  - 若 **为null**，则 弹栈 并 打印当前栈顶元素值，然后往右孩子遍历