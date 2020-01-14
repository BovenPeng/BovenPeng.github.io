---
title: leetcode题解-324-4的幂
date: 2019-12-02 15:24:44
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 位运算
- ×
---



## [题目](https://leetcode-cn.com/problems/power-of-four/solution/)

### 非位运算解：

```C++
class Solution {
public:
    bool isPowerOfFour(int num) {
        
        if (num == 0) return false;

        int mod;

        while (num != 1)
        {
            mod = num % 4;
            num /= 4;
            
            if (mod != 0)
            {
                return false;
            }
        }

        return true;
    }
};
```



### 位运算：

```C++
class Solution {
public:
    bool isPowerOfFour(int num) {
        return num > 0 && !(num&(num-1)) && !(num & 0xAAAAAAAA);
    }
};
```

考虑：

- `num > 0`时，才会是一个数的幂。负数和0不会是一个正整数的幂。
- `!(num&(num-1))`时，用来判断`num`是不是2的幂，
  - 因为一个数是2的幂的话，则它2进制的第一位必然是1，与`num-1`与运算后，结果必然为0
- `!(num & 0xAAAAAAAA)`, 用来判断`num`是不是4的幂，
  - 因为一个数是4的幂的话，则它2进制必然第一位是1且第一位是奇数位，`0xA=1010`，则4的幂与其求与运算后，则为0，求反得1

## 总结

位运算小技巧：

- `!(n&(n-1))`，用于判断`n`是不是2的幂
- `!(num&(num-1)) && !(num & 0xAAAAAAAA)`, 用于判断`n`是不是4的幂
- 对于`n`的幂，可以先枚举一定的例子，然后对枚举结果进行归纳找到规律。



## Reference

https://leetcode-cn.com/problems/power-of-four/solution/e-you-shi-yi-dao-zhuang-bi-jie-fa-de-suan-fa-ti-2/

https://www.geeksforgeeks.org/program-to-find-whether-a-no-is-power-of-two/

https://www.geeksforgeeks.org/find-whether-a-given-number-is-a-power-of-4-or-not/