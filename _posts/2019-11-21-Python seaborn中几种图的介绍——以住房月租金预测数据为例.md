---
layout:     post
title:      "Python seaborn中几种图的介绍——以住房月租金预测数据为例"
date:       2019-11-21
author:     "宋佳"
tags:
    - python可视化
    - seaborn
    - 数据分析
---
# 画所有图都需要做的事
**引入一些包**

``` python
    import pandas as pd
    pd.plotting.register_matplotlib_converters()
    #用matplotlib注册pandas格式化器和转换器
    import matplotlib.pyplot as plt
    %matplotlib inline
    #不用输入plt.show()就能将图像自动显示
    import seaborn as sns
```
**加载要用的数据**   

``` python
    train_filepath = "D:/python_work/data/train.csv"
    test_filepath = "D:/python_work/data/test.csv"
    #指明数据集的位置
    train_data = pd.read_csv(train_filepath,index_col="时间")
    test_data = pd.read_csv(test_filepath,index_col="id")
    #提供数据集的文件路径
```
**预览以下一部分/全部数据**

``` pyhton
    train_data.head()
    #打印前五行数据
    train.tail()
    #打印后五行数据
```
# 相关性与因果性的区别
## 描述
![xgyg](/Blog-Share/img/1911/11/Chasel/xgyg.png)
B与C有相关性，但没有因果性
*    A发生时，B发生的概率很大，AB有相关性
*    A发生时，B一定发生，AB有因果性
##科学如何验证相关性与因果性
*    做出假设
*    收集数据。
采集到的数据显示，A与B的同时发生有统计上的显著性
*    更新认知模型
>人类对客观世界事或物的认知是通过回答三个问题“What”、“How”和“Why”来完成的，即什么、怎么、为什么，简称3M认知模型。“是什么”是关于事物本质的问题，事物的本质是该事物区别于它事物的内在规定性。要说清它，就不能不分析它的性质、属性、特征、表现形式等与本质直接相关的各种问题。“怎么”则为人们“应该怎么做”而提供的指导。“为什么”是对事物问题发生原因的探讨。任何事物的存在或现象的出现都不可能没有原因，只有正确认识事物发生的原因，认清其因果联系，才能真正认识此项事物。
*    总结
现有的认知模型对现在现象的描述很好，但还不够好，所以我们朝着更上游探索。   
这需要收集足够多的数据，形成对现象的解释。在这个过程中，我们也达到了拓展知识边界，提升对世界理解水平的作用。
# 图
## 表示趋势
### 折线图

``` python
    plt.figure(figsize=(16,6))
    sns.lineplot(data=test_data['房屋面积'],label="house_s")
    plt.xlabel("楼层")
```
![line_s](/Blog-Share/img/1911/03/Chasel/line_s.png)
可以看出楼层数为1的住房面积最大

``` python
    plt.figure(figsize=(16,6))
    sns.lineplot(data=test_data['卧室数量'],label="bedroom")
    sns.lineplot(data=test_data['厅的数量'],label="hall")
    sns.lineplot(data=test_data['卫的数量'],label="wc")
    plt.xlabel("楼层")
```
![line_3](/Blog-Share/img/1911/03/Chasel/line_3.png)
可以看出楼层数为1的住房卧室、厅、卫的数量都最大，与面积最大相吻合。
## 表示关系
### 热度图

``` python

```
### 散点图

``` python
    sns.scatterplot(x=train_data['房屋面积'],y=train_data['月租金'])
```
![sandian](/Blog-Share/img/1911/03/Chasel/sandian.png)
可以看出虽然有几个异常值，但房屋面积与月租金有很大的相关性
去掉异常值之后再画出一条回归线
``` python
    train_data2 = train_data.drop(train_data[(train_data['房屋面积']>0.2) & (train_data['月租金']<20)].index)
    sns.scatterplot(x=train_data2['房屋面积'],y=train_data2['月租金'])
    sns.regplot(x=train_data2['房屋面积'],y=train_data2['月租金'])
```
### 柱状图

``` python
    plt.figure(figsize=(10,6))
    sns.barplot(x=train_data['区'],y=train_data['月租金'])
```
![bar1](/Blog-Share/img/1911/03/Chasel/bar1.png)
这张图可以表示不同小区间月租金的差别
## 表示分布
### 直方图

``` python
    train_data.hist(figsize=(20, 15), bins=50, grid=False)
    plt.show()
```
可以用hist函数观察各特征的分布情况
![bar2](/Blog-Share/img/1911/03/Chasel/bar2.png)
也可以观察单个列的分布，当我想观察地铁线路的分布时
``` python
    sns.distplot(a=train_data['地铁线路'],kde=False)
```
出现错误
```python
    ValueError: cannot convert float NaN to integer
```
因为数据中有空值，所以可以把空值转换为0，再观察其分布
### 密度图
密度图可以视为平滑的直方图

``` python
    sns.kdeplot(train_data['位置'],shade=True)
```
![kde](/Blog-Share/img/1911/03/Chasel/kde.png)
可以观察单列，如位置的分布情况
# 总结（困难）
*    pandas包时可以用时不可以用，于是重装了anaconda
*    不会正确删除数据，导致不会画热度图、回归线和单个直方图
*    不会在博客里插入图片
*    太多了 My vegetable has exploded