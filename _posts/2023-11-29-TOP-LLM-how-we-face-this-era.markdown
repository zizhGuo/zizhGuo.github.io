---
layout: post
# title:  "QuestBee开源自动化beeline cmd生成工具 QuestBee Beeline CMD Genaration Tool （updating）"
title:  "个体在大语言模型时代的正确行走姿态是什么 How should an individual respond to the age of LLMs"
date:   2023-11-29 09:33:30
author: Zizhun Guo
authorCN: 郭子谆
category: Article
categories: jekyll update
visible: 1
porject: 0
top: 1
---

<div style="text-align: center;">
    <a href ="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/llms-discussion/llm.png">
   <img src="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/llms-discussion/llm.png" alt="drawing" style="width: 100%;"/>
   </a>
   <figcaption>本插画为DALLE生成，提示词为ChatGPT生成</figcaption>
</div>

> 我对ChatGPT提出的问题：个体在大语言模型时代的正确行走姿态是什么 How should an individual respond to the age of LLMs，这是一个讨论关于大预言模型对人类生活产生影响和讨论人类应该如何应对的博客标题，请根据标题生成一个博客背景插画

> ChatGPT回复我的提示词：The image will feature a person standing at a crossroads, symbolizing decision-making in the age of LLMs. The paths will represent different approaches to embracing technology – one leading towards a future integrated with AI, another towards maintaining traditional human values, and a middle path symbolizing balance. The backdrop will feature a skyline blending modern digital elements with classic architectural styles, symbolizing the fusion of technology and tradition. The individual will appear thoughtful and prepared, embodying a balanced and informed approach to the era of LLMs. The overall tone will be contemplative and futuristic.

<br>

正文：

我发现有很多博客，写作思路都比较统一和呆板。在谈论到使用大预言模型的时候，或是为了科普工具的使用而着重详解提示词的组成细节，或是为了开拓思路而不停地头脑风暴，但其实作为单一的细节描述提供方，其大部分内容都重复且在网络上有迹可循，而或是作为开拓思路的头脑风暴来讲，很多时候都是作者在自己的思维框架内进行的，为了罗列各种可能性。有技术含量的内容则更多聚焦于利用API开发套壳软件，或者是直接详细讨论模型的构建细节，和训练优化的技术，而这些内容对于普通用户来讲，可能并不是那么有用。因此，我想尝试一下，用一种更加简单的方式，来讲述我对于大预言模型的理解和一些讨论。

先说我对大模型的理解，基本的还是构建在基于黑盒神经网络，将所有数据都作为输入，然后输出一个预测结果。这个预测结果可以是一个单词，也可以是一个句子，也可以是一个段落这种文档形式，也可以是其他类目的内容如图片，音频，视频等。其基本理念就是端到端生产内容，人类只需要告诉模型“我要什么”，模型就能直接输出“内容”。这就好比，我们告诉一个人“我要一杯咖啡”，他就能给我们做一杯咖啡，而不需要我们告诉他“咖啡是怎么做的”。这就是大模型的基本理念。因为这中间的所有过程都被黑盒化了，相当于将复杂的生产过程压缩进了一整套模型中，而又因为任务的复杂，所以模型需要更多的参数去拟合，当参数量上升到几十亿上百亿时，已经不是普通几个家用显卡可以有效训练的时候，我们就觉得这个模型相当大了。

试想一下，这是多么美妙的事情，我们大多数人在学习和工作中遇到需要解决的问题时，都是需要我们通过脑力或体力输入我们的思想，然后产出内容和结果，而这中间会占用我们大量的体力精力和时间，而且很多时候我们的思想并不是那么清晰，我们需要不断地修改调整，而这个过程往往是非常痛苦的。而如果我们能够通过一个简单的输入，就能直接准确得到我们想要的内容，那么我们就可以将更多的精力和时间放在思考上，而不是在生产上。这就是大模型的魅力所在。

话虽如此，但还存在一个老生常谈的东西，我们是否将因为大模型的泛滥而变得愚蠢，又或者大模型本身是基于已存在的知识和内容所结合，我们得到的东西是否具备创新性呢，我们能否保护好我们的私人创意和版权归属；如果涉及到隐私问题，什么样的内容可以被用来训练，而什么不能，会不会我家的门牌号也被投喂进了大模型中，而某天大模型给其他人透露了我家的地址，诸如此类的问题涉及到社会方方面面，已经不是一个工具本身应负有的责任范围，甚至已经超出了其所能承担的责任极限。我们当然可以使用大模型帮助我们进行内容生产，但是谁将对内容的真实性，私密性，安全性负责，是不是需要有个度，这些衍生话题已经不再是新闻了，他们已经是各国政府和科技公司的头等大事了。

外力手段可以通过行政令方式予以限制，限制其商业用途同样也会限制其科技发展的进程，是否会对国家科技实力国际化竞争产生负面影响。AI本身会不会失去控制，如果服务器的读写权力都赋予给了AI，会不会有大模型运维岗位产生，会不会衍生出大模型“黑客”，AI会不会因触达的维度增加产生智能，小说电影中AI占领人类网络的情景会不会发生。这些都是非常有意思的点。对于普通用户来说，这一日可能还很远，对于AI创业者来说，AI已经近在咫尺。大模型技术已经在某些垂直领域得到广泛的使用，比如游戏行业的原画制作，素材生成，电影行业的视频剪辑，音频合成，文本生成，阮江行业的代码生成。本人因为职业缘故，已经开始长时间使用大模型帮助和支持个人的工作，产出效果也至少提升了几倍不止，而且大模型还充当了本人的私人老师，能够更快地系统性的接触新的领域，去学习其中的门道，所以能够亲历大模型的使用，直面感受大模型的能力的冲击，也是非常震撼且幸运的事情。

在我看来，大模型时代才是刚刚开始，大模型不能做的事情还很多很多，你不可能让大模型帮我们砌房子，帮我们送外卖，帮我们参加标准化考试，甚至帮残疾人过马路，很多需要多种设备，专业知识，控制权和责任制度等因素的工作，大模型是没有能力和实力去承担的，可能说如果在近未来因为物联网，芯片，数字化技术的普及商用，在云计算，边缘计算，智能设备的涵盖到人类生活各个角落，大模型模型的服务才能够在各种接口中穿梭，从一个设备“跃迁”至另一个设备中，当它在法理和伦理进步到空前的世界后，人类社会彻底接纳AI作为其可承担法律责任的自然实体时，也许它能够彻底接管人类社会在各项机能，作为基础设施彻底升级了人类社会。但是，世界依然有太多的问题没有得到解决，芯片制程问题，电池容量问题，以及其他如经济增速减缓，食品安全，民生医疗等问题，大模型，或者说AI是否都能够解决？当我们国际竞争和大国对抗当中砥砺前行时，新的技术能否去改善甚至是打破现有格局，都或将成为科技进步的阻力和助力。

虽然大模型现在还不会是万能灵药，但总归来说，科技已经快速地重塑社会的某些部分。作为个人，当汽车发明后，马车夫这个职业可能会慢慢消失了。想清楚为什么做，抓住本质，才能在快速变迁的时代中生存。

以后我会发布更多大模型的使用经验分享和案例，也算是在新时代做一个合格新司机的必经之路的修行。

喜欢我的博客可以收藏我的网站哦，愿世界和平。



---
Copyright @ 2021 Zizhun Guo. All Rights Reserved.

