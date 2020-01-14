---
title: leetcode题解-901-股票价格跨度
mathjax: true
date: 2020-01-07 21:19:38
categories:
tags:
- 单调栈
---

## [题目](https://leetcode-cn.com/problems/online-stock-span/)

#### 单调递增最大栈

```C++
class StockSpanner {
private:
    stack<int> prices, cache;

public:
    int next(int price) 
    {
        int w = 1;
        while (!prices.empty() && prices.top() <= price)
        {
            w += cache.top();
            prices.pop();
            cache.pop();
        }
        
        prices.push(price);
        cache.push(w);
        return w;
    }
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
```



单调栈模板的伪代码:

```java
Stack stack = new Stack();

for (遍历这个数组)
{
	while (栈不为空 && 栈顶元素小于当前元素)
	{
		栈顶元素出栈;
		对这个出栈值做a操作;
	}
	入栈当前数据;
}

while (栈不为空)
{
	栈顶元素出栈;
	对这个出栈值做b操作;
}
```

问题在于，因为单调栈是要把元素都丢弃的，状态都被“折叠”了，我们会丢失长度，所以容易想到，我们需要cache一下之前栈内元素被折叠的长度
cache有很多种方式，可以用hash表等数据结构，也可以用动态规划
但这题的更优解是，使用另一个同步栈来缓存，读者可以根据下面第一版的代码，动笔推导一下折叠过程，就能体会同步栈工作的原理了。

都说编程旷世难题是取名字，名字取好了，问题也就解决了一半（才怪
我们可以将两个栈分别命名为 prices 和 cache

然后就有了下面初版的代码

我们如果发现插入元素满足本身栈的递减需求，则直接返回1，因为该值前一个值是比它大的
如果不满足，则开始折叠，并将栈中，值比它小的所有段落都累计起来，再将自己插入栈中即可



## 总结

- 单调栈的题目复杂化方法
  - 总会在 **对出栈值做a操作** 和 **对出栈值做b操作** 这2个地方进行一些复杂化
  - 会在 **出栈值出栈的存储方式/处理方式** 进行一些复杂化的操作 

## Reference

https://leetcode-cn.com/problems/online-stock-span/solution/gu-piao-jie-ge-kua-du-by-leetcode/

