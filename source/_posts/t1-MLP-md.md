---
title: t1_MLP
mathjax: true
date: 2020-02-14 21:27:58
categories:
- 动手深度学习
tags:
---

## MLP

多层感知机就是含有至少一个隐藏层的由全连接层组成的神经网络，且每个隐藏层的输出通过激活函数进行变换。多层感知机的层数和各隐藏层中隐藏单元个数都是超参数。

### 定义模型

```python
num_inputs, num_outputs, num_hiddens = 784, 10, 256

net = nn.Sequential(
        d2l.FlattenLayer(),
        nn.Linear(num_inputs, num_hiddens),
        nn.ReLU(),
        nn.Linear(num_hiddens, num_outputs),
        )

for params in net.parameters():
	init.normal_(params, mean=0, std=0.01)
```



## Reference

[神经网络快速入门：什么是多层感知器和反向传播？ - 机器之心的文章 - 知乎](https://zhuanlan.zhihu.com/p/23937778)