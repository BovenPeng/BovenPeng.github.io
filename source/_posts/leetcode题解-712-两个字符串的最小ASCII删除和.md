---
title: leetcode题解-712-两个字符串的最小ASCII删除和
date: 2019-11-01 15:16:47
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 动态规划
- 字符串
- 插入1个虚拟0
- ×
---



## [题目](https://leetcode-cn.com/problems/minimum-ascii-delete-sum-for-two-strings/)



## 官方解

#### 状态定义：

$$
\begin{align*}
& i:\ \text{字符串}s1的第i位\\
& j:\ \text{字符串}s2的第j位\\
& s1[i:]:\ 第i位到末尾的子串\\
& s2[j:]:\ 第j位到末尾的子串\\
& dp[i][j]:\ 表示使字符串s1[i:],s2[j:]从第i位到末尾的子串相等，要删除的ASCII码值最小 
\end{align*}
$$



#### 状态转移方程：

$$
\begin{align*}
dp[i][j] = \begin{cases} & min(dp[i+1][j] + s1.asciiAtPos(i), dp[i][j+1] + s2.asciiAtPos(j))\ & \text{if $s1[i] != s2[j]$ }\\
& df[i-1][j-1]\ &\text{if $s1[i] = s2[j]$} \end{cases}
\end{align*}
$$



#### Base case:

$$
\begin{align*}
&dp[i][j] = 0  &\text{if $i == 0$ and $j == 0$} 
\end{align*}
$$



#### Corner case:

$$
dp[i][j] = 
\begin{cases}
& dp[i+1][j] + s1.asciiAtPos(i) & \text{if $s[j:].size() == 0$, 即$j==s1.size()$}\\
& dp[i][j+1] + s2.asciiAtPos(j) & \text{if $s[i:].size() == 0$, 即$i==s1.size()$}\\
\end{cases}
$$





#### C++实现

```C++
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        vector<vector<int>> dp(s1.size()+1, vector<int>(s2.size()+1));
        
        int m = s1.size(), n = s2.size();

        for (int i = m - 1; i >= 0; --i)
        {
            dp[i][n] = dp[i+1][n] + s1[i];
        }
        for (int j = n - 1; j >= 0; --j)
        {
            dp[m][j] = dp[m][j+1] + s2[j];
        }

        for (int i = m - 1; i >= 0; --i)
        {
            for (int j = n - 1; j >= 0; --j)
            {
                if (s1[i] == s2[j])
                {
                    dp[i][j] = dp[i+1][j+1];
                }
                else
                {
                    dp[i][j] = min(dp[i+1][j] + s1[i], dp[i][j+1] + s2[j]);
                }
            }
        }
        

        return dp[0][0];
    }

private:
    int getStrAsciiSum(const string s)
    {
        int sum = 0;
        for (auto i : s)
        {
            sum += i;
        }
        return sum;
    }
};
```



#### Java实现

```java
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int[][] dp = new int[s1.length() + 1][s2.length() + 1];

        for (int i = s1.length() - 1; i >= 0; i--) {
            dp[i][s2.length()] = dp[i+1][s2.length()] + s1.codePointAt(i);
        }
        for (int j = s2.length() - 1; j >= 0; j--) {
            dp[s1.length()][j] = dp[s1.length()][j+1] + s2.codePointAt(j);
        }
        for (int i = s1.length() - 1; i >= 0; i--) {
            for (int j = s2.length() - 1; j >= 0; j--) {
                if (s1.charAt(i) == s2.charAt(j)) {
                    dp[i][j] = dp[i+1][j+1];
                } else {
                    dp[i][j] = Math.min(dp[i+1][j] + s1.codePointAt(i),
                                        dp[i][j+1] + s2.codePointAt(j));
                }
            }
        }
        return dp[0][0];
    }
}
```



## 总结

- 类似于 [最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence) 的一道题。
- 在 二维DP数组初始化的时候，没有考虑到初始化大小为$(m+1)*(n+1)$ .
  - 对于$dp[m][n] = 0$其实是一种 **base case**，
  - 是考虑到第$m$位字符到第$m$位字符上的一个情况，即 空串情况。



## Reference

[leetcode官方解](https://leetcode-cn.com/problems/minimum-ascii-delete-sum-for-two-strings/solution/liang-ge-zi-fu-chuan-de-zui-xiao-asciishan-chu-he-/)

[Falcon解](https://leetcode-cn.com/problems/minimum-ascii-delete-sum-for-two-strings/solution/asciima-zui-da-de-zi-xu-lie-zui-chang-gong-tong-zi/)



### Extended

[std::basic_string](https://en.cppreference.com/w/cpp/string/basic_string)

[basic_string/substr](https://en.cppreference.com/w/cpp/string/basic_string/substr)