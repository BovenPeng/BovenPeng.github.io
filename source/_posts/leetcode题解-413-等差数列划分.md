---
title: leetcode题解-413.等差数列划分
date: 2019-11-03 22:11:43
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- 数组
- 递归
- ×
---

## My way

#### 状态转移方程:

$$
dp[i][j] = 
\begin{cases}
& 2^{j-i-2}-1 & \text{if $A[i:j]$是等差数列}\\
& dp[i-1][j] + dp[i][j-1] & otherwise\\
\end{cases}
$$





## 官方解

### 2. 优雅暴力



### 3. 递归

#### 状态定义：

$$
\begin{align*} 
& sum:\ 数组A中的所有的等差数列的个数 \\
& slice(A,\ i):\ 求区间(k,\ i)中，而不在区间(k,\ j)中等差数列的个数，其中j<i. \\
&
\end{align*}
$$

- 假设知道了$slice(A, i-1)$的值为x, 同时这个区间元素用$[a_0, a_1, a_2, ..., a_{i-1}]$来表示。
- 若这个区间本身就是一个等差数列，那么这里所有相邻元素之间的差值是相等的。
- 现加入一个元素$a_i$将区间扩展为$(0,\ i)$，如果扩展之后的区间还是一个等差数列，那么一定存在$a_i - a_{(i-1)} == a_{(i-1)} -a_{(i-2)}$.
- 故每加入一个新元素，就会多出$ap$个等差数列。其中新增的等差数列的区间为$(0,i), (1,i), ..., (i-2, i)$, 这些区间总数为$x+1$. 这是因为除了区间$(0,i)$之外，其余的区间如$(1,i), (2,i),...,(i-2,i)$都可以对应到之前的区间$(0,i-1), (1,i-1), ...,(i-3,i-1)$上去，其值为$x$.



- 因此，每次调用`slices`, 如果第$i$个元素与前一个元素的差值正好等于之前的差值，我们直接就可以算出新增的等差数列的个数$ap$, 同时可以更新$sum$. 
- 但是，如果新元素跟之前一个元素的差值不等于之前的差值，也就不会增加等差数列的个数

```java
public class Solution {
    int sum = 0;
    public int numberOfArithmeticSlices(int[] A) {
        slices(A, A.length - 1);
        return sum;
    }
    public int slices(int[] A, int i) {
        if (i < 2)
            return 0;
        int ap = 0;
        if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
            ap = 1 + slices(A, i - 1);
            sum += ap;
        } else
            slices(A, i - 1);
        return ap;
    }
}
```



### 4.DP

#### 状态定义：

$$
\begin{align*}
& dp:\ 存储在区间(k,i), 而不在区间(k,j)中的等差数列个数，其中j<i\\
& 
\end{align*}
$$

```java
public class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int[] dp = new int[A.length];
        int sum = 0;
        for (int i = 2; i < dp.length; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                dp[i] = 1 + dp[i - 1];
                sum += dp[i];
            }
        }
        return sum;
    }
}
```



### 5.$O(1)空间复杂度$DP

对于$dp$数组来说，仅有$dp[i-1]$决定$dp[i]$.

故只用保存最近一个$dp$ 就可以了。

```java
public class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int dp = 0;
        int sum = 0;
        for (int i = 2; i < A.length; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                dp = 1 + dp;
                sum += dp;
            } else
                dp = 0;
        }
        return sum;
    }
}
```



### 6. 公式计算



```java
public class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int count = 0;
        int sum = 0;
        for (int i = 2; i < A.length; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                count++;
            } else {
                sum += (count + 1) * (count) / 2;
                count = 0;
            }
        }
        return sum += count * (count + 1) / 2;
    }
}
```



## reference



### extended

[How to sum up elements of a C++ vector?](https://stackoverflow.com/questions/3221812/how-to-sum-up-elements-of-a-c-vector)

[how to sum up a vector of vector int in C++ without loops](https://stackoverflow.com/questions/58681103/how-to-sum-up-a-vector-of-vector-int-in-c-without-loops)

