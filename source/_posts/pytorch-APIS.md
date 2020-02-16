---
title: pytorch_APIS
mathjax: true
date: 2020-02-16 18:02:39
categories:
- 动手深度学习
tags:
---

## 1. 梯度相关

因为`PyTorch`存在梯度累加机制。

好处：

- 当内存不够使用更大的`batchsize`时，可以使用多次计算较小的`batchsize`的梯度平均值来替代。

坏处：

- 每次都需要对梯度进行清零



[how are optimizer.step() and loss.backward() realted](https://discuss.pytorch.org/t/how-are-optimizer-step-and-loss-backward-related/7350)

## 2. optimizer related

优化器optimizer，是根据神经网络的反向传播梯度信息来更新网络的参数，以达到降低loss的作用。

所以要使optimizer有用，需要如下两点：

1. `optimizer`需要知道当前模型(神经网络)的参数；所以再训练之前，需要将对应模型的参数放入`optimizer`之中去。类似如下代码：

   ```python
   optimizer = Adam(model.parameter(), lr=train.lr)
   ```

2. `optimizer`需要知道反向传播的梯度信息。

   `optimizer`更新参数空间需要基于反向梯度，因此，当调用`optimizer.step()`的时候应当是`loss.backward()`的时候.

   ```python
   loss.backward()
   optimizer.step()
   ```



### `optimizer.step()` 和 `scheduler.step()`异同

- `optimizer.step()` 只放在`mini-batch`之中。而不是`epoch`训练中，这是因为现在的`mini-batch`训练模式是假定每一个训练集就只有`mini-batch`这样大，因此实际上可以将每一次`mini-batch`看做是一次训练，一次训练更新一次参数空间，因而`optimizer.step()`放在这里。

- `scheduler.step()`按照`Pytorch`的定义是用来更新优化器的学习率的，一般是按照`epoch`为单位进行更换，即多少个`epoch`后更换一次学习率，因而`scheduler.step()`放在`epoch`这个大循环下。

  `scheduler.step()`的参数`step_size`表示`scheduler.step()`每调用`step_size`次，对应的学习率就会按照策略调整一次。所以如果`scheduler.step()`是放在`mini-batch`里面，那么`step_size`指的是经过这么多次迭代，学习率改变一次。

[Pytorch optimizer.step() 和loss.backward()和scheduler.step()的关系与区别 ](https://blog.csdn.net/xiaoxifei/article/details/87797935)

## 3. Tensor related

#### scatter_(*input, dim, index, src*) → Tensor

将`src`中的所有值按照`index`确定的索引写入本tensor中。其中索引是根据给定的dimension，dim按照`gather()`描述的规则来确定。

注意，index的值必须是在_0_到_(self.size(dim)-1)_之间，

**参数：** - **input** (*Tensor*)-源tensor - **dim** (*int*)-索引的轴向 - **index** (*LongTensor*)-散射元素的索引指数 - **src** (*Tensor or float*)-散射的源元素

```python
>>> x = torch.rand(2, 5)
>>> x

 0.4319  0.6500  0.4080  0.8760  0.2355
 0.2609  0.4711  0.8486  0.8573  0.1029
[torch.FloatTensor of size 2x5]

>>> torch.zeros(3, 5).scatter_(0, torch.LongTensor([[0, 1, 2, 0, 0], [2, 0, 0, 1, 2]]), x)

 0.4319  0.4711  0.8486  0.8760  0.2355
 0.0000  0.6500  0.0000  0.8573  0.0000
 0.2609  0.0000  0.4080  0.0000  0.1029
[torch.FloatTensor of size 3x5]

>>> z = torch.zeros(2, 4).scatter_(1, torch.LongTensor([[2], [3]]), 1.23)
>>> z

 0.0000  0.0000  1.2300  0.0000
 0.0000  0.0000  0.0000  1.2300
[torch.FloatTensor of size 2x4]
```



## 4. Device related

### Pytorch 0.4.0代码与0.3.x上的变化。

```python
# 定义device，是否使用GPU，依据计算机配置自动会选择
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
#用.to(device)来决定模型使用GPU还是CPU
model = CNN().to(device)

# 训练
total_loss = 0
for input, target in train_loader:
	#不需要转化为Variable，直接用Tensors作为输入，用.to(device)来决定使用GPU还是CPU
    input, target = input.to(device), target.to(device)
    hidden = input.new_zeros(*h_shape)  
   
	# 获得loss值，也与老版本不同
    total_loss += loss.item()          

# 测试
with torch.no_grad():      # 测试时不会进行梯度计算，节约内存
    for input, target in test_loader:
```

[PyTorch 0.4 迁移指南（与之前版本编程上的不同点）](https://blog.csdn.net/sunqiande88/article/details/80172391)