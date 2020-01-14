---
title: leetcode题解-64-最小路径和
date: 2019-10-30 23:06:41
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 动态规划
- 2维DP
- 2维DP-to-1维DP
- 数组对角遍历
- √
---



## My way

第一眼感觉就是一个当前表格状态依赖于左边表格状态和上边表格状态的二维DP题。

### 状态定义：

$$
\begin{align*}
& i:\ 表示第i行 \\
& j:\ 表示第j列 \\
& [i,\ j]:\ 表示第i行，j列上的数字\\
& grid[i][j]:\ 表示原始网格上第i行，j列上的数字\\
& dp[i][j]:\ 表示经过某些步之后，达到第i行，j列的时候，这个数字和的最小值\\
\end{align*}
$$



### 状态转移方程：

$$
\begin{align*}
& dp[i][j] = grid[i][j] + max(df[i-1][j],\ df[i][j-1])
\end{align*}
$$



### Base Case:

$$
\begin{align*}
& df[0][0] = grid[0][0]
\end{align*}
$$



### Corner Case:

$$
\begin{align*}
& df[i][j] = grid[i][j] + df[i, j-1],\ \text{if $j-1$ == 0} \\
& df[i][j] = grid[i][j] + df[i-1, j],\ \text{if $j-1$ == 0}
\end{align*}
$$



C++实现

```C++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        vector<vector<int>> vvi(grid);

        int m = grid.size(), n = grid[0].size();

        for (int i = 0; i < m; ++i)
        {
            for (int j = 0; j < n; ++j)
            {
                if (i == 0 && j == 0)
                {
                    vvi[i][j] = grid[i][j];
                }
                else if (i == 0)
                {
                    vvi[i][j] = grid[i][j] + vvi[i][j-1];
                }
                else if (j == 0)
                {
                    vvi[i][j] = grid[i][j] + vvi[i-1][j];
                }
                else 
                {
                    vvi[i][j] = grid[i][j] + min(vvi[i-1][j], vvi[i][j-1]);
                }
            } 
        }
        return vvi[m-1][n-1];
    }
};
```



### 可以试试斜着遍历

!对于$m*n\ array$来说，不太好实现。

有一个思路就是补成正方形之后，然后以对角线的方式倾斜遍历数组。



## 官方解

### 1.暴力

不做解释了。

### 2.二维DP

类似我的思路。

### 3.一维DP

因为这里每个阶段的状态仅依赖上一个表格状态和左一个表格的状态，

再加上我们利用按行遍历的方式，依次求出每一行对应的$dp[i][j]$，然后得到结果。



#### 官方Java解

```java
public class Solution {
   public int minPathSum(int[][] grid) {
       int[] dp = new int[grid[0].length];
       for (int i = grid.length - 1; i >= 0; i--) {
           for (int j = grid[0].length - 1; j >= 0; j--) {
               if(i == grid.length - 1 && j != grid[0].length - 1)
                   dp[j] = grid[i][j] +  dp[j + 1];
               else if(j == grid[0].length - 1 && i != grid.length - 1)
                   dp[j] = grid[i][j] + dp[j];
               else if(j != grid[0].length - 1 && i != grid.length - 1)
                   dp[j] = grid[i][j] + Math.min(dp[j], dp[j + 1]);
               else
                   dp[j] = grid[i][j];
           }
       }
       return dp[0];
   }
}
```

#### 重上到下，C++实现

```C++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        vector<int> dp(grid[0].size());

        for (int i = 0; i < grid.size(); ++i)
        {
            for (int j = 0; j < grid[0].size(); ++j)
            {
                if (i == 0 && j == 0)
                {
                    dp[j] = grid[0][0];
                }
                else if (i == 0 && j != 0)
                {
                    dp[j] = grid[i][j] + dp[j-1]; 
                }
                else if (i != 0 && j == 0)
                {
                    dp[j] = grid[i][j] + dp[j];
                }
                else 
                {
                    dp[j] = grid[i][j] + min(dp[j], dp[j-1]);
                }
            }
        }

        return dp[grid[0].size()-1];
    }
};
```





## Reference

[leetcode 官方解](https://leetcode-cn.com/problems/minimum-path-sum/solution/zui-xiao-lu-jing-he-by-leetcode/)



### extended

[Traverse an array diagonally](https://stackoverflow.com/questions/21346343/traverse-an-array-diagonally)

[Traverse 2D m*n Array (Matrix) Diagonally](https://stackoverflow.com/questions/2862802/traverse-2d-array-matrix-diagonally)