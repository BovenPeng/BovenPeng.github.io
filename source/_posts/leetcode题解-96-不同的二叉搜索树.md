---
title: leetcode题解-96-不同的二叉搜索树
date: 2019-10-29 20:50:08
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 动态规划
- 树
- Catalan数
- 插入1个虚拟0
- ×
---

## [题目](https://leetcode-cn.com/problems/unique-binary-search-trees/)

## 官方解

思考题目中，输入为$n$，表示由$n$个节点来构成二叉搜索树。

可以考虑到的是，每个子树的根节点不同，则为一个新的子树结构(包括 **叶节点**)。

所以要考虑到如何从 **根节点** 入手，以及对于当前根节点的左右子树的递推的方式解决。



### 动态规划

#### 状态定义：

考虑到涉及到 **输入节点个数$n$** 和所取 **根节点$i$**，可以定义如下：
$$
\begin{align*}
& F(i,\ n) = \text{以$i$为根节点，长度为$n$的二叉搜索树的树的个数} \\
& G(n) = \text{长度为$n$的二叉搜索树的树的个数} \\
\end{align*}
$$
可以得到如下

#### 状态转移方程/递推公式：

$$
\begin{align*}
& G(n) = \sum_{i=1}^{n}F(i,\ n) \\
& F(i,\ n) = G(i-1) * G(n-i),\ 因为同一长度n的二叉搜索树个数一样 \\
& so\ we\ get: \\
& G(n) = \sum_{i=1}^{n}\{G(i-1) * G(n-i)\} \\
& 0 <= i <= n
\end{align*}
$$



#### Base case:

$$
\begin{align*}
& G(0) = 0\\
& G(1) = 1\\
& 0 <= i <= n
\end{align*}
$$

$G[0] = 0$，

- 一方面说明 **空树也是一种二叉搜索树**。
- 一方面为了 **构造的递推式中涉及到乘法，要保证乘积因子不为0才可**。



```java
public class Solution {
  public int numTrees(int n) {
    int[] G = new int[n + 1];
    G[0] = 1;
    G[1] = 1;

    for (int i = 2; i <= n; ++i) {
      for (int j = 1; j <= i; ++j) {
        G[i] += G[j - 1] * G[i - j];
      }
    }
    return G[n];
  }
}
```



### 数学way

#### 数学归纳法：

可以归纳得出：$G(n)$的值被称为Catalan数$C_n$。

定义如下：
$$
\begin{align*}
& C_0 = 1, \\
& C_{n+1} = \frac{2(2n+1)}{n+2} C_n
\end{align*}
$$

```java
class Solution {
  public int numTrees(int n) {
    // Note: we should use long here instead of int, otherwise overflow
    long C = 1;
    for (int i = 0; i < n; ++i) {
      C = C * 2 * (2 * i + 1) / (i + 2);
    }
    return (int) C;
  }
}
```



## 总结

- 要考虑到二叉搜索树的基本定义之一
  - 左子树上的值 < 根节点 < 右子树上的值
  - **空树也是二叉搜索树的一种**
- *如何摸索出状态定义的中的2个状态：$n$, $i$.
  - 题目已给条件中的状态有: **构成二叉搜索树的序列长度**$n$。
  - 考虑到子树的根节点不同，影响到二叉搜索树的结构，所以推测出: **$i$是$[0,\ n]$中取的一个值作为根节点**。
- ***Catalan数**，一个组合数学问题常常涉及到的。

## Reference

[leetcode官方解](https://leetcode-cn.com/problems/unique-binary-search-trees/solution/bu-tong-de-er-cha-sou-suo-shu-by-leetcode/)

[wikipidia-cn: catalan number]([https://zh.wikipedia.org/zh-hk/%E5%8D%A1%E5%A1%94%E5%85%B0%E6%95%B0](https://zh.wikipedia.org/zh-hk/卡塔兰数))

[baidu baike: 卡特兰数](https://baike.baidu.com/item/%E5%8D%A1%E7%89%B9%E5%85%B0%E6%95%B0)



### extended

[C++ memory model](https://en.cppreference.com/w/cpp/language/memory_model)

[C memory model](https://en.cppreference.com/w/c/language/memory_model)

