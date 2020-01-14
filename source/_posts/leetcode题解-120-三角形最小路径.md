---
title: leetcode题解-120-三角形最小路径
date: 2019-10-31 15:55:13
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 动态规划
- 2维DP
- 2维DP-to-1维DP
- 自顶向下DP
- 自底向上DP
- 树
- 插入1个虚拟0
- ×
---

## [题目](https://leetcode-cn.com/problems/triangle/submissions/)

## My way

很简单的二维dp思路，每一层的元素依赖于上一层同列元素和上一层前一列元素。

### 二维DP

#### 状态定义：

$$
\begin{align*}
& i:\ 行号 \\
& j:\ 列号 \\
& triangle[i][j]:\ 原始三角形上的i行,\ j列的数字 \\
& dp[i][j]:\ dp数组的i行,\ j列上的数字 \\
& 0 <= i == j <= n, n为行数
\end{align*}
$$



#### 状态转移方程:

$$
dp[i][j] = triangle[i][j] + min\{dp[i-1][j-1],\ dp[i-1][j] \}
$$



#### Base case:

$$
dp[i][j] = triangle[i][j],\ \text{if $i == 0$ and $j == 0$}
$$



#### Corner case:

$$
dp[i][j] = triangle[i][j] + dp[i-1][j],\ \text{if $j == 0$} \\
dp[i][j] = triangle[i][j] + dp[i-1][j-1],\ \text{if $i == j$}
$$



##### C++实现

```c++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        vector<vector<int>> dp(triangle);

        for (int i = 0; i < triangle.size(); ++i)
        {
            for (int j = 0; j < triangle[i].size(); ++j)
            {
                if (i == 0 && j == 0)
                {
                    dp[i][j] = triangle[0][0];
                }
                else if (i != 0 && j == 0)
                {
                    dp[i][j] = triangle[i][j] + dp[i-1][j];
                }
                else if (i == j)
                {
                    dp[i][j] = triangle[i][j] + dp[i-1][j-1];
                }
                else
                {
                    dp[i][j] = triangle[i][j] + min(dp[i-1][j-1], dp[i-1][j]);
                }
            }
        }

        return *min_element(dp[triangle.size()-1].begin(), dp[triangle.size()-1].end());
    }
};
```



### 一维DP

如何仅使用$O(n)$大小的额外空间呢？

因为每一行的状态仅依赖于上一行的状态，故可将二维DP to 一维DP.

对空间复杂度进行优化。

#### 状态定义：

$$
\begin{align*}
& i:\ 行号 \\
& j:\ 列号 \\
& triangle[i][j]:\ 原始三角形上的i行,\ j列的数字 \\
& dp[k]:\ dp数组的第k个数字 \\
& 0 <= j <= i <= k == n, n为行数 \\
\end{align*}
$$

#### 状态转移方程:

$$
dp[j] = triangle[i][j] + min\{dp[j-1], dp[j]\}
$$



#### Base case:

$$
dp[j] = triangle[i][j],\ \text{if $i == 0$ and $j == 0$}
$$



#### Corner case:

$$
dp[j] = triangle[i][j] + dp[j],\ \text{if $j == 0$} \\
dp[j] = triangle[i][j] + dp[j-1],\ \text{if $i == j$}
$$



##### C++实现

```C++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        vector<int> dp(triangle.size());

        for (int i = 0; i <= triangle.size() - 1; ++i)
        {
            for (int j = triangle[i].size() - 1; j >= 0; --j)
            {
                if (i == 0 && j == 0)
                {
                    dp[j] = triangle[0][0];
                }
                else if (i != 0 && j == 0)
                {
                    dp[j] = triangle[i][j] + dp[j];
                }
                else if (i == j)
                {
                    dp[j] = triangle[i][j] + dp[j-1];
                }
                else
                {
                    dp[j] = triangle[i][j] + min(dp[j-1], dp[j]);
                }
            }
        }

        return *min_element(dp.begin(), dp.end());
    }
};
```



## Elon way

关键能有这个思路上的转变：

**自顶向下最短距离** <=> **自底向上最短距离**



### 自顶向下, 记忆化搜索 

- 思路类似上面 **二维DP**。
- 还利用了**树的遍历**思维解决问题

```java
class Solution {
    int row;
    Integer[][] memo;

    public int minimumTotal(List<List<Integer>> triangle) {
        row = triangle.size();
        memo = new Integer[row][row];
        return helper(0,0, triangle);
    }
    private int helper(int level, int c, List<List<Integer>> triangle){
        // System.out.println("helper: level="+ level+ " c=" + c);
        if (memo[level][c]!=null)
            return memo[level][c];
        if (level==row-1){
            return memo[level][c] = triangle.get(level).get(c);
        }
        int left = helper(level+1, c, triangle);
        int right = helper(level+1, c+1, triangle);
        return memo[level][c] = Math.min(left, right) + triangle.get(level).get(c);
    }
}
```



### 自底向上, DP 

```java
public int minimumTotal(List<List<Integer>> triangle) {
    int row = triangle.size();
    int[] minlen = new int[row+1];
    for (int level = row-1;level>=0;level--){
        for (int i = 0;i<=level;i++){   //第i行有i+1个数字
            minlen[i] = Math.min(minlen[i], minlen[i+1]) + triangle.get(level).get(i);
        }
    }
    return minlen[0];
}
```



## 总结

- 这里的**自顶向下**，**自底向上** 感觉就是我之前所理解的**二维DP**。不过我是采用非递归方式实现的，而这里是递归方式实现的。
- *思考题目后，思路上的转变:
  - **自顶向下最短距离** <=> **自底向上最短距离**
- 以及多多尝试 **递归方式实现此类DP**。

## Reference

[Elon leetcode解法](https://leetcode-cn.com/problems/triangle/solution/di-gui-ji-yi-hua-sou-suo-zai-dao-dp-by-crsm/)

### extended

[How to find minimum value from vector? [closed]](https://stackoverflow.com/questions/13015932/how-to-find-minimum-value-from-vector)