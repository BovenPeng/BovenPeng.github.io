---
title: leetcode题解-739-每日温度
mathjax: true
date: 2020-01-07 16:48:20
categories:
tags:
- 单调栈
---

## [题目](https://leetcode-cn.com/problems/daily-temperatures/)

### My way

#### 最大单调递增栈

```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) 
    {
        stack<int> s;
        vector<int> res(T.size());
        unordered_map<int, int> map;

        for (size_t i = 0; i < T.size(); ++i)
        {
            while(!s.empty() && T[s.top()] < T[i])
            {
                map[s.top()] = i - s.top();
                s.pop();
            }

            s.push(i);
        }

        while (!s.empty())
        {
            map[s.top()] = 0;
            s.pop();
        }

        for (size_t i = 0; i < res.size(); ++i)
        {
            res[i] = map[i];
        }

        return res;
    }
};
```

但是发现时间/空间复杂度都很低，

- 不应该是所有的`元素`和`右边最大(小)元素`都用`tuple`存放在一个容器内。
- 而应该遍历的时候就直接将结果存放起来

优化代码结构后：

```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) 
    {
        stack<int> s;
        vector<int> res(T.size());

        for (size_t i = 0; i < T.size(); ++i)
        {
            while(!s.empty() && T[s.top()] < T[i])
            {
                res[s.top()] = i - s.top();
                s.pop();
            }

            s.push(i);
        }


        return res;
    }
};
```



## Reference

https://leetcode-cn.com/problems/daily-temperatures/solution/mei-ri-wen-du-by-leetcode/

