---
layout: post
title:  图绘破解SQL窗函数之道 Cracking the Illustrated SQL Window Function
date:   2022-12-21 21:26:20
author: Zizhun Guo
authorCN: 郭子谆
category: Projects
categories: jekyll update
visible: 1
project: 1
---
###### 目录
- [1. 机制介绍 Mechanism Introduction](#1-机制介绍-mechanism-introduction)
    - [1.1 Aggregation in dictionary](#11-aggregation-in-dictionary)
    - [1.2 图解分组聚合 Illustrated Group-by Aggregation](#12-图解分组聚合-illustrated-group-by-aggregation)
    - [1.3 图解窗函数聚合 Illustrated Window Function Aggregation](#13-图解窗函数聚合-illustrated-window-function-aggregation)
    - [1.4 分组聚合的局限性 The limitation of Group-by Aggregation](#14-分组聚合的局限性-the-limitation-of-group-by-aggregation)
    - [1.5 窗函数聚合的优势](#15-窗函数聚合的优势)



#### 1. 机制介绍 Mechanism Introduction

###### 1.1 Aggregation in dictionary

Merriam-Webster字典中，‘aggregation’的意思是‘**a group, body, or mass composed of many distinct parts or individuals**’，这里的意思非常抽象。首先解释一下分组，在SQL中，我们使用记录来字段来区分分组，也就是说不同记录在相同的字段的值时，可被分为相同的组；在相同的组内，我们计算一些共同的性质来代表这个组，就是聚合了。更具体一点，我们有一张表，有两个字段，一个是颜色，一个是数字，那么我们可以使用颜色作为分组的依据，那么所有红色的记录就是一个分组，所有绿色的记录就是一个分组，所有蓝色的记录就是一个分组，以此类推。那么在每个分组内，我们可以计算数字的平均值，那么这个平均值就是这个分组的聚合值。这就是分组聚合的过程。

###### 1.2 图解分组聚合 Illustrated Group-by Aggregation

<div style="text-align: center;">
<a href ="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/Cracking-the-SQL-window-function/1.png">
<img src="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/Cracking-the-SQL-window-function/1.png" alt="drawing" style="width: 100%;"/>
</a>
<figcaption> 图解分组聚合 </figcaption>
</div>

但实质上，见图中左边部分（右边已经讲过了）的抽象解答，我用小正方体表示一条记录，那么左图表示的是：用颜色作为立方体的分组依据，用聚合的立方体边长和数字来代表分组的共同特性。那么可以看出，左边三个小方块的颜色是一样的，那么他们就是一个分组，而他们的数字分别是3，4，5，那么他们的聚合值就是4（这里的聚合值选择是平均数），因此大红色的立方体的中间就是4，且边长是3（这里代表聚合的计数）。同理，绿色小方块分组的个数和平均值亦在大绿色立方体的属性中有所体现。这就是分组聚合的过程。而在结构表中中，这样的过程更容易理解，因为我们可以直接看到每一条记录的值，和他们聚合后的新字段的值。

###### 1.3 图解窗函数聚合 Illustrated Window Function Aggregation

<div style="text-align: center;">
<a href ="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/Cracking-the-SQL-window-function/2.png">
<img src="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/Cracking-the-SQL-window-function/2.png" alt="drawing" style="width: 100%;"/>
</a>
<figcaption> 图解窗函数聚合 </figcaption>
</div>

窗函数的区别，我使用了另一张图来解答，可以观察到，相比于分组聚合图中小方块转变为代表分组后的共有特征值，我们使用窗函数后，首先所有原始实体的所有属性均获得保留，这体现在红色和绿色的小方块的数量，颜色和数字都没有变化。但是，我们在窗函数中，使用了聚合函数，这里我使用了平均数和计数，那么我们可以看到，新的红色和绿色大方块的的数字增加了平均值，而且其边长也改变为对应原颜色的小方块的数量，这个数字和边长就是聚合函数的结果，也就是说，我们在窗函数中，使用聚合函数，但是不会丢失原始记录的信息，而是在原始记录的基础上，增加了聚合函数的结果。这就是窗函数和普通分组聚合的区别。

###### 1.4 分组聚合的局限性 The limitation of Group-by Aggregation

<div style="text-align: center;">
<a href ="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/Cracking-the-SQL-window-function/3.png">
<img src="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/Cracking-the-SQL-window-function/3.png" alt="drawing" style="width: 100%;"/>
</a>
<figcaption> 分组聚合示意图</figcaption>
</div>

1. 分组聚合压缩了表中记录的数据，而窗函数聚合记录数保持相同
2. 同一个Query（不做union，sub-query或者关联join等）中分组聚合针对同一种分组进行，难以组合不同分组维度的聚合值，而窗函数可以在统一额表中不通分组维度（不同的窗）进行，且组合得到不同分组维度的聚合值

###### 1.5 窗函数聚合的优势

<div style="text-align: center;">
<a href ="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/Cracking-the-SQL-window-function/4.png">
<img src="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/Cracking-the-SQL-window-function/4.png" alt="drawing" style="width: 100%;"/>
</a>
<figcaption> 窗函数聚合示意图 </figcaption>
</div>

窗函数聚合可以通过设置不同的窗（类似分组group by概念，在SQL中通过over clause语句中的partition by来实现），以实现不通分组维度的聚合值可以集成在同一张表的结果里。事实上，窗函数的执行也是在Select语句执行完成后，对结果集使用不同窗进行统计分析，后续我会提到，这里可以看图理解。

---Copyright @ 2021 Zizhun Guo. All Rights Reserved.