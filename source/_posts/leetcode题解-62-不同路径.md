---
title: leetcode题解-62-不同路径
date: 2019-11-04 15:34:12
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- 组合数学
- 2维DP-to-1维DP
- 数组
- ×
---



## My way

### 1. 2维DP

#### 状态转移方程：

$$
\begin{align*}
& dp[i][j] = dp[i-1][j] + dp[i][j-1] \\
\end{align*}
$$



#### base case:

$$
\begin{align*}
& dp[i][j] = 1,\ \text{if $i==1$ or $j==1$} \\
\end{align*}
$$



#### corner case:

$$
\begin{align*}
& dp[i][j] = 1, \text{if $i == 1$ and $j == 1$} \\
& AND \\
& return\ function
\end{align*}
$$





### 2. 1维DP

还可以转换成1维DP.





## powcai way

### 组合数学：

PS： 这个排列组合的方法其实我想到过，但是没有找出通项公式 :cry:





## Reference

[powcai 解](https://leetcode-cn.com/problems/unique-paths/solution/dong-tai-gui-hua-by-powcai-2/)