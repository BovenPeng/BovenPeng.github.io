---
title: leetcode题解-14-最长公共前缀
mathjax: true
date: 2020-02-11 01:27:26
categories:
tags:
---

## [题目](https://leetcode-cn.com/problems/longest-common-prefix/)

### 官方解

#### 1.水平扫描法

##### 思路：

$$
LCP(S_1 ...S_n) = LCP(LCP(LCP(S_1, S_2), S_3), ...Sn)
$$

求$2$个字符串的最长前缀一般来说是每个字符串从头比较到尾，直到不相等为止。

但假如是$n$个字符串之间的比较的话，为了避免每次比较从头到尾进行，可以根据如上述公式，

- 选取一个字符串$A$作为前缀，长度为$k$;
- 若该字符串$A$与另一个$B$的前$k$字符串不同；

- 则去掉一个字符串$A$中的末尾字符，形成新字符串$A'$，

- 与串$B$比较直至满足前缀完全相等的情况；

- 再将得到的最大公共前缀与串$C,...,N$比较，按上述循环进行，直至满足条件。

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string s_0 = strs.size() ? strs[0] : "";

        for (auto& str: strs)
        {
            # 前缀串s_0 与 串str的前s_0.size()部分不等，则前缀串s_0去掉末尾字符
            while (str.substr(0, s_0.size()) != s_0)
            {
                s_0 = s_0.substr(0, s_0.size()-1);
                if (s_0 == "")  return "";
            }
        }

        return s_0;
    }
};
```

