---
title: leetcode题解-669-修剪二叉搜索树
mathjax: true
date: 2020-01-02 16:19:25
categories:
tags:
---

## [题目](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)

我的思路是先得到满足要求的节点，再重新构建二叉搜索树，不过显然不是最优解。

### [官方way](https://leetcode-cn.com/problems/trim-a-binary-search-tree/solution/xiu-jian-er-cha-sou-suo-shu-by-leetcode/)

#### 递归 + 状态判断

```java
class Solution {
    public TreeNode trimBST(TreeNode root, int L, int R) {
        if (root == null) return root;
        if (root.val > R) return trimBST(root.left, L, R);
        if (root.val < L) return trimBST(root.right, L, R);

        root.left = trimBST(root.left, L, R);
        root.right = trimBST(root.right, L, R);
        return root;
    }
}
```

剪枝思路：

- `root->val < L`, 

  root及左孩子剪枝，右孩子提升为root

- `root->val > R`

  root及右孩子剪枝，左孩子提升为root

- `L <= root->val && root->val <= R`

  不需要对root剪枝，递归调用左右孩子



## References

https://leetcode-cn.com/problems/trim-a-binary-search-tree/solution/xiu-jian-er-cha-sou-suo-shu-by-leetcode/