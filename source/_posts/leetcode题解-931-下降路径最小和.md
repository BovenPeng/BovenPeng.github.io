---
title: leetcode题解-931-下降路径最小和
date: 2019-11-02 22:38:48
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- 2维dp
- 2维DP-to-1维DP
- √
---

## [题目](https://leetcode-cn.com/problems/minimum-falling-path-sum/)



## My way

### 1.二维DP

```C++
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& A) {
        int m = A.size();
        int n = A[0].size();
        vector<vector<int>> dp(A);

        // base case
        for (int j = 0; j < n; ++j)
        {
            dp[0][j] = A[0][j];
        }

        // 
        for (int i = 1; i < m; ++i)
        {
            for (int j = 0; j < n; ++j)
            {
                if (j - 1 == -1)
                {
                    dp[i][j] = min(dp[i-1][j], dp[i-1][j+1]) + A[i][j];
                }
                else if (j + 1 == n)
                {
                    dp[i][j] = min(dp[i-1][j-1], dp[i-1][j]) + A[i][j];
                }
                else
                {
                    dp[i][j] = min(min(dp[i-1][j-1], dp[i-1][j]), dp[i-1][j+1]) + A[i][j];
                }
            }
        }

        return *min_element(dp[m-1].begin(), dp[m-1].end());
    }
};
```



### 2.一维DP

可以将二维DP压缩至一维DP处理。

## 

