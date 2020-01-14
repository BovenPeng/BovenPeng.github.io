---
title: leetcode题解-714-买卖股票的最佳时机含手续费
date: 2019-11-01 21:17:26
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 贪心算法
- 动态规划
- 数组
- ×
---

## ~~My way~~

一开始审题失误了，没有看到股票可以 **多次买入，卖出**。

所以想了个 **只准单次买入，卖出的解法**。

```C++
class Solution {
private:
    static bool mycomp(vector<int> a, vector<int> b) 
    {
        return *max_element(a.begin(), a.end()) < *max_element(b.begin(), b.end());
    }

public:
    int maxProfit(vector<int>& prices, int fee) {
        size_t n = prices.size();
        vector<vector<int>> dp(n, vector<int>(n));

        // Base case
        for (size_t i = 0; i < n; ++i)
        {
            dp[i][i] = 0;
        }      

        for (size_t i = 0; i < n; ++i)
        {
            for (size_t j = i + 1; j < n; ++j)
            {
                dp[i][j] = dp[i][j-1] + prices[j] - prices[j-1] - fee;
            }
        }

        auto itv = max_element(dp.begin(), dp.end(), mycomp); // find the vector 
                                                     // with the max element
        return *max_element((*itv).begin(), (*itv).end()); // finds the max element 
                                                        // in the desired vector
    }
};
```



## 官方解

通过第$i$天和$i-1$天$cash,\ hold$的递推关系，来得到最后最大收益。

*但是给我的感觉，这个思路，并不是很好捋清楚。

#### 状态定义：

$$
\begin{align*}
& cash:\ 不持有股票时的最大利润 \\
& hold:\ 持有股票时的最大利润\\
\end{align*}
$$

#### 状态转移方程：

```java
cash = Math.max(cash, hold + prices[i] - fee);	// max(原来请况，假设第i天卖出股票)
hold = Math.max(hold, cash - prices[i]);		// max(原来情况，假设第i天买进股票)
```





#### Base case

```java
cash = 0;
hold = -prices[0]; // 假设第0天买了股票
```





```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int cash = 0, hold = -prices[0];
        for (int i = 1; i < prices.length; i++) {
            cash = Math.max(cash, hold + prices[i] - fee);
            hold = Math.max(hold, cash - prices[i]);
        }
        return cash;
    }
}
```



#### e.g. 

$$
\begin{align*}
&输入: prices = [1, 3, 2, 8, 4, 9], fee = 2\\
&输出: 8\\
&解释: 能够达到的最大利润:  \\
&在此处买入 prices[0] = 1\\
&在此处卖出 prices[3] = 8\\
&在此处买入 prices[4] = 4\\
&在此处卖出 prices[5] = 9\\
&总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8. \\

\end{align*}
$$

|      | prices | cash             | hold            |
| ---- | ------ | ---------------- | --------------- |
| 0    | 1      | 0                | -1              |
| 1    | 3      | max(0, -1+3-2)=0 | max(0, 0-3)=-3  |
| 2    | 2      | max(0, -1+2-2)=0 | max(-1, 0-2)=-1 |
| 3    | 8      | max(0, -1+8-2)=5 | max(-1, 5-8)=-1 |
| 4    | 4      | max(5, -1+4-2)=5 | max(-1, 5-4)=1  |
| 5    | 9      | max(5, 1+9-2)=8  | max(1, 8-9)=-1  |





## Reference

[官方解](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/solution/mai-mai-gu-piao-de-zui-jia-shi-ji-han-shou-xu-fei-/)

[laluladong解](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/solution/yi-ge-fang-fa-tuan-mie-6-dao-gu-piao-wen-ti-by-l-2/)



### extended

[Find the max value in vector> without for loop](https://stackoverflow.com/questions/36228231/find-the-max-value-in-vectorvectorint-without-for-loop)

[markdown generator](https://www.tablesgenerator.com/markdown_tables)