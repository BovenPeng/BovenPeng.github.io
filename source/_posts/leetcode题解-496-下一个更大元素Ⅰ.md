---
title: leetcode题解-496-下一个更大元素Ⅰ
mathjax: true
date: 2020-01-06 14:40:05
categories:
tags:
- 单调栈
---

## [题目](https://leetcode-cn.com/problems/next-greater-element-i/)

### [官方解way](https://leetcode-cn.com/problems/next-greater-element-i/solution/xia-yi-ge-geng-da-yuan-su-i-by-leetcode/)

#### 最大单调递增栈

```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> map;
        stack<int> s;

        for (int i = 0; i < nums2.size(); ++i)
        {
            while (!s.empty() && s.top() < nums2[i])
            {
                map[s.top()] = nums2[i];
                s.pop();
            }
            s.push(nums2[i]);
        }

        while (!s.empty())
        {
            map[s.top()] = -1;
            s.pop();
        }

        vector<int> vi;
        for (int i = 0; i < nums1.size(); ++i)
        {
            vi.push_back(map[nums1[i]]);
        }

        return vi;
    }
};
```

#### 最大单调递增栈的流程：

数组中元素依次进栈:

1. 假如**栈不为空** 且 **栈顶元素小于数组当前元素**, 则将 **栈顶元素出栈** 直至 **栈不为空** 或 **栈顶元素 大于等于 数组当前元素**
   - 将栈顶元素和第一个大于栈顶(的栈顶右边)元素，形成一个`tuple`
   - 此处是放到`unordered_map`中来
2. 当前元素进栈
3. 当数组中所有元素遍历完之后，将栈中元素依次弹出
4. 遍历存放`tuple`的容器



## 总结

这是一个很经典的单调栈题。



## Reference

https://leetcode-cn.com/problems/next-greater-element-i/solution/xia-yi-ge-geng-da-yuan-su-i-by-leetcode/