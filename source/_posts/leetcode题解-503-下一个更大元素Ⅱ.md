---
title: leetcode题解-503-下一个更大元素Ⅱ
mathjax: true
date: 2020-01-07 11:11:54
categories:
tags:
- 循环数组
- Next Great Number
---



### [labuladong way](https://leetcode-cn.com/problems/next-greater-element-ii/solution/dan-diao-zhan-jie-jue-next-greater-number-yi-lei-2/)

#### 不严格单调递增栈

```C++
vector<int> nextGreaterElements(vector<int>& nums) 
{
    int n = nums.size();
    vector<int> res(n); // 存放结果
    stack<int> s;
    // 假装这个数组长度翻倍了
    for (int i = 2 * n - 1; i >= 0; i--) {
        while (!s.empty() && s.top() <= nums[i % n])
            s.pop();
        res[i % n] = s.empty() ? -1 : s.top();
        s.push(nums[i % n]);
    }
    return res;
}
```



### Why?

- 为什么这里是用 **不严格的单调递增栈** 而不是 **严格的单调递增栈**
- 为什么通过**索引翻倍-1**后，是选择方向遍历元素的，而不是正向遍历？





## 总结

- 通过**索引翻倍-1** 和 **%**运算，来达到模拟数组中元素翻倍的效果。

  ```C++
  // 假装这个数组长度翻倍了
  for (int i = 2 * n - 1; i >= 0; i--) 
  {
  	while (!s.empty() && s.top() <= nums[i % n])
  		s.pop();
  	res[i % n] = s.empty() ? -1 : s.top();
  	s.push(nums[i % n]);
  }
  ```

- 同时，在元素出栈后，对栈空状态进行判断，

  - 若空了则，`res`对应值为`-1`
  - 若不空则，`res`对应值为`n`
  - 省去了一个存放`tuple`的容器的空间大小



## Reference

https://leetcode-cn.com/problems/next-greater-element-ii/solution/dan-diao-zhan-jie-jue-next-greater-number-yi-lei-2/