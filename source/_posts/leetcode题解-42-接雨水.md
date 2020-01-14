---
title: leetcode题解-42-接雨水
mathjax: true
date: 2020-01-05 21:32:52
categories:
tags:
---

## [题目](https://leetcode-cn.com/problems/trapping-rain-water/)

### 官方way

#### 3. (最大)单调递减栈

```C++
class Solution {
public:
    int trap(vector<int>& height) 
    {
        stack<int> s;
        int cur = 0;
        int ans = 0;

        while (cur != height.size())
        {
            while (!s.empty() && height[cur] > height[s.top()])
            {
                int top = s.top();
                s.pop();

                if (s.empty())  break;

                int dist = cur - s.top() - 1;
                int bounded_height = min(height[s.top()], height[cur]) - height[top];
                ans += dist * bounded_height;
            }

            s.push(cur++);
        }    

        return ans;
    }
};
```

##### 维护一个最大单调递减栈的流程如下

让数组中元素依次进栈:

- **假如栈空**，元素直接进栈；

- **假如栈顶top元素小于数组cur元素**，则将数组内元素依次弹出直至：

  - **栈顶元素大于数组cur元素**

  - **栈空**

    - **若栈顶元素pop()后且此时栈不为空**，

      `dist = cur - s.top() - 1;`, 

      `bounded_height = min(height[s.top()], height[cur]) - height[top];`

      `ans += dist * bounded_height;`

      由此累加`cur`元素到`top`元素的矩形长度进`ans`;

- **假如数组中无元素**，则对栈中元素依次出栈;

PS：在这里是对数组的索引进行进栈操作，而不是具体到元素。