---
title: 'leetcode题解: 338.比特位计数'
date: 2019-10-23 21:29:29
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 位运算
- DP
- 插入1个虚拟0
- ×
---

## [题目](https://leetcode-cn.com/problems/counting-bits/)

## My way:

这题tag是dp，按着dp的思路写出了 **状态**， **状态转移方程**包括 *边界条件*， *常规情况*。

大致思路如下：

- 定义状态：

  - $i$: 数字为$i$时；
  - $f[i]$: 数字为i时，$f[i]$表示有多少个1，在数字$i$的二进制表示形式下。

- 状态转移方程：

  - 边界：$f[0] = [\ \ ]$

  - 正常情况：(第一时间也拿不出来，通过举例拿到的，)

    eg.	

    $f[1] = 1$

    $f[2] = 1$

    $f[3] = 1 + 1 = f[2] + f[1]$

    $f[4] = 1$

    $f[5] = 2 = f[4] + f[1]$

    $f[6] = 2 = f[4] + f[2]$

    $f[7] = 3 = f[4] + f[2] + f[1]$	

    

    得到：
    $$
    f[i] = \begin{cases}
    [\ \ ], & \text{if}\ i == 0 \\
    1, & \text{if}\ i == 2^m,\ \text{m is unisgned integer} \\
    1 + f[i - j], & \text{if}\ 2^j < i < 2^{j+1},\ \text{j is unsigned integer}
    \end{cases}
    $$
    合并下情况有：
    $$
    f[i] = 1 + f[i-j],\ \text{if}\ 2^j < i < 2^{j+1},\ \text{j is unsigned integer}
    $$

然后以Cpp实现了如下：

```c++
class Solution {
public:
    vector<int> countBits(int num) {
        if (num == 0)
        {
            return vector<int>{0};
        }
        
        if (num == 1)
        {
            return vector<int> {0, 1};
        }
        
        vector<int> vi {0, 1};
        
        for (int i = 2; i < num+1; ++i)
        {
            int j;
            for (j = 0; pow(2, j) <= i; ++j)
            {}
            j = pow(2, j-1);
                    
            int val;
            if (i-j-1 == -1)
            {
                val = 1;
            }
            else
            {
                val = 1 + vi[i-j];
            }
            vi.push_back(val);
        }
        
        return vi;
    }
};
```

不过很遗憾这是一种**很低效的解法**



## Standard Way

详细解释见之后reference的官方解释link处，这里只给出最后的思路/状态转移方程和我的一些理解及实现。

### 1.Pop Count

#### 思路

利用位运算`x & (x - 1)`来不断消除最右边的一位1，直至数字为0时，可以算出这个数字有多少位1。

**PS**：确实没想到位运算这块，思维狭隘了。



#### 实现

```C++
class Solution {
public:
    vector<int> countBits(int num) {
        if (num == 0)
        {
            return vector<int>{0};
        }
        
        if (num == 1)
        {
            return vector<int> {0, 1};
        }

        vector<int> vi {0, 1, };

        for (int i = 2; i < num + 1; ++i)
        {
            vi.push_back(pop_count(i));
        }

        return vi;
    }

private:
    int pop_count(int num)
    {
        // 通过 x &= x - 1 的位运算操作，来消除掉一个数从左到右的最右边的一个1
        int count;
        for (count = 0; num != 0; ++count)
        {
            num &= num - 1;
        }

        return count;
    }
};
```

官方解这里利用了 default initialization来implicit初始化 未初始化的结果。

```java
public class Solution {
    public int[] countBits(int num) {
        int[] ans = new int[num + 1];
        for (int i = 0; i <= num; ++i)
            ans[i] = popcount(i);
        return ans;
    }
    private int popcount(int x) {
        int count;
        for (count = 0; x != 0; ++count)
          x &= x - 1; //zeroing out the least significant nonzero bit
        return count;
    }
}
```



### 2. 动态规划 + 最高有效位 

#### 状态转移方程

结合1解法的`pop_count` $P(x)$函数来，有以下状态转移函数：
$$
P(x+b) = P(x) + 1, b= 2^m > x
$$

#### 我的理解

能想到最左最高有效位刚好多一个1的情况，这个1在m位的话，则是多了$2^m$值。



#### 实现

```C++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> vi(num+1);
        // 可以不初始化为，gcc, clang下，float, double, size_t, unsigned会由default initialization初始化0
        // 同理, string会初始化为''
        //vi[0] = 0;
        
        int x = 0, b = 1;
        
        // 利用外部循环来找到满足 b = 2^m > x的情况
        // i = 0是因为每4个一循环，给下4个取值
        for ( ; b <= num; b <<= 1, x = 0)
        {
            while (x < b && x + b <= num)
            {
                vi[x+b] = vi[x] + 1;
                ++x;
            }
        }

        return vi;
    }
};
```



这里官方解的Java实现有个非常巧妙的地方：

- 利用了 `while (b <= num) {...}`的外部循环来找到 $b = 2^m > x$ 的$b$值

```java
# standard solution
public class Solution {
    public int[] countBits(int num) {
        int[] ans = new int[num + 1];
        int i = 0, b = 1;
        // [0, b) is calculated
        while (b <= num) {
            // generate [b, 2b) or [b, num) from [0, b)
            while(i < b && i + b <= num){
                ans[i + b] = ans[i] + 1;
                ++i;
            }
            i = 0;   // reset i
            b <<= 1; // b = 2b
        }
        return ans;
    }
}
```



### 3. 动态规划 + 最低有效位 

#### 状态转移方程

同样基于`pop_count()`函数
$$
P(x) = P(x/2) + (x\ mod\ 2)
$$

#### 我的理解

同理2解法，由最高有效位，想到最低有效位：



#### 实现

```C++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> vi(num+1);

        int i = 1;
        for ( ; i < num + 1; ++i)
        {
            vi[i] = vi[i/2] + (i % 2);
        }

        return vi;
    }
};
```



这里官方解，利用了位运算来代替已有的`/`和`%`运算：

- `x >> 1`  <=> `x / 2`
- `x & 1` <=> `x % 2`

```java
public class Solution {
  public int[] countBits(int num) {
      int[] ans = new int[num + 1];
      for (int i = 1; i <= num; ++i)
        ans[i] = ans[i >> 1] + (i & 1); // x / 2 is x >> 1 and x % 2 is x & 1
      return ans;
  }
}
```



### 4.动态规划 + 最后设置位

#### 状态转移方程

同样基于`pop_count()`函数
$$
P(x) = P(x\ \&\ (x-1)) + 1
$$

#### 我的理解

利用位运算`x & (x-1)`消除最右边的一位“1”，

使得 `x` 比 `x & (x-1)` 少一位“1”， 加上这个“1”即可.



#### 实现

```C++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> vi(num+1);
        
        for (int i = 1; i < num + 1; ++i)
        {
            vi[i] = vi[i & (i - 1)] + 1;
        }

        return vi;
    }
};
```

官方解：

```java
public class Solution {
  public int[] countBits(int num) {
      int[] ans = new int[num + 1];
      for (int i = 1; i <= num; ++i)
        ans[i] = ans[i & (i - 1)] + 1;
      return ans;
  }
}

```



## 总结

- 简单DP，注意定义好状态，写出状态转移方程，然后注意corner case。写成代码即可。

- 注意涉及到 **2进制**， **全0 1 的数字**， **非负整数, unsigned, size_t, size_type**等等，

  往位运算上面想。

- 总结这里的位运算操作，之后考虑全部总结下常见位运算操作, 我记得耗子有篇文章总结了很多这方面的。

  - `x & (x - 1)` 消除 `x` 的最右边的一位1. (PS: 这一个确实不熟悉，虽然也见过)
  - `x << 2` <=>  ` x * 2`
  - `x >> 2` <=>  `x / 2`
  - `x & 1`   <=>  `x % 2`      

  (PS：那篇文章link找不到了)

  

## Reference

[leetcode官方解答](https://leetcode-cn.com/problems/counting-bits/solution/bi-te-wei-ji-shu-by-leetcode/)

[Large braces for specifying values of variables by condition](https://tex.stackexchange.com/questions/9065/large-braces-for-specifying-values-of-variables-by-condition)



### Extended knowledge 

[ std::bitset<N>::bitset in cppreference](https://en.cppreference.com/w/cpp/utility/bitset/bitset)

[How to print (using cout) a number in binary form?](https://stackoverflow.com/questions/7349689/how-to-print-using-cout-a-number-in-binary-form)

[default initialization](http://www.enseignement.polytechnique.fr/informatique/INF478/docs/Cpp/en/cpp/language/default_initialization.html)

[lifetime](https://en.cppreference.com/w/cpp/language/lifetime)

[Why does c++ initialise a std::vector with zeros, but not a std::array?](https://stackoverflow.com/questions/48775464/why-does-c-initialise-a-stdvector-with-zeros-but-not-a-stdarray)