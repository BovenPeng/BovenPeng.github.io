---
title: leetcode题解-104-二叉树的最大深度
date: 2019-12-04 21:57:18
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- DFS
- ×
---

## [题目](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

### My way

#### 简单递归实现

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
    int maxDepth(TreeNode* root) {
        if (!root)  return 0;

        return max(maxDepth(root->left)+1, maxDepth(root->right)+1);
    }
};
```

时间$O(N)$仅超过51%，空间$O(log(N)) + O(N) = O(N)$超过19%，说明递归压栈吃了不少空间，尝试改成非递归。



#### 手动压栈

```C++
class Solution {
public:

int maxDepth(TreeNode* root){
	stack<pair<TreeNode*, int>> stack;
	
	if (root)	stack.push(pair(root, 1));
	
	int depth = 0;
	while (!stack.empty())
	{
		pair<TreeNode*, int> cur = stack.top();
        stack.pop();
        root = cur.first;
        int cur_depth = cur.second;

        if (root)
        {
            depth = max(depth, cur_depth);
            stack.push(pair(root->left, cur_depth+1));
            stack.push(pair(root->right, cur_depth+1));
        }
	}

    return depth;
}
};
```

时间$O(N)$仅超过51%，空间$O(N)$超过50%，手动压栈节省了不少空间。

有没有时间上更优化的解法呢？



#### 层次遍历



### 



## References

https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/er-cha-shu-de-zui-da-shen-du-by-leetcode/

