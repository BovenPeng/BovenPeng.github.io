---
title: 内存不够时，替代sns.countplot()的实现方法
date: 2019-06-24 15:33:13
categories:
- EDA
tags:
- code snippet
- data visualization
- matplotlib
- seaborn
---

# 内存不够时，替代sns.countplot()的实现方法

最近做EDA的的时候会存在这样一种情况，**数据量过大**时，直接使用`sns.countplot()`内存很容易直接被榨干掉。

暂时没想清为什么会直接占用那么多内存，应该去StackOverflow提个Q的。

不过想了下又查了下StackOverflow，应该有用`df.value_counts() 和` `matplotlib`来实现`sns.countplot()`的实现方法，如此，[What is Matplotlib's alternative for countplot from seaborn?](https://stackoverflow.com/questions/55667185/what-is-matplotlibs-alternative-for-countplot-from-seaborn) 



引用自StackOverflow，[ImportanceOfBeingErnest](https://stackoverflow.com/users/4124317/importanceofbeingernest)

Say you have this data:

```python
import numpy as np; np.random.seed(42)
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame({"Sex" : np.random.choice(["male",  "female"], size=1310, p=[.65, .35]),
                   "other" : np.random.randint(0,80, size=1310)})
```

You can plot a countplot in seaborn as

```python
import seaborn as sns
sns.countplot(x="Sex", data=df)
plt.show()
```

{% asset_img countplot.jpg sns.countplot %}

Or you can create a bar plot in pandas

```python
df["Sex"].value_counts().plot.bar()
plt.show()
```

{% asset_img plt_bar.jpg df["Sex"].value_counts().plot.bar()%}

Or you can create a bar plot in matplotlib

```python
counts = df["Sex"].value_counts()
plt.bar(counts.index, counts.values)
plt.show()
```

{% asset_img value_counts_plot_bar.jpg plt.bar(df["Sex"].value_counts().index, df["Sex"].value_counts().values)%}



# Reference

- [What is Matplotlib's alternative for countplot from seaborn?](https://stackoverflow.com/questions/55667185/what-is-matplotlibs-alternative-for-countplot-from-seaborn)
- [Pandas:Visualization Guide](https://pandas.pydata.org/pandas-docs/stable/user_guide/visualization.html)

