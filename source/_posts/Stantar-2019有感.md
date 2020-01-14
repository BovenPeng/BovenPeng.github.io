---
title: Stantar 2019有感
date: 2019-04-22 14:17:54
categories:
- data competition
tags: 
- kaggle
- data competition
- 学习计划
---

# Stantar 2019有感

看了很多个top solution，尤其是[金牌区域的](<https://www.kaggle.com/c/santander-customer-transaction-prediction/discussion/88926#latest-518288>)。

给我感觉在这个比赛中的几个重要的**magic**有：

- frequency coding；在对于匿名数据或者categorical feature上，可以先试着这样做下，一般是会提升local CV之类；
- fake data；对于training set和testing set中的unique value的数量明显不同，让人怀疑testing set中的数据包含伪造数据。对于testing set中的伪造数据进行删除即可。[List of Fake Samples and Public/Private LB split](https://www.kaggle.com/yag320/list-of-fake-samples-and-public-private-lb-split)
- independent feature；利用feature间的相互独立性，在用LightGBM训练时，只对某一特征训练，故有200个lgb classifier再对预测结果进行简单的simple linear blending。[a single model using all features vstacked that is enough for a top20 on Private](<https://www.kaggle.com/titericz/giba-single-model-public-0-9245-private-0-9234>)

感受颇深的是对于这些**magic**，一是自己确实没有通过EDA来发现到，能力有限；二是其实在kernel和discussion中都有多多少少提及到，自己未引起重视又或者不知如何是好。

个人主要感觉还是：

1. 在EDA方面考虑得不够到位，而且可以利用：
   1. Local CV和LB的提升上差异来分析是哪里的问题，然后解决并使得Local CV和LB的提升近似。比如此次中，一开始无论local CV提升有多高，LB都会锁在9.001上。而解决到fake data的存在后，local CV提升，LB也会随之提升，使得LB锁在9.001的情况不复存在；
   2. 可以根据kernel和discussion区域中，若在新的发布后，上的存在着许多人的大幅度增长，则说明某个kernel或discussion揭露了**magic**；
2. **Model**这一块也是有问题，很明显在NN上存在很多问题，尤其是现在去看top solution中开源的NN代码，会存在很多逻辑看不太懂的情况；
3. **Parameter Tuning**上面也有问题，除了最基本的**Grid Search**和**Random Search**外，对于**Bayesian optimization**没有理解且无法应用出来。[Chris这个discussion给了很多inspirations](https://www.kaggle.com/c/santander-customer-transaction-prediction/discussion/89320#517455)；
4. 对于Data Science这块Coding能力上也存在着问题，不能很流畅的阅读代码逻辑；
5. 这个比赛没有用到**Feature Selection**, 然而这块也是我的弱项；
6. 在最后如何选取2个**the most robust model**用于提交到PB上也仍未解决。

不过感觉还是收获了不少，在我上述几方面加强后，还是需要回来reiview Stantar这个比赛的各个solution。

# Reference

[Gold Medal Solutions of Stantar 2019](<https://www.kaggle.com/c/santander-customer-transaction-prediction/discussion/88926#latest-518288>)

[List of Fake Samples and Public/Private LB split](<https://www.kaggle.com/yag320/list-of-fake-samples-and-public-private-lb-split>)

[Giba: a single model using all features vstacked that is enough for a top20 on Private](<https://www.kaggle.com/titericz/giba-single-model-public-0-9245-private-0-9234>)

[Chirs: How do we optimize GBM hyperparameters?](<https://www.kaggle.com/c/santander-customer-transaction-prediction/discussion/89320#517455>)

