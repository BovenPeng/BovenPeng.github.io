---
title: 如何指定xtick在一个特定的范围
date: 2019-06-24 15:24:14
categories:
- EDA
tags:
- code snippet
- EDA
- pandas
- data disualization
---

在做EDA的时候，会有种情况，时序数据所做的`plot`的x-axis会存在过于密集的情况，如下

{% asset_img high_frequency_x_axis_plot.png high_frequency_x_axis_plot %}

所以我们要做的就是只用将数据所在范围按一定频率切分开就行了，如这个Q，[how to change the xticks to a specific range \[duplicate\\]](https://stackoverflow.com/questions/56713197/how-to-change-the-xticks-to-a-specific-range) 和 [Changing the “tick frequency” on x or y axis in matplotlib?](https://stackoverflow.com/questions/12608788/changing-the-tick-frequency-on-x-or-y-axis-in-matplotlib)



分别引用自StackOverflow用户 [Heike](https://stackoverflow.com/users/840947/heike)和[Alexandre B.](https://stackoverflow.com/users/10041823/alexandre-b)并作出了一定修改。

在comment和answer处提供了两种解决方案：

- 添加一行代码`plt.xticks(range(9, 40, 10), range(10, 41, 10))`

  这里第一个参数是是将xtick分块显示，第二个参数是各个分块处显示的数字.(这里原来的数据找不到了，就用了answer中随机生成的数据为例子).

  例子:

  ```python
  # Your data to count
  y = np.random.randint(0,41,1000)
  
  # Create plot
  fig, ax = plt.subplots()
  sns.countplot(y)
  
  # Show graph
  plt.xticks(range(0, 41, 10), range(0, 41, 10))
  plt.xlabel('user_id')
  plt.show()
  ```

  {% asset_img set_xticks_plot.png plot_with_setting_xticks%}

  但是假如将第一个参数设置的分块范围超过数据已有的范围，则会在plot超出数据已有范围处显示出一段blank。

  {% asset_img plot_with_blank.png plot_with_blank%}



- One way is to define the labels on the x-axis. The `set_xticklabels` method from `matplotlib`module do the job [(doc)](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.set_xticklabels.html). By defining your own labels, you can hide them by setting the label equal to `''`.

  By defining your own labels, you **need to take care** that they are still **consistent with your data**.

  Here is one example:

  ```python
  # import modules
  import numpy as np
  import seaborn as sns
  import matplotlib.pyplot as plt
  
  #Init seaborn
  sns.set()
  
  # Your data to count
  y = np.random.randint(0,41,1000)
  
  # Create the new x-axis labels 
  x_labels = ['' if i%10 != 0 else str(i) for i in range(len(np.unique(y)))]
  print(x_labels)
  # ['0', '', '', '', '', '', '', '', '', '', 
  # '10', '', '', '', '', '', '', '', '', '', 
  # '20', '', '', '', '', '', '', '', '', '', 
  # '30', '', '', '', '', '', '', '', '', '', '40']
  
  # Create plot
  fig, ax = plt.subplots()
  sns.countplot(y)
  
  # Set the new x axis labels
  ax.set_xticklabels(x_labels)
  # Show graph
  plt.show()
  ```

  {% asset_img define_label_plot.png defined_own_label_plot %}

  

# Reference

- [how to change the xticks to a specific range \[duplicate\\]](https://stackoverflow.com/questions/56713197/how-to-change-the-xticks-to-a-specific-range) 
- [Changing the “tick frequency” on x or y axis in matplotlib?](https://stackoverflow.com/questions/12608788/changing-the-tick-frequency-on-x-or-y-axis-in-matplotlib)
- [matplotlib.axes.Axes.set_xticklabels](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.set_xticklabels.html)

