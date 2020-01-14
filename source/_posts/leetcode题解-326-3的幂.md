---
title: leetcode题解-326-3的幂
date: 2019-12-02 16:23:28
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 数学
- ×
---





## [题目](https://leetcode-cn.com/problems/power-of-three/)

我只会第一种循环迭代，后面3种方法均出自leetcode官方题解。



### 1. 循环迭代

```C++
class Solution {
public:
    bool isPowerOfThree(int n) {
        if (n < 1)  return false;
        
        while (n % 3 == 0)
        {
            n /= 3;
        }

        return n == 1;
    }
};
```

时间: $O(log_b(n))$

空间: $O(1)$

### 2. 基准转换

将所有的数转化为以3为基数的数字。

```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return Integer.toString(n, 3).matches("^10*$");
    }
}
```

时间: $O(log_3n)$

空间: $O(log_3n)$



### 3. 运算法

$$
n = 3^i i = log_3(n)i = \frac{log_b(n)}{log_b(3)}
$$

```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return (Math.log10(n) / Math.log10(3)) % 1 == 0;
    }
}
```

为了解决精度错误，应该设置一个较小的`epsilon`

```java
return (Math.log(n) / Math.log(3) + epsilon) % 1 <= 2 * epsilon;
```



### 4. 整数限制

在Java中，该变量是4个byte，则最大值为$\frac{2^{32}}{2} - 1 = 2147483647$，并得到最大的3的幂为$3^{19} = 1162251467$.

- 若`n`是3的幂，则$ 3^{19} \% n == 0$必成立

```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
}
```

时间: $O(1 )$

空间: $O( 1)$





## References

https://leetcode-cn.com/problems/power-of-three/solution/3de-mi-by-leetcode/