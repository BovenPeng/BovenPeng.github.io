---
title: 'leetcode题解: 1025.除数博弈'
date: 2019-10-24 22:17:03
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 数学题
- DP
- ×
---

## [题目](https://leetcode-cn.com/problems/divisor-game/)

## My way:

这题靠归纳推理，首先很明显就能看出来偶数的时候Alice赢了；

然后多举了几个例子就发现奇数的时候Bob赢了。

就直接一个条件语句，结束。

但是尝试套dp的做法的时候，没捋清楚，看了综评最高的，基本懂了。

```C++
class Solution {
public:
    bool divisorGame(int N) {
        if (!(N & 1))
        {
            return true;
        }
        else
        {
            return false;
        }
    }
};
```



## 综评最高解

### 1.归纳法：

基本思路：

- 最终结果应该是占到 2 的赢，占到 1 的输；

- 若当前为奇数，奇数的约数只能是奇数或者 1，因此下一个一定是偶数；

- 若当前为偶数， 偶数的约数可以是奇数可以是偶数也可以是 1，因此直接减 1，则下一个是奇数；

因此，**奇则输，偶则赢**。

```python
class Solution:
    def divisorGame(self, N: int) -> bool:
        return N%2==0
```



### 2.DP:

- 定义状态
  - $i$: 数字$N$为$i$时；
  - $f[i]$: 数字$N$为$i$时，Alice的胜负状况，`true`为胜，`false`为负。
- 定义状态转移方程：

$$
f[i] = \begin{cases} 
true, & \text{if}\ f[i-j] == false,\ \text{if}\ i\ \%\ j == 0\ \\
      & and\ j\ is\ unsigned\ integer\ which \in (0, N) \  \\
false, & \text{otherwise}
\end{cases}
$$



#### 实现

```C++
class Solution {
public:
    bool divisorGame(int N) {
        vector<bool> vi(N+1);

        for (int i = 2; i < N + 1; ++i)
        {
            for (int j = 1; j <= i / 2; ++j)
            {
                if (vi[i-j] == false && i % j == 0)
                {
                    vi[i] = true;
                }
            }
        }


        return vi[N];
    }
};
```

综评解

```python
class Solution:
    def divisorGame(self, N: int) -> bool:
        target = [0 for i in range(N+1)]
        target[1] = 0 #若爱丽丝抽到1，则爱丽丝输
        if N<=1:
            return False
        else:
        
            target[2] = 1 #若爱丽丝抽到2，则爱丽丝赢
            for i in range(3,N+1):
                for j in range(1,i//2):
                    # 若j是i的余数且target[i-j]为假（0）的话，则代表当前为真（1）
                    if i%j==0 and target[i-j]==0:
                        target[i] = 1
                        break
            return target[N]==1
```





## 总结

- 初等数学review:
  - 奇数的因数： {奇数}
  - 偶数的因数： {偶数}
  - 奇数的最大公因数 = 奇数
- 目前做到的dp还是简单递推问题，可以简单套换成：
  - 状态$i$就是数字为$i$的时候
  - $f[i]$就是数字为$i$时候，结果为$f[i]$

## Reference

[pandawakaka 给出的解](https://leetcode-cn.com/problems/divisor-game/solution/python3gui-na-fa-by-pandawakaka/)