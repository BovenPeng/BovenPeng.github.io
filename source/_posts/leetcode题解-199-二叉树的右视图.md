---
title: leetcode题解-199-二叉树的右视图
mathjax: true
date: 2020-01-05 10:19:59
categories:
tags:
---

## 

## 

### 官方解

#### 非递归dfs

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
    vector<int> rightSideView(TreeNode* root) 
    {
        unordered_map<int, int> rightMostValAtDepth;
        stack<TreeNode*> nodeStack;
        stack<int> heightStack;
        int maxDepth = -1;

        nodeStack.push(root);
        heightStack.push(0);

        while(!nodeStack.empty())
        {
            root = nodeStack.top(); nodeStack.pop();
            int depth = heightStack.top(); heightStack.pop();

            if (root)
            {
                maxDepth = max(maxDepth, depth);

                if (!rightMostValAtDepth.count(depth))
                {
                    rightMostValAtDepth[depth] = root->val;
                }

                nodeStack.push(root->left);
                nodeStack.push(root->right);
                heightStack.push(depth+1);
                heightStack.push(depth+1);
            }
        }

        vector<int> vi;
        for (int depth = 0; depth <= maxDepth; ++depth)
        {
            vi.push_back(rightMostValAtDepth[depth]);
        }
        return vi;
    }
};
```

非递归实现DFS遍历 + 带 树高 状态



#### 非递归BFS

##### java实现

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        Map<Integer, Integer> rightmostValueAtDepth = new HashMap<Integer, Integer>();
        int max_depth = -1;

        /* These two Queues are always synchronized, providing an implicit
         * association values with the same offset on each Queue. */
        Queue<TreeNode> nodeQueue = new LinkedList<TreeNode>();
        Queue<Integer> depthQueue = new LinkedList<Integer>();
        nodeQueue.add(root);
        depthQueue.add(0);

        while (!nodeQueue.isEmpty()) {
            TreeNode node = nodeQueue.remove();
            int depth = depthQueue.remove();

            if (node != null) {
                max_depth = Math.max(max_depth, depth);

                /* The last node that we encounter at a particular depth contains
                * the correct value, so the correct value is never overwritten. */
                rightmostValueAtDepth.put(depth, node.val);

                nodeQueue.add(node.left);
                nodeQueue.add(node.right);
                depthQueue.add(depth+1);
                depthQueue.add(depth+1);
            }
        }

        /* Construct the solution based on the values that we end up with at the
         * end. */
        List<Integer> rightView = new ArrayList<Integer>();
        for (int depth = 0; depth <= max_depth; depth++) {
            rightView.add(rightmostValueAtDepth.get(depth));
        }

        return rightView;
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
public:
    vector<int> rightSideView(TreeNode* root) 
    {
        unordered_map<int, int> rightMostValAtDep;
        queue<TreeNode*> nodeQue;
        queue<int> heightQue;
        int maxDepth = -1;

        nodeQue.push(root);
        heightQue.push(0);

        while(!nodeQue.empty())
        {
            root = nodeQue.front();     nodeQue.pop();
            int height = heightQue.front(); heightQue.pop();

            if (root)
            {
                maxDepth = max(maxDepth, height);

                rightMostValAtDep[height] = root->val;

                nodeQue.push(root->left);
                nodeQue.push(root->right);
                heightQue.push(height+1);
                heightQue.push(height+1);
            }
        }

        vector<int> vi;
        for (int i = 0; i <= maxDepth; ++i)
        {
            vi.push_back(rightMostValAtDep[i]);
        }

        return vi;
    }
};
```

非递归实现BFS遍历 + 带 树高 状态



