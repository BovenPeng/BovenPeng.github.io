---
title: 以DataFrame的形式按序输出关联矩阵(correlation matrix)
date: 2019-04-23 20:45:53
categories:
- EDA
tags:
- code snippet
- EDA
- pandas
- data disualization
---

# 以DataFrame的形式按序输出关联矩阵(correlation matrix)

一般来说在做EDA的时候，我们都是输出相关矩阵的热力图或者是以表格形式输出。

但是我在Stantar 2019的一个[kernel](<https://www.kaggle.com/artgor/santander-eda-fe-fs-and-models>)见到了按序输出关联矩阵系数的代码，故在此基础上进行了一定修改，实现了一个自己的code snippet.

具体例子见下：(此处数据采用[Titanic in Kaggle](<https://www.kaggle.com/c/titanic>))

```python
train_corr = train_df.drop(['PassengerId'], axis=1).corr()
train_corr
```



```python
# 设置热力图尺寸为(20, 12)
plt.subplots(figsize=(20, 12))
sns.heatmap(train_corr, vmin=-1, vmax=1, annot=True, square=True)
```

{% asset_img heatmap.jpg heatmap %}

```python
corr = train_corr.abs().unstack().sort_values(kind='quicksort', ascending=False).reset_index()
corr = corr[(corr['level_0'] != corr['level_1']) & (corr['level_0'] == 'Survived')]
corr
```

{% asset_img correlation_matrix_HD.jpg correlation_matrx %}

一般是设置`ascending`参数，来看你是想要按序输出高或低相关度.

这里feature数量比较少，故直接输出，一般是`corr`调用`tail()`或者`head()`来输出该顺序中的后或前几个相关系数。例如：

```python
corr.tail(4)
```



下一篇会在再讲下，如何将`Pandas.DataFrame`的表格存储为图片，这也是在写这篇blog过程中的一点idea的实现.



# Reference

[Titanic in Kaggle](<https://www.kaggle.com/c/titanic>)

[按序输出correlation matrix出处](<https://www.kaggle.com/artgor/santander-eda-fe-fs-and-models>)

[Pandas.DataFrame.unstack](<https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.unstack.html>)

[Pandas.DataFrame.reset_index](<https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.reset_index.html>)

