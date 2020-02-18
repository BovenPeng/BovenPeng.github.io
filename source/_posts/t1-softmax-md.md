---
title: t1_softmax
mathjax: true
date: 2020-02-14 21:27:42
categories:
- 动手深度学习
tags:
---

## softmax

搜了几篇文章看了，和手推了`softmax`, 交叉熵公式。

[这篇文章](https://zhuanlan.zhihu.com/p/62919791)给了一个为什么要用`softmax` + `cross entropy`的解释，

因为在反向传播时传递的同样是预测值与label值的偏差，即 $y_i - t_i$ .

与均方误差在反向传播时传递的是预测值与label值的偏差一样。

### 将模型training过程封装起来

```python
num_epochs, lr = 5, 0.1
# 本函数已保存在d2lzh包中⽅方便便以后使⽤用
def train_ch3(net, train_iter, test_iter, loss, num_epochs,
batch_size, params=None, lr=None, optimizer=None):
    for epoch in range(num_epochs):
    	train_l_sum, train_acc_sum, n = 0.0, 0.0, 0
    for X, y in train_iter:
    	y_hat = net(X)
    l = loss(y_hat, y).sum()
    # 梯度清零
    if optimizer is not None:
    	optimizer.zero_grad()
    elif params is not None and params[0].grad is not None:
        for param in params:
       		param.grad.data.zero_()
    l.backward()
    if optimizer is None:
    	d2l.sgd(params, lr, batch_size)
    else:
    	optimizer.step() 
    	
    	
        train_l_sum += l.item()
        train_acc_sum += (y_hat.argmax(dim=1) ==
        y).sum().item()
        n += y.shape[0]
    test_acc = evaluate_accuracy(test_iter, net)
    print('epoch %d, loss %.4f, train acc %.3f, test acc %.3f'
    % (epoch + 1, train_l_sum / n, train_acc_sum / n,
    test_acc))

train_ch3(net, train_iter, test_iter, cross_entropy, num_epochs,
batch_size, [W, b], lr)
```





## References

[softmax和cross-entropy是什么关系？ - 董鑫的回答 - 知乎](https://www.zhihu.com/question/294679135/answer/885285177)

[Softmax函数与交叉熵 - 杀手XIII的文章 - 知乎](https://zhuanlan.zhihu.com/p/27223959)

[为什么softmax搭配cross entropy是解决分类问题的通用方案？ - 教练我想学挖掘机的文章 - 知乎](https://zhuanlan.zhihu.com/p/62919791)

