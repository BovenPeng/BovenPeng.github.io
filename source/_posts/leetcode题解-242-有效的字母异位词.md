---
title: leetcode题解-242-有效的字母异位词
mathjax: true
date: 2020-01-23 22:35:25
categories:
tags:
- 哈希
---

## [题目](https://leetcode-cn.com/problems/valid-anagram/)

### My way

#### c++实现

```C++
class Solution {
public:
    bool isAnagram(string s, string t) {
        unordered_map<char, int> m;

        if (s.size() < t.size())    swap(s, t);

        for (auto& it : s)
        {
            --m[it];
        }

        for (auto& it: t)
        {
            ++m[it];
        }

        for (auto& it: m)
        {
            if (it.second != 0)  return false;
        }

        return true;
    }
};
```

- 利用哈希表构成一个**计数器**的效果。
- 利用`ctor`初始化`int`变量默认值是0，
  - 在`s`里出现过，则自减1；
  - 在`t`里出现过，则自增1；
- 要注意，`s`, `t`字符串的大小可能不一样。所以需要在计数前，交换一次。



### [官方 way](https://leetcode-cn.com/problems/valid-anagram/solution/you-xiao-de-zi-mu-yi-wei-ci-by-leetcode/)

```C++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size())   return false;

        int counter[26] = {0};

        for (size_t i = 0; i < s.size(); ++i)
        {
            ++counter[s[i] - 'a'];
            --counter[t[i] - 'a'];
        }

        for (auto& it: counter)
        {
            if (it != 0) return false;
        }

        return true;
    }
};
```

- 利用一个`int`数组构造出一个计数器



## References

https://leetcode-cn.com/problems/valid-anagram/solution/you-xiao-de-zi-mu-yi-wei-ci-by-leetcode/