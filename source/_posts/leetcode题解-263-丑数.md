---
title: leetcode题解-263-丑数
date: 2019-11-27 16:39:57
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 数学
- ×
---



## [题目](https://leetcode-cn.com/problems/ugly-number/)

```C++
class Solution {
public:
    bool isUgly(int num) {

        while (num != 1)
        {
            int ori = num;
            if (num % 2 == 0)
            {
                num /= 2;
            }
            if (num % 3 == 0)
            {
                num /= 3;
            }
            if (num % 5 == 0)
            {
                num /= 5;
            }

            if (ori == num)
            {
                return false;
            }
        }

        return true;
    }
};
```

#### 思路：

丑数： 因数包括 1，2， 3， 5的数。

所以考虑 **非丑数** 的2种情况：

1. 除了1，2，3，5的质数
2. 因数除了 1，2，3，5外，还有质数的非质数



- 模2， 3，5后，这个数还是原来的值 => 非丑数
- 模2， 3，5后，这个数为1 => 丑数
- 模2， 3，5后，这个数为一个不为1的数，继续模2，3，5，直至出现上面两种情况。







## 总结

- 数学题，考虑找规律，找补集思考
- 质数 常涉及到 $/, \%$运算？
- 