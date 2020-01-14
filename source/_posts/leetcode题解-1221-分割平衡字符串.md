---
title: leetcode题解-1221-分割平衡字符串
date: 2019-10-28 21:56:31
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 贪心算法
- 栈
- 线性表
- 括号匹配
- ×
---

## [题目](https://leetcode-cn.com/problems/split-a-string-in-balanced-strings/submissions/)

虽然打的是 **贪心算法** 的tag，但是我感觉实际上是括号匹配的简单变种。

## 解法

### 括号匹配/栈的做法

考虑L, R分别对应左右两个括号。

利用stack的`push`, `pop`和括号匹配的基本思路来完成。

1. 栈为空，`push`当前元素。
2. `top`元素 等于 当前元素，`push`当前元素，否则`pop` `top`元素。
3. 栈为空，平衡次数+1。



#### PS: 注意思路实现是带有先后顺序的。

```C++
class Solution {
public:
    int balancedStringSplit(string s) {
        int count = 0;
        stack<char> sc;
        for (int i = 0; i < s.size(); ++i)
        {
            char c = s[i];
            
            if (sc.empty() || c == sc.top())
            {
                sc.push(c);
            }
            else
            {
                sc.pop();
            }

            if (sc.empty())
            {
                ++count;
            }
        }

        return count;
    }
};
```



### 利用数字计数代替括号匹配做法

因为这里不同于括号匹配需要是一个`<>`的匹配状态，可以是`LR`, `RL`, `LLRR`, `RRLL`，

也就是说可以看成一个**简单版本的括号匹配**过程，

因为不需要要求`L`, `R`之间的先后关系，所以可以通过 **数字计数** 取代掉 **栈括号匹配** 的过程。

1. 若当前元素为`L`, 计数`++num`；为`R`, 计数`--num`;
2. 若`num == 0`, 则平衡次数+1。

```c++
class Solution {
public:
    int balancedStringSplit(string s) {
        int num = 0; 
        int count = 0;

        for (auto it = s.begin(); it < s.end(); ++it)
        {
            if (*it == 'L')
            {
                ++num;
            }
            else
            {
                --num;
            }

            if (num == 0)
            {
                ++count;
            }
        }

        return count;
    }
};
```



## 总结

- 这题提及的"**尽可能多的平衡字符串分割**“ + 实例，说明了这里的字符串分割是不存在前后分割出的字符串结果有交集的。
  - 可以思考到，正常的从头到尾的一次遍历即可。
- 提及的” **‘L’和'R'字符的数量是相同的** ", 并没有提及到 **L**和**R**要有前后顺序的关系
  - 可以思考到，利用 **数字计数法** 代替 **括号匹配法**。



## Reference

[Karua leetcode 题解](https://leetcode-cn.com/problems/split-a-string-in-balanced-strings/solution/java-tan-xin-suan-fa-by-nan-lin/)

[amanehayashi leetcode 题解](https://leetcode-cn.com/problems/split-a-string-in-balanced-strings/solution/java-ji-yu-zhan-shi-xian-by-amanehayashi/)



### Extended

[std::stack](https://en.cppreference.com/w/cpp/container/stack)

[STL header stack](https://en.cppreference.com/w/cpp/header/stack)

[iterator library](https://en.cppreference.com/w/cpp/iterator)

