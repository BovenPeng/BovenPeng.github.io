---
title: leetcode题解-145-二叉树的后序遍历
date: 2019-12-08 20:49:13
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的遍历
- √
---



## [题目](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

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
        helper(root->right);
        vi.push_back(root->val);
    }

public:
    vector<int> postorderTraversal(TreeNode* root) {
        helper(root);

        return vi;
    }
};
```



#### 非递归手动压栈+逆序输出

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
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> s;

        if (root)
        {
            s.push(root);

            while(!s.empty())
            {
                auto top = s.top();    s.pop();
                vi.push_back(top->val);

                if (top->left)     s.push(top->left);
                if (top->right)    s.push(top->right);
            }
        }

        return vector<int>(vi.rbegin(), vi.rend());
    }
};
```





#### 非递归手动压栈 + 辅助变量判断右孩子是否访问过

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
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> mystack;
        vector<int> ans;
        TreeNode* curr = root;
        TreeNode* pre = NULL;
        
        while(curr || !mystack.empty())
        {
            while(curr)
            {
                mystack.push(curr);
                curr = curr->left;
            }
            curr = mystack.top();
            
            //若右节点已经访问过或者没有右节点  则输出该节点值
            if(!curr->right || pre == curr->right){
                mystack.pop();
                ans.push_back(curr->val);    
                pre = curr;
                curr = NULL;
            }else{
                curr = curr->right;
                pre = NULL;
            }
        }
        return ans;
    }
};
```



## References

https://leetcode-cn.com/problems/binary-tree-postorder-traversal/solution/c-by-jjjjjz-2/