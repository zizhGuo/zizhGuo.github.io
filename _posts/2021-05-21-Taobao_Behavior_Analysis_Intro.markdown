---
layout: post
title:  "Alibaba Taobao User Behaviors Analysis: Dataset 1 billion records"
date:   2021-05-21 11:34:57
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---


**Alibaba Taobao.com**

> Taobao (Chinese: 淘宝网) is a Chinese online shopping platform. It is headquartered in Hangzhou and owned by Alibaba. It is ranked as the eighth most-visited website. Taobao.com was registered on April 21, 2003 by Alibaba Cloud Computing (Beijing) Co., Ltd.

> Taobao Marketplace facilitates consumer-to-consumer (C2C) retail by providing a platform for small businesses and individual entrepreneurs to open online stores that mainly cater to consumers in Chinese-speaking regions (Mainland China, Hong Kong, Macau and Taiwan) and abroad,[4] which is made payable by online accounts. Its stores usually offer an express delivery service.

> [https://en.wikipedia.org/wiki/Taobao](https://en.wikipedia.org/wiki/Taobao)

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Intro/AlibabaLogo.jpg">
   <img src="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Intro/AlibabaLogo.jpg" alt="drawing" style="width: 40%;"/>
   </a>
   <br>
    <a href ="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Intro/Taobao_Logo.svg">
   <img src="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Intro/Taobao_Logo.svg" alt="drawing" style="width: 40%;"/>
    </a>
   <figcaption>Alibaba Group LOGO </figcaption>
</div>

<br>

---

<br>

#### Dataset: User Behavior Data from Taobao for Recommendation

**Introduction**

The dataset is collected from [**Tianchi**](https://tianchi.aliyun.com/dataset/dataDetail?dataId=649&userId=1) - Data Science Workshop from Aliyun(阿里云)- literally means [**Alibaba Cloud**](https://us.alibabacloud.com/), the cloud computing service ranked **third-largest** infrastucture as a service provider, right behind Amazon Web Services, Microsoft Azure.

User Behavior is a dataset of user behaviors from Taobao, for recommendation problem with implicit feedback. The dataset is offered by Alibaba.

|File   |Description   |Feature   |
|---|---|---|
| UserBehavior.csv  |All user behavior data   | User ID, item ID, category ID, behavior type, timestamp  |

**UserBehavior.csv**

We random select about 1 million users who have behaviors including click, purchase, adding item to shopping cart and item favoring during November 25 to December 03, 2017. The dataset is organized in a very similar form to MovieLens-20M, i.e., each line represents a specific user-item interaction, which consists of user ID, item ID, item's category ID, behavior type and timestamp, separated by commas. The detailed descriptions of each field are as follows:

|Field   |Explanation   |
|---|---|
|User ID   |An integer, the serialized ID that represents a user   |
| Item ID  |An integer, the serialized ID that represents an item   |
|Category ID   |An integer, the serialized ID that represents the category which the corresponding item belongs to   |
|Behavior type   |A string, enum-type from ('pv', 'buy', 'cart', 'fav')   |
|Timestamp   |An integer, the timestamp of the behavior   |



Note that the dataset contains 4 different types of behaviors, they are

|Behavior   | Explanation  |
|---|---|
|pv   | Page view of an item's detail page, equivalent to an item click  |
|fav   | Purchase an item  |
|cart   |Add an item to shopping cart   |
|buy   |Favor an item   |

Dimensions of the dataset are

| Dimension  | 	Number  |
|---|---|
|# of users   |987,994   |
|# of items   |4,162,024   |
|# of categories   | 9,439  |
|# of interactions   |100,150,807   |


---
Copyright @ 2021 Zizhun Guo. All Rights Reserved.

