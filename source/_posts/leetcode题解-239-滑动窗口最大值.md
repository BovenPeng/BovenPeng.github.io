---
title: leetcode题解-239-滑动窗口最大值
mathjax: true
date: 2020-01-13 19:59:04
categories:
tags:
- 单调栈
- 单调队列
---

## [题目](https://leetcode-cn.com/problems/sliding-window-maximum/)

### [labuladong way](https://leetcode-cn.com/problems/sliding-window-maximum/solution/dan-diao-dui-lie-by-labuladong/)

#### 单调队列

```C++
class MonotonicQueue {
private:
    deque<int> data;
public:
    void push(int n) {
        while (!data.empty() && data.back() < n) 
            data.pop_back();
        data.push_back(n);
    }
    
    int max() { return data.front(); }
    
    void pop(int n) {
        if (!data.empty() && data.front() == n)
            data.pop_front();
    }
};

vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    MonotonicQueue window;
    vector<int> res;
    for (int i = 0; i < nums.size(); i++) {
        if (i < k - 1) { //先填满窗口的前 k - 1
            window.push(nums[i]);
        } else { // 窗口向前滑动
            window.push(nums[i]);
            res.push_back(window.max());
            window.pop(nums[i - k + 1]);
        }
    }
    return res;
}
```

不同于一般最大单调栈之处:

- 只需求滑窗k里面的最大的一个值出来，也就意味着求出最大的max之后，max之前的所有元素可以丢掉了.
- 仅在`滑窗大小 < k`时，元素才需push入栈。

  



## References

https://leetcode-cn.com/problems/sliding-window-maximum/solution/dan-diao-dui-lie-by-labuladong/

