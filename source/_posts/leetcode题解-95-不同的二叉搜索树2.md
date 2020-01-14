---
title: leetcode题解-95-不同的二叉搜索树2
date: 2019-10-31 20:46:50
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 动态规划
- 树
- 树的遍历
- 递归
- ×
---



## [题目](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)



## 官方解

简要改自官网解。

利用树的遍历，以递归方式实现不同二叉树的构造。

{% asset_img tree.png tree %}

我们从序列 $1 ...n$ 中取出数字 $i$，作为当前树的树根。于是，剩余 $i - 1$ 个元素可用于左子树，$n - i$ 个元素用于右子树。
如 前文所述，这样会产生 $G(i - 1)$ 种左子树 和 $G(n - i)$ 种右子树，其中 $G$ 是卡特兰数。

现在，我们对序列 $1 ... i - 1$ 重复上述过程，以构建所有的左子树；然后对 $i + 1 ... n$ 重复，以构建所有的右子树。

这样，我们就有了树根 $i$ 和可能的左子树、右子树的列表。

最后一步，对两个列表循环，将左子树和右子树连接在根上。



C++实现

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
    // [start, end]
    vector<TreeNode*> generate_trees(int start, int end)
    {
        vector<TreeNode*> all_trees;

        if (start > end)
        {
            all_trees.push_back(NULL);
            return all_trees;
        }

        // pick up a root 
        for (int i = 0; i <= end; ++i)
        {
            vector<TreeNode*> leftTrees = generate_trees(start, i - 1);
            vector<TreeNode*> rightTrees = generate_trees(i + 1, end);

            for (auto l : leftTrees)
            {
                for (auto r : rightTrees)
                {
                    TreeNode* curTree(i);
                    curTree->left = l;
                    curTree->right = r;
                    all_trees.push_back(curTree);
                }
            }

        }

        return all_trees;
    }

    vector<TreeNode*> generateTrees(int n) 
    {
        if (n == 0)
        {
            return vector<TreeNode*>();
        }
        else
        {
            return generate_trees(1, n);
        }
    }
};
```

Java实现

```java
class Solution {
  public LinkedList<TreeNode> generate_trees(int start, int end) {
    LinkedList<TreeNode> all_trees = new LinkedList<TreeNode>();
    if (start > end) {
      all_trees.add(null);
      return all_trees;
    }

    // pick up a root
    for (int i = start; i <= end; i++) {
      // all possible left subtrees if i is choosen to be a root
      LinkedList<TreeNode> left_trees = generate_trees(start, i - 1);

      // all possible right subtrees if i is choosen to be a root
      LinkedList<TreeNode> right_trees = generate_trees(i + 1, end);

      // connect left and right trees to the root i
      for (TreeNode l : left_trees) {
        for (TreeNode r : right_trees) {
          TreeNode current_tree = new TreeNode(i);
          current_tree.left = l;
          current_tree.right = r;
          all_trees.add(current_tree);
        }
      }
    }
    return all_trees;
  }

  public List<TreeNode> generateTrees(int n) {
    if (n == 0) {
      return new LinkedList<TreeNode>();
```

## Reference

[官方解](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/solution/bu-tong-de-er-cha-sou-suo-shu-ii-by-leetcode/)