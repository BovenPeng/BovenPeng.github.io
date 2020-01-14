---
title: leetcode题解-264-丑数Ⅱ
date: 2019-11-27 21:47:20
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- 堆
- ×
---



## [题目](https://leetcode-cn.com/problems/ugly-number-ii/submissions/)

1.  用STL实现小根堆

按理说应该实现些其它功能，比如：

- vecotor中是否包含multiple elements
- 堆的一些常用功能，如：heapify

```C++
class Solution {
public:
    int nthUglyNumber(long n) {
        vector<long> vi = {1};

        make_heap(vi.begin(), vi.end(), greater<>());
        long top;
        for (long i = 0; i < n; ++i)
        {
            pop_heap(vi.begin(), vi.end(), greater<>());
            
            top = vi.back();
            vi.pop_back();

            if (find(vi.begin(), vi.end(), top*2) == vi.end())
            {
                vi.push_back(top*2);
            }
            if (find(vi.begin(), vi.end(), top*3) == vi.end())
            {
                vi.push_back(top*3);
            }
            if (find(vi.begin(), vi.end(), top*5) == vi.end())
            {
                vi.push_back(top*5);
            }

            make_heap(vi.begin(), vi.end(), greater<>());
        }

        return top;
    }
};
```

实际上效率低了，因为用原生的`make_heap`需要重新构造一次堆，但是用`heapify`这种调整堆结构会快很多。



2. DP实现

状态转移方程：
$$
dp[i] = min(2*dp[l_2], 3*dp[l_3], 5*dp[l_5])
$$

```C++
class Solution {
public:
    int nthUglyNumber(long n) {
        vector<long> dp(n+1);

        dp[1] = 1;
        long l2 = 1, l3 = 1, l5 = 1;

        for (long i = 2; i <= n; ++i)
        {
            dp[i] = min(2*dp[l2], min(3*dp[l3], 5*dp[l5]));

            if (dp[i] >= 2*dp[l2])
            {
                l2 += 1;
            }
            if (dp[i] >= 3*dp[l3])
            {
                l3 += 1;
            }
            if (dp[i] >= 5*dp[l5])
            {
                l5 += 1;
            }
        }

        return dp[n];
    }
};
```



## Reference

[powcai解](https://leetcode-cn.com/problems/ugly-number-ii/solution/dui-he-dong-tai-gui-hua-by-powcai/)