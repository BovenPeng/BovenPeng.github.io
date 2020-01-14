---
title: leetcode题解-1227-风机座位分配概率
date: 2019-10-31 18:55:54
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 动态规划
- 数学题
- 组合数学
- 插入1个虚拟0
- ×
---

## ~~My way~~

习惯不是很好。。最开始试答案试出来结果的。

$1/n$, $1/2$, 然后得到正确解。。

因为第一眼看上去就是组合数学题，就类似你买彩票，先买后买的顺序不影响概率。

### ~~简单思路~~

为什么试$1/n$，以为要考虑的是$n$个状态取1个。

后来发现应该是$1/2$，感觉解释起来就有问题了。



## 水氷/Leoooooo/winterock way

简单递推dp的思路，在此略作修改。

#### 状态定义：

$$
\begin{align*}
& i:\ 第i个入座的人 \\
& n:\ 一共有n个座位 \\
& f[i]:\ 第i个入座的人，坐到自己位置上的概率 \\
\end{align*}
$$

#### *状态转移方程：

$$
\begin{align*}
f[i] = 1/n + (n-2)/n * f(n-1) 
\end{align*}
$$

##### **Leoooooo**给出的推导过程：

$$
\begin{alignat}{1}
f(n) = & 1/n                      & // \text{1st person picks his own seat, probability = 1/n} \\
    &+ 1/n * 0                    & // \text{1st person picks last one's seat, probability = 1/n} \\
	&+ (n-2)/n *                  & // \text{1st person picks one of seat from 2nd to (n-1)th, probability = (n-2)/n} \\
    &( \\
	    &\ \ \ 1/(n-2) * f(n-1)         & // \text{1st person pick 2nd's seat, 2nd person becomes the new "careless" person, and there are n-1 seats left. it becomes subproblem of n-1.}\\
        &\ \ \ 1/(n-2) * f(n-2)         & // \text{1st person pick 3rd's seat, 2nd person will pick his own seat, 3nd person becomes the new "careless" person, and there are n-2 seats left. it becomes subproblem of n-2.}\\
        &\ \ \ ......\\
        &\ \ \ 1/(n-2) * f(2)           & // \text{1st person pick (n-1)th's seat, (n-1)th person becomes the new "careless" person, there are 2 seats left.}\\
	&) \\ \\
	
=> & f(n) = 1/n                                                      & \text{\{when $n <= 2$\}} \\
   & f(n) = 1/n + 1/n * ( f(n-1) + f(n-2) + f(n-3) + ... + f(2) )    & \text{\{when $n > 2$\}} \\
\end{alignat}
$$

根据**winterock**给出的解释，简单来说:

按照条件概率，分成3种情况，在此以1号为例子：

1. $1/n$, 第1个人，选择了自己的座位，则对$2,...,n$位置空出来了，他们必然可以做到自己的位置，即$n$可以坐到自己的位置；
2. $1/n*0$, 第1个人坐到$n$位置上，则$n$必然坐不到自己位置上；
3. $(n-2)/n * (...)$ ，这个情况，简单来说就是，如果$i$坐到$k$位置上，则$(i,\ k)$里的人都能坐到自己位置上来，因为每个人看到自己位置空的时候必然坐上来。具体举例不展开了，很容易想明白。

##### 证明结论为$f(n)=1/2$:

$$
\begin{align*}
& \text{When}\ n <= 2 \\
& \ f(n) = 1/n \\
& \text{When}\ n > 2\ \text{(all these 5 formulas are correct, we can choose any one of them)} \\
 & \ f(n) = 1/n + 1/n * ( f(n-1) + f(n-2) + f(n-3) + ... + f(2) )  \\
 & \ f(n+1) = f(n)\\
 & \ f(n) = 1/n + (n-2)/n * f(n)   \\
 & \ f(n) = 1/n + (n-2)/n * f(n-1) \ \text{is correct too since} f(n) == f(n-1) \\
 & \ f(n) = 1/2 \\
 \end{align*}
$$

```C++
class Solution {
public:
    double nthPersonGetsNthSeat(int n)
    {
        double f[n+1];
        f[1] = 1;
        if(n == 1)
            return 1;
        for (int i = 2; i <= n; ++i)
            f[i] = (1 + (i - 2) * f[i - 1]) / (double)i;
        return f[n];
    }
};
```



## 总结

- 怎么**求出合理的递推式用于状态转移方程**。
  - 这里是用了 **条件概率**来入手。
  - 然后通过思考发现了这样一个规律:
    - 如果$i$坐到$k$位置上，则$(i,\ k)$里的人都能坐到自己位置上来，因为每个人看到自己位置空的时候必然坐上来。
- 有没有从组合数学的思路的解法呢？



## Reference

[水氷 leetcode 解](https://leetcode-cn.com/problems/airplane-seat-assignment-probability/solution/han-you-zhuang-tai-zhuan-yi-fang-cheng-tui-chu-de-/)

[Leoooooo 状态转移方程详解](https://leetcode.com/problems/airplane-seat-assignment-probability/discuss/414864/True-formula-and-explanations-for-all-other-formulas)

[winterock 状态转移方程详解](https://leetcode.com/problems/airplane-seat-assignment-probability/discuss/411905/It's-not-obvious-to-me-at-all.-Foolproof-explanation-here!!!-And-proof-for-why-it's-12)