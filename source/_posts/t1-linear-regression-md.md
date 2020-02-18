---
title: t1_linear_regression
mathjax: true
date: 2020-02-14 21:26:12
categories:
- 动手深度学习
tags:
---

## 线性回归

### 表达式表示

$$
\hat{y} = Xw + b \\
其中模型输出 \hat{y} \in R^{n+1}，批量数据样本特征X\in R^{n  d}, 权重w \in R^{d*1}, 偏差b\in R.\\
$$

$$
批量数据样本标签 y \in R^{n *1}  。设模型参数 \theta = [w_1, w_2, b]^T  ，我们可以重写损失函数为:\\
l(\theta) = \frac{1}{2n}(\hat{y} - y)^T(\hat{y} - y)\\
$$

$$
小批量量随机梯度下降的迭代步骤将相应地改写为:\\
\theta \leftarrow \theta - \frac{\eta}{|\Beta|}\sum_{i \in \Beta} \nabla_\theta l^{(i)}(\theta)
$$



### 定义模型

#### 继承`nn.Module`

```python
class LinearNet(nn.Module):
	def __init__(self, n_feature):
		super(LinearNet, self).__init__()
		self.linear = nn.Linear(n_feature, 1)
	# forward 定义前向传播
	def forward(self, x):
		y = self.linear(x)
		return y
    
net = LinearNet(num_inputs)
print(net) # 使⽤用print可以打印出⽹网络的结构
```



```python
# output
LinearNet(
(linear): Linear(in_features=2, out_features=1, bias=True)
)
```



### 用`nn.sequential`将网络层按照传入`sequential`的顺序加入到计算图中

```python
# 写法⼀一
net = nn.Sequential(
	nn.Linear(num_inputs, 1)
# 此处还可以传⼊入其他层
)
# 写法⼆二
net = nn.Sequential()
net.add_module('linear', nn.Linear(num_inputs, 1))
# net.add_module ......
# 写法三
from collections import OrderedDict
net = nn.Sequential(OrderedDict([
		('linear', nn.Linear(num_inputs, 1))
		# ......
	]))
print(net)
print(net[0])
```



```python
# output
Sequential(
(linear): Linear(in_features=2, out_features=1, bias=True)
)
Linear(in_features=2, out_features=1, bias=True)
```

### 初始化模型参数

```python
from torch.nn import init
init.normal_(net[0].weight, mean=0, std=0.01)
init.constant_(net[0].bias, val=0) # 也可以直接修改bias的data:
net[0].bias.data.fill_(0)
```



### 定义损失函数

```python
loss = nn.MSELoss()
```



### 定义优化算法

```python
import torch.optim as optim
optimizer = optim.SGD(net.parameters(), lr=0.03)
print(optimizer)
```



```python
#output
SGD (
    Parameter Group 0
    dampening: 0
    lr: 0.03
    momentum: 0
    nesterov: False
    weight_decay: 0
)
```

#### 调整学习率

##### 1. 为不同子网络调整学习率，常用于finetuning时

```python
optimizer =optim.SGD([
                    # 如果对某个参数不不指定学习率，就使⽤用最外层的默认学习率
                    {'params': net.subnet1.parameters()}, # lr=0.03
                    {'params': net.subnet2.parameters(), 'lr': 0.01}
                ], lr=0.03)

```



##### 2. 修改`optimizer.param_groups`中的学习率

```python
# 调整学习率
for param_group in optimizer.param_groups:
param_group['lr'] *= 0.1 # 学习率为之前的0.1倍

```



##### 3. 新建优化器

由于optimizer十分轻量级，构建开销很小，故而可以构建新的optimizer。但是后者对于使用动量的优化器器（如Adam），会丢失动量等状态信息，可能会造成损失函数的收敛出现震荡等情况。

### 小结

- `pytorch`各个模块的作用:
  - `torch.utils.data `模块提供了了有关数据处理理的⼯工具， 
  - `torch.nn `模块定义了了⼤大量量神经⽹网络的层，
  - `torch.nn.init`模块定义了了各种初始化方法，
  - `torch.optim` 模块提供了了模型参数初始化的各种方法



## References

[latex 导数相关符号](<https://www.jianshu.com/p/8aa646fad1c5>)

[最小二乘法*线性回归*：矩阵视角](https://zhuanlan.zhihu.com/p/33899560)



## API

[torch.utils.data.dataloader](https://pytorch.org/docs/stable/_modules/torch/utils/data/dataloader.html)

[PyTorch: How to use DataLoaders for custom Datasets](https://stackoverflow.com/questions/41924453/pytorch-how-to-use-dataloaders-for-custom-datasets)