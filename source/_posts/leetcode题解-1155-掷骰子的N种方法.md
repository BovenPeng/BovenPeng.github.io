---
title: leetcode题解-1155-掷骰子的N种方法
date: 2019-11-21 21:46:27
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- ×
---



```C++
class Solution {
public:
    int numRollsToTarget(int d, int f, int target) 
    {
        if (d < 1 || f < 1 || target < 1 || target > d*f)
        {
            return 0;
        }

        // 1.
        int MOD = 1000000007;
        // 2.
        int MIN = min(f, target);
        // 3.
        int MAX = d*f;

        vector<vector<int>> dp(d+1, vector<int>(MAX+1, 0));

        for (int j = 1; j <= MIN; ++j)
        {
            dp[1][j] = 1;
        }

        for (int i = 2; i <= d; ++i)
        {
            // 因为每个筛子至少丢出来的值是 >=1 的
            for (int j = i; j <= MAX; ++j)
            {
                for (int k = 1; j - k >= 0 && k <= f; ++k)
                {
                    dp[i][j] = (dp[i][j] + dp[i-1][j-k]) % MOD;
                }
            }
        }


        return dp[d][target];
    }

};
```

