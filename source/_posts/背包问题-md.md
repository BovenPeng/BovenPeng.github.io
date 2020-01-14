---
title: 背包问题.md
date: 2019-11-29 19:43:14
mathjax: true
categories:
- 总结
tags: 
- DP
- 背包问题
---

背包问题总共有9种，称为**背包9讲**。

1. 01背包问题
2. 完全背包问题
3. 多重背包问题
4. 混合背包问题
5. 二维费用的背包问题
6. 分组背包问题
7. 背包问题求方案数
8. 求背包问题的方案
9. 有依赖的背包问题





有$N$件物品和一个容量是$V$的背包。

> 不同背包问题的条件：每件物品只能使用一次。

第$i$件物品的体积是 $v_i$，价值是 $w_i$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

#### 输入格式

第一行两个整数，$N, V$用空格隔开，分别表示物品数量和背包容积。

接下来有 $N$ 行，每行两个整数 $v_i, w_i$，用空格隔开，分别表示第 $i$ 件物品的体积和价值。

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$0<N,V≤10000$
$0<v_i,w_i≤10000$

#### 输入样例

```
4 5
1 2
2 4
3 4
4 5
```

#### 输出样例：

```
8
```



## 01背包问题

### 题目描述

有$N$件物品和一个容量是$V$的背包。

> 每件物品只能使用一次。

第$i$件物品的体积是 $v_i$，价值是 $w_i$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

#### 输入格式

第一行两个整数，$N, V$用空格隔开，分别表示物品数量和背包容积。

接下来有 $N$ 行，每行两个整数 $v_i, w_i$，用空格隔开，分别表示第 $i$ 件物品的体积和价值。

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$0<N,V≤10000$
$0<v_i,w_i≤10000$

#### 输入样例

```
4 5
1 2
2 4
3 4
4 5
```

#### 输出样例：

```
8
```



### 解

#### 状态：

状态$f[i][j]$表示前$i$个物品的总体积$j$下，总价值为$f[i][j]$.

$result = f[i][j]$

初始状态：$f[0][0] = 0$

终止状态：$f[n][m]$



#### 状态转移方程：

考虑第$i$个状态和$i-1$个状态间的递推关系:

- 不选第$i$个物品，$f[i][j] = f[i-1][j]$

- 选第$i$个物品，$f[i][j] = f[i-1][j-v[i]] + w[i]$

- $f[i][j] = max(f[i-1][j], f[i-1][j-v[i]]+ w[i])$

  其中$v[i]$是第$i$个物品的体积, $w[i]$是第$j$个物品的价值

```C++
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
int f[N][N];
int V[N], w[N];

int main()
{
	cin >> n >> m;
	for (int i = 1; i <= n; ++i) cin >> v[i] >> w[i];
	
	for (int i = 1; i <= n; ++i)
		for (int j = 0; j <= m; ++j)
		{
			f[i][j] = f[i-1][j];
			if (j >= m)
			{
				f[i][j] = max(f[i][j], f[i-1][j-v[i]] + w[i]);
			}
		}
		
	cout << f[n][m] << endl;
    
    return 0;
}
```



#### 因为状态$i$每次都仅依赖状态$i-1$，故可以优化额外空间复杂度为$O(logN)$

```C++
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
int f[N];
int v[N], w[N];

int main()
{
	cin >> n >> m;
	for (int i = 1; i <= n; ++i)	cin >> v[i] >> w[i];
	
	for (int i = 1; i <= n; ++i)
		for (int j = m; j >= v[i]; --j)
			f[j] = max(f[j], f[j-v[i]] + w[i]);
			
    /*
    所有f[i] = 0, 则输出f[m];
    假如仅f[0] = 0, 则需要遍历一遍枚举出最小值;
    */
	cout << f[m] << endl;
	return 0;
}
```

优化输入部分后：

```C++
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
int f[N];

int main()
{
	cin >> n >> m;
    for (int i = 0; i < n; ++i)
    {
        int v, w;
        cin >> v >> w;
        for (int j = m; j >=v; ++j)
        {
            f[j] = max(f[j], f[j-v] + w);
		}
	}

	cout << f[n] << endl;
    
    return 0;
}
```

- 因为要依赖dp表中上一层的状态，要先更新下标比较大的dp数组元素，此时通过状态转移方程求最大值的时候还未更新下标较小的dp数组元素，即下标较小的dp数组元素还是上一层的值，所以要逆序遍历。

- 要保证在1维DP中计算$f[j]$状态时，用的是$f[i-1][j-v[i]]$而不是$f[i][j-v[i]]$的

- ！不能理解的时候再去看dp表，因为要保证这一层的状态依赖于上一层的状态。

  - e.g. 若顺序计算的话

    对于 $i=2, j=6, v[2] = 3$，

    $dp[6]=max(dp[6], dp[6-v[2]]+w[2]) = max(dp[6], dp[3] + w[2])$

    因为动态规划是第$i$层的状态依赖于第$i-1$层的。

    这时，要计算$max$外的$dp[6]$，要用到$max$内的$dp[6], dp[3]$。

    其中$max$内的$dp[6]$是第$i-1$层的，是$dp[3]$是第$i$层的，故使得状态转移不满足要求，所以得逆序。

    可以见 [此处例子](https://www.cnblogs.com/lanhj/archive/2012/12/05/2802437.html)



## 完全背包问题

有$N$件物品和一个容量是$V$的背包。

> 每件物品都有无限件可用。

第$i$件物品的体积是 $v_i$，价值是 $w_i$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

#### 输入格式

第一行两个整数，$N, V$用空格隔开，分别表示物品数量和背包容积。

接下来有 $N$ 行，每行两个整数 $v_i, w_i$，用空格隔开，分别表示第 $i$ 件物品的体积和价值。

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$0<N,V≤10000$
$0<v_i,w_i≤10000$

#### 输入样例

```
4 5
1 2
2 4
3 4
4 5
```

#### 输出样例：

```
10
```



### 解：

```C++
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
int f[N];

int main()
{
	cin >> n >> m;
    for (int i = 0; i < n; ++i)
    {
        int v, w;
        cin >> v >> w;
        
        // 要累加每一次的结果
        for (int j = v; j <= m; ++j)
        {
            f[j] = max(f[j], f[j-v] + w);
		}
	}
		
    cout << f[m];
	return 0;
}
```

从代码上看就是，把 **01背包** 的代码的二重循环，由重后往前遍历，改成了重前往后遍历。

从状态转移方程上来看，通过二维DP表的形式:

- $f[i][j] = max(f[i-1][j], f[i-1][j-v[i]]+ w[i])$

  $max(不选第i个物品，\ 选了第i个物品)$

- $if\ j-v[i]*k <= 0 \\f[i][j] = max(f[i-1][j], f[i][j-v[i]]+ w[i], f[i][j-v[i]*2]+w[i]*2+...)$

  $max(不选第i个物品，\ 选了第i个物品1次, \ 选了第i个物品2次, ...)$

  > 现在完全背包的特点恰是每种物品可选无限件，所以在考虑“加选一件第i种物品”这种策略时，却正需要一个可能已选入第i种物品的子结果f[i][v-c[i]]，所以就可以并且必须采用v= 0..V的顺序循环。
  >
  > 或者由这个2维动态转移方程得来。





## 混合背包问题





## References

https://www.cnblogs.com/jbelial/articles/2116074.html

https://www.bilibili.com/video/av33930433?p=1

https://www.acwing.com/solution/acwing/content/3982/

