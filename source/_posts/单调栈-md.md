---
title: 单调栈.md
mathjax: true
date: 2020-01-07 20:34:57
categories:
- 总结
tags:
- 总结
---

## 单调栈

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



## Reference

https://leetcode-cn.com/problems/online-stock-span/solution/dan-diao-zhan-tao-lu-xie-fa-you-hua-wei-guan-fang-/

