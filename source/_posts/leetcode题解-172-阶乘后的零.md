---
title: leetcode题解-172-阶乘后的零
mathjax: true
date: 2019-12-30 20:03:57
categories:
- 数学
- 数论
tags:
---

## [题目](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)

- 求$N!$在尾部有多少个0，即求有多少个2和5.

- 由于实际上在$N!$中，2的个数多于5，所以求5的个数即可，所以结果是$N/5$吗？

- 并不是，因为每过$5^i$个数之后，都会多出现一个5.

  $result = N/5 + N/(5^2) + N/(5^3) + ... =((N/5)/5)/5...$

```C++
class Solution {
public:
    int trailingZeroes(int n) {
        int count = 0;
        while (n)
        {
            count += n/5;
            n /= 5;
        }

        return count;
    }
};
```



## Reference

https://leetcode-cn.com/problems/factorial-trailing-zeroes/solution/xiang-xi-tong-su-de-si-lu-fen-xi-by-windliang-3/

https://leetcode-cn.com/problems/factorial-trailing-zeroes/solution/q172-factorial-trailing-zeroes-by-ronhou/

