---
title: leetcode题解-654-最大二叉树
mathjax: true
date: 2019-12-11 21:25:17
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的构造
- 单调栈
- √
---



## [题目](https://leetcode-cn.com/problems/maximum-binary-tree/)

### My way

#### 递归构造树

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
    TreeNode* helper(const vector<int>& nums)
    {
        auto it = max_element(nums.begin(), nums.end());
        if (it == nums.end())   return nullptr;
        TreeNode* root = new TreeNode(*it);
        root->left = helper(vector<int>(nums.begin(), it));
        root->right = helper(vector<int>(it + 1, nums.end()));

        return root;
    }
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        TreeNode* root = helper(nums);

        return root;    
    }
};
```



### [infinite way](https://leetcode-cn.com/problems/maximum-binary-tree/solution/c-dong-tai-gui-hua-by-infinite-15-7/)

#### 单调栈

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
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        const int size = nums.size();
        vector<TreeNode*> st(size);
        int st_size = 0;
        for (const auto num : nums) {
            auto p = new TreeNode(num);
            if (st_size == 0 || st[0]->val < num) { // 栈内所有元素都比当前值小
                p->left = st[0];
                st[0] = p;
                st_size = 1;
            } else if (st[st_size - 1]->val > num) { // 栈内所有元素都比当前值大
                st[st_size - 1]->right = p;
                st[st_size++] = p;
            } else { // 二分查找临界位置
                int begin = 0;
                int end = st_size - 1;
                while (begin + 1 < end) {
                    auto half = (begin + end) / 2;
                    if (st[half]->val < num)
                        end = half;
                    else
                        begin = half;
                }
                p->left = st[end];
                st[begin]->right = p;
                st[begin + 1] = p;
                st_size = begin + 2;
            }
        }

        return st[0];
    }
};
```

修改了一下

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
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        vector<TreeNode*> st(nums.size());

        int st_size = 0;
        for (const auto& num : nums)
        {
            auto p = new TreeNode(num);

            if (st_size == 0 || st[0]->val < num)   // 大于栈顶元素
            {
                p->left = st[0];
                st[0] = p;
                st_size = 1;
            }
            else if (st[st_size - 1]->val > num)	// 小于栈底元素 
            {
                st[st_size - 1]->right = p;
                st[st_size++] = p;
            }
            else	// 在栈中元素时，要找到这个元素的位置，然后设置它的left结点指向小于它的元素
            {
                int begin = 0, end = st_size - 1;
                while (begin + 1 < end)
                {
                    int half = (end - begin) / 2 + begin;
                   
                    if (st[half]->val < num)    end = half;
                    else                        begin = half;
                }

                p->left = st[end];
                st[begin]->right = p;
                st[begin + 1] = p;
                st_size = begin + 2;
            }
        }

        return st[0];
    }
};
```



## 总结

- 这题刚好满足最大单调栈的应用场景，

  - 树结点的`left`, `right`的要求刚好对应单调栈里的左，右部分

    

## Reference

https://leetcode-cn.com/problems/maximum-binary-tree/solution/c-dong-tai-gui-hua-by-infinite-15-7/