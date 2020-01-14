---
title: leetcode题解-231-2的幂
date: 2019-12-02 16:07:54
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 位运算
- ×
---



## [题目](https://leetcode-cn.com/problems/power-of-two/submissions/)

```C++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > 0 && !(n&(n-1));
    }
};
```

考虑：

- `num > 0`时，才会是一个数的幂。负数和0不会是一个正整数的幂。

- `!(num&(num-1))`时，用来判断`num`是不是2的幂，

  - 因为一个数是2的幂的话，则它2进制的第一位必然是1，与`num-1`与运算后，结果必然为0

  

## Reference

https://leetcode-cn.com/problems/power-of-four/solution/e-you-shi-yi-dao-zhuang-bi-jie-fa-de-suan-fa-ti-2/

https://www.geeksforgeeks.org/program-to-find-whether-a-no-is-power-of-two/

