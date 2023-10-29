---
layout: post
title:  Copilot-如何使用AI代码生产助手 Part1 Copilot helps secure the coding journey
date:   2023-10-09 21:26:20
author: Zizhun Guo
authorCN: 郭子谆
category: Projects
categories: jekyll update
visible: 1
project: 1
---
###### 目录
- [背景](#背景)
- [影响](#影响)
- [功能总结](#功能总结)
- [案例](#案例)
    - [无提示词non-prompting completion](#无提示词non-prompting-completion)
    - [有提示词prompting completion](#有提示词prompting-completion)
    - [代码转译 Code convertion](#代码转译-code-convertion)
    - [代码纠错 Code debugging](#代码纠错-code-debugging)



#### 背景   
Copilot作为一个基于chatgpt语言应用的代码编写辅助工具，从2022年中开始收费到现在已经推出一段时间了，在此期间，人们使用它的反响不一，尽管Copilot支持不多的IDE，比如Visual Studio Code还有JetBrain，其功能已经可以嵌入到我日常的大部分的代码编写工作场景中。语言模型最显著的作用，顾名思义，就是应用于语言中，正如ChatGPT对于英文翻译工作挑战性极大（直接导致了我取消订阅了Grammarly），对于人类发明的代码语言，基于GitHub大量代码库的自然语言模型的Copilot也能在代码编写的工作上实现多样且有效的辅助能力。

#### 影响

过去人们在编程工作中，一个良性的工作状态是，一半依赖于过去的编程经验和记忆所形成的习惯性解决方案，一半是依赖于有限的信息平台（官方文档和搜索引擎），其实这样对于上古程序员已然全套技术工具。在稍远一点时期，在搜索引擎出现和技术社区生态蓬勃发展，前人留下的问题及解决方案，和互联网即时性的沟通方式，更加充实了软件开发的生产需求。至少，在大语言模型出现之前，彼时的编程助手还是基于if-else和正则表达式的傻瓜式的鸡肋产品（更像是一个游戏的新手教程，能做的事情极其有限）。

但在现如今，开源工具遍地，新工具层出不穷，新的语言/新的轮子生态蓬勃发展的时候，一个精力有限的人类个体在各类问题上很难都做到迅速定位问题解法，在信息分散的时代（或者说混乱），升级版的搜索引擎是很有必要的，ChatGPT就是该时代的产物，他的出现极大程度上改善甚至进化了程序开发者的生产方式。因为程序开发所工作的场景就是虚拟世界，解法可以迅速被测试反馈，其成本是远小于使用ChatGPT的回答应用到现实世界的情况，况且其模型训练用的数据到底有多少其它类目（抛开掉技术或者互联网话题），这是完全待商榷的，不管怎么说，至少使用电子设备工作的人中，ChatGPT的帮助会非常大。

Coplilet聚焦于代码编写，是否可以利用Copilot在平日的工作中增加效率，非常值得尝试。本文我会统一记录Copilot的使用记录总结，以方便个人或他人查阅。Copilot具备适用于多种编程语言的能力。在编程范式和通用性描述的既定历史遗留问题有非常高效的代码生成和补全能力，可以有效减免简单重复性大的代码编写工作，其中额外释放出的个体生产力可以被发挥在更具创造里的事情上（前提条件上，当前的商业模式允许员工这样做）。这就是它的意义所在，工具最终都是服务于操作的主体，而主体就是人，而人最大的价值在于适用大脑的创造力，毕竟人类能够主宰地球所依靠的就是脑力。

#### 功能总结
- 自动补全输入 Code completion
    - 无提示词 non-prompting 联想补全
    - 有提示词 prompting 联想补全
- 代码转译 Code convertion
    - 格式变更
    - 自然语言机器翻译
- 代码纠错 Code debugging
- 代码分析 Code analysis


#### 案例
###### 无提示词non-prompting completion
<div style="text-align: center;">
<a href ="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/202310-copilot-1//1.png">
<img src="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/202310-copilot-1//1.png" alt="drawing" style="width: 100%;"/>
</a>
<figcaption> 自动检测出我想import pandas数据分析包 </figcaption>
</div>

       
###### 有提示词prompting completion  


###### 代码转译 Code convertion

<div style="text-align: center;">
<a href ="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/202310-copilot-1//2.png">
<img src="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/202310-copilot-1//2.png" alt="drawing" style="width: 100%;"/>
</a>
<figcaption> 根据中文提示词机器翻译</figcaption>
</div>
        
<div style="text-align: center;">
<a href ="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/202310-copilot-1//3.png">
<img src="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/202310-copilot-1//3.png" alt="drawing" style="width: 100%;"/>
</a>
<figcaption> 根据英文提示词机器翻译</figcaption>
</div>


###### 代码纠错 Code debugging  
        
<div style="text-align: center;">
<a href ="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/202310-copilot-1//4.png">
<img src="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/202310-copilot-1//4.png" alt="drawing" style="width: 100%;"/>
</a>
<figcaption> 自动检测代码错误，并给出更正代码 </figcaption>
</div>


---Copyright @ 2021 Zizhun Guo. All Rights Reserved.