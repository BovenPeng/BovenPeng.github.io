---
title: leetcode题解-979-在二叉树中分配硬币
mathjax: true
date: 2020-01-02 13:58:50
categories:
tags:
---

## [题目](https://leetcode-cn.com/problems/distribute-coins-in-binary-tree/)

### [官方way](https://leetcode-cn.com/problems/distribute-coins-in-binary-tree/solution/zai-er-cha-shu-zhong-fen-pei-ying-bi-by-leetcode/)

```java
class Solution {
    int ans;
    public int distributeCoins(TreeNode root) {
        ans = 0;
        dfs(root);
        return ans;
    }

    public int dfs(TreeNode node) {
        if (node == null) return 0;
        int L = dfs(node.left);
        int R = dfs(node.right);
        ans += Math.abs(L) + Math.abs(R);
        return node.val + L + R - 1;
    }
}
```

一种抽象的思维：

- 抽象：硬币数 $\rightarrow$ 负载量，考虑成: $一个节点的负载量 = abs(硬币数-1)$
- 节点间的硬币的移动数量: `abs(dfs(node->left))` +  `abs(dfs(node->right))`



## Reference

https://leetcode-cn.com/problems/distribute-coins-in-binary-tree/solution/zai-er-cha-shu-zhong-fen-pei-ying-bi-by-leetcode/