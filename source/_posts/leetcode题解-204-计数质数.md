---
title: leetcode题解-204-计数质数
date: 2019-11-27 20:36:34
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- 数学
- ×
---

## 





## [题目](https://leetcode-cn.com/problems/count-primes/)

最佳思路，**Sieve of Eratosthenes** 又或者叫 **数筛**

```java
class Solution {
    public int countPrimes(int n) {
    boolean[] isPrim = new boolean[n];
    Arrays.fill(isPrim, true);
    for (int i = 2; i * i < n; i++) 
        if (isPrim[i]) 
            for (int j = i * i; j < n; j += i) 
                isPrim[j] = false;
    
    int count = 0;
    for (int i = 2; i < n; i++)
        if (isPrim[i]) count++;
    
    return count;
    }
}
```

C++实现

```C++
class Solution {
    public int countPrimes(int n)
    {
        bool isPrime[n];
        memset(isPrime, true, sizeof(isPrime));
        
        for (int i = 2; i*i < n; ++i)
        {
            if(isPrime[i])
            {
                for (int j = i*i; j < n; j += i)
                {
					isPrim[j] = false;
                }
            }
		}
        
        int count = 0;
        for (int i = 2; i < n; ++i)
        {
            if (isPrime[i]) ++count;
		}
        
        return count;
	}
}
```



## 总结

- 质数的判断上的条件优化
  - 只用判断到$\sqrt{n}$即可，我之前都是判断到$n/2$
-  我对于数筛法的理解是：
  - 一种 **记忆化搜索**， 减少了重复子过程的计算。
    - 非质数 = 质数 * 质数
    - 非质数 = 非质数 * 非质数/质数



## References

[labuladong 解](https://leetcode-cn.com/problems/count-primes/solution/ru-he-gao-xiao-pan-ding-shai-xuan-su-shu-by-labula/)