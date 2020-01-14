---
title: leetcode题解-1143-最公共子序列长
date: 2019-11-03 19:37:42
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- 2维dp
- 最大公共子序列
- 插入1个虚拟0
- ×
---

## [题目](https://leetcode-cn.com/problems/longest-common-subsequence/submissions/)

简单DP，只给出状态转移方程。

#### 状态转移方程

$$
dp[i][j] = 
\begin{cases}
&	dp[i-1][j-1]+1, & \text{if $text1[i]==text2[j]$} \\
&   max(dp[i-1][j],\ dp[i][j-1]), & otherwise \\
\end{cases}
$$



```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size() + 1, n = text2.size() + 1;
        vector<vector<int>> dp(m, vector<int>(n));

        for (int i = 1; i < m; ++i)
        {
            for (int j = 1; j < n; ++j)
            {
                if (text1[i-1] == text2[j-1])
                {
                    dp[i][j] = dp[i-1][j-1] + 1;
                }
                else
                {
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }

        return dp[m-1][n-1];
    }
};
```



## 总结

- 这里只求，**最大公共子序列的长度**，简单的2维DP即可实现
- 记住，这种在状态转移方程中存在`f[i] = f[i-1]`的递推式情况下，
  - 将设置 `新数组长度 = 旧数组长度 + 1`
  - 从而可以减少在`base case`, `corner case`的语句，减少代码冗余程度。