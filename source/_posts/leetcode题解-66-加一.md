---
title: leetcode题解-66-加一
mathjax: true
date: 2020-02-19 01:48:22
categories:
tags:
- 数组
---

## [题目](https://leetcode-cn.com/problems/plus-one/)

```C++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) 
    {
        for (int i = digits.size() - 1; i >= 0; --i)
        {
            ++digits[i];
            digits[i] %= 10;
			
            // 说明不需要在最前面加一个 “1” 了
            if (digits[i]) return digits;
        }

        // 例子： 999 + 1 = 1000
        // 首位还需加上一个1
        digits.insert(digits.begin(), 1);
        return digits;    
    }
};
```



## 总结

- 很简单的循环判断，一开始也AC，但是代码很冗余，可读性差些，根据reference中的代码解进行修改。

## References

https://leetcode-cn.com/problems/plus-one/solution/java-shu-xue-jie-ti-by-yhhzw/