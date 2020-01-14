---
title: leetcode题解-312-戳气球
date: 2019-11-04 15:57:30
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- 分治算法
- 哨兵
- ×
---

## My way

#### 很蠢的思路：

- 选择`min`值，爆掉，然后循环至结束。

测都不用测。。。

没想到怎么往DP上面套。



## ivan_allen 解

#### 思路转变：

**气球从头到尾爆炸 <=> 气球从尾到头爆炸**



#### 状态转移方程:

$$
dp[i][j] = max(dp[i][j],\ nums[i]*nums[k]*nums[j] + dp[i][k] + dp[k][j]), \text{if $k \in\ [i,\ j]$}
$$



#### 遍历方式：

- 控制子数组长度`len`
- 子数组首尾`i`, `j`
- 使用`k`对数组分割



#### 二维DP实现：

```C++
class Solution {
public:
    int N;
    int maxCoins(vector<int>& nums) {
        
        // 添加哨兵在头尾，因为假如最后只删一个数字的时候，是需要乘以2个1的
        nums.insert(nums.begin(), 1);
        nums.push_back(1);

        int m = nums.size();
        vector<vector<int>> dp(m, vector<int>(m));

        // 只能按长度打表
        // dp[i][j], j - i >= 2
        for (int len = 2; len < m; ++len)
        {
            for (int i = 0; i < m - len; ++i)
            {
                int j = i + len;
                for (int k = i + 1; k < j; ++k)
                {
                    dp[i][j] = max(dp[i][j], nums[i]*nums[k]*nums[j] + dp[i][k] + dp[k][j]);
                } 
            }
        }

        return dp[0][m-1];
    }

};
```



## 总结

- 思路的转变：

  - **气球从头到尾爆炸 <=> 气球从尾到头爆炸**

- **头尾虚拟1的添加** 类似于之前**虚拟0的添加** 和 **哨兵的思想**

  



## Reference

[ivan_allen解](https://leetcode-cn.com/problems/burst-balloons/solution/dong-tai-gui-hua-by-ivan_allen-2/)

