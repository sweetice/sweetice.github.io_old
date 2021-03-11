---
title: 'How to plot performance figures in reinforcement learning papers'
date: 2019-04-19
permalink: /posts/2019/04/plot-performance-figures-in-rl/
tags:
  - Reinforcement Learning
  - skills
---


# 如何绘制出强化学习论文里面的曲线图

![论文里面好看的图](https://s2.ax1x.com/2019/04/10/ATk7lt.png)  

paper里面含有精美的图能够增加文章中的概率, 那么强化学习论文里面那种精美的图到底是怎么画出来的呢?  
答案是:使用seaborn绘制. 有人也提出来,可以使用origin绘制,但考虑到数据从python体系内转换到origin格式. 需要借助一些中介工具(比如excel), 这样子会增加难度.因此我们还是考虑使用seaborn来绘制.

seaborn风格的图片如下:

![seaborn](https://s2.ax1x.com/2019/04/10/ATABB8.png)

## 绘图需要用到的数据

在RL里面,我们用来画图的数据一般包含:算法名字(用来分类), 平均reward回报, 随机种子(用来解决), 以及step次数.

我们使用随机数据来生成这样的图片:
```
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

time = np.linspace(0,200,201).reshape(-1,1)
Algo = ['PPO', 'DDPG', 'TRPO', 'DDPG-SG']


all = pd.DataFrame([])
for seed in range(10):
    for algo in Algo:
        data = pd.DataFrame(np.ones((201, 4)))
        data.columns = ['step', 'algo', 'avg_reward', 'seed']
        data['step'] = time
        data['algo'] = algo
        data['avg_reward'] = time + np.random.randn(201,1)*10
        data['seed'] = seed
        all = pd.concat([all, data], 0)

sns.lineplot(x='step', y='avg_reward', data=all, hue='algo')
plt.show()
```

得到的效果:

![效果](https://s2.ax1x.com/2019/04/10/ATA6hj.png)


api详细信息请点[此处](https://seaborn.pydata.org/generated/seaborn.lineplot.html#seaborn.lineplot)

## API细节

lineplot带有大量的参数

```angular2html
seaborn.lineplot(x=None, y=None, hue=None, size=None, 
style=None, data=None, palette=None, hue_order=None, 
hue_norm=None, sizes=None, size_order=None, size_norm=None, 
dashes=True, markers=None, style_order=None, units=None, 
estimator='mean', ci=95, n_boot=1000, sort=True, 
err_style='band', err_kws=None, legend='brief', ax=None, **kwargs)¶
```

但是我们需要用到的参数就是那么几个

- x  
输入的横坐标,注意横坐标的取值不一定是唯一的.在RL中,我们输入的横坐标一般是step.
- y 
纵坐标, 我们的纵坐标一般是平均回报.
- hue  
按照什么标准来分类画图.一般以算法来区分.
-  ci   
置信区间的大小, 设置为"sd"时候,就会画出标准差范围.一般取95.
-error_style  
误差带的类型,可在band和bars之间选择.一般选择band.


