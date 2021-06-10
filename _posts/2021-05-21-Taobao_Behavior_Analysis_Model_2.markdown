---
layout: post
title:  "Alibaba Taobao User Behaviors Analysis: AARRR Framework (Activation/Retention) - DAU, Retention Rate, etc"
date:   2021-05-25 13:34:57
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---

<br>

---

<br>

#### [Editing]

#### 1. Recap of the user behavior table

```
+-------+-------+-----------+--------+----------+-------------------+----------+-----+---+----+---------+
|user_id|item_id|category_id|behavior|timestamps|           datetime|      date|month|day|hour|dayofweek|
+-------+-------+-----------+--------+----------+-------------------+----------+-----+---+----+---------+
|      1|2268318|    2520377|      pv|1511544070|2017-11-24 12:21:10|2017-11-24|   11| 24|  12|        6|
|      1|2333346|    2520771|      pv|1511561733|2017-11-24 17:15:33|2017-11-24|   11| 24|  17|        6|
|      1|2576651|     149192|      pv|1511572885|2017-11-24 20:21:25|2017-11-24|   11| 24|  20|        6|
|      1|3830808|    4181361|      pv|1511593493|2017-11-25 02:04:53|2017-11-25|   11| 25|   2|        7|
|      1|4365585|    2520377|      pv|1511596146|2017-11-25 02:49:06|2017-11-25|   11| 25|   2|        7|
|      1|4606018|    2735466|      pv|1511616481|2017-11-25 08:28:01|2017-11-25|   11| 25|   8|        7|
|      1| 230380|     411153|      pv|1511644942|2017-11-25 16:22:22|2017-11-25|   11| 25|  16|        7|
|      1|3827899|    2920476|      pv|1511713473|2017-11-26 11:24:33|2017-11-26|   11| 26|  11|        1|
|      1|3745169|    2891509|      pv|1511725471|2017-11-26 14:44:31|2017-11-26|   11| 26|  14|        1|
|      1|1531036|    2920476|      pv|1511733732|2017-11-26 17:02:12|2017-11-26|   11| 26|  17|        1|
+-------+-------+-----------+--------+----------+-------------------+----------+-----+---+----+---------+
only showing top 10 rows
```

|Behavior   | Explanation  |
|---|---|
|pv   | Page view of an item's detail page, equivalent to an item click  |
|fav   | Purchase an item  |
|cart   |Add an item to shopping cart   |
|buy   |Favor an item   |

#### 2. Activation

The Activation evaluates the Ecommerce's ability to provide users with the "Aha moment". It overlaps the concept with the acquisition a little, but the difference is that the activation focuses on micro-conversion part whereas users are having enjoyable and solid experience in individual part of product process. 

###### Daily Active behaviors distribution

Details aside, first look at the distribution for the number of daily behaviors between 2017-11-24 to 2017-12-03.

```py
df_date_behavior_count = spark.sql("""
SELECT 
    date,
    SUM(CASE WHEN behavior = 'pv' THEN 1 ELSE 0 END) AS pv,
    SUM(CASE WHEN behavior = 'fav' THEN 1 ELSE 0 END) AS fav,
    SUM(CASE WHEN behavior = 'cart' THEN 1 ELSE 0 END) AS cart,
    SUM(CASE WHEN behavior = 'buy' THEN 1 ELSE 0 END) AS buy
FROM 
    taobao
GROUP BY 
    date
ORDER BY date
""").toPandas()

print(df_date_behavior_count)
```
<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Model_2/DAB.png">
   <img src="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Model_2/DAB.png" alt="drawing" style="width: 60%;"/>
   </a>
   <figcaption>Fig 1: Daily Active behaviors histogram </figcaption>
</div>

###### Daily Active Users(DAU) distribution

```py
df_DAU = spark.sql("""
SELECT 
    date,
    COUNT(DISTINCT user_id) AS DAU
FROM 
    taobao
GROUP BY 
    date
ORDER BY 
    date
""").toPandas()

print(df_DAU)
```
<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Model_2/DAU.png">
   <img src="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Model_2/DAU.png" alt="drawing" style="width: 60%;"/>
   </a>
   <figcaption>Fig 2: Daily Active Users histogram </figcaption>
</div>

Compare 

###### Hourly Active Behaviors distribution

```py
df_hour_behavior_count = spark.sql("""
SELECT 
    hour,
    SUM(CASE WHEN behavior = 'pv' THEN 0.1 ELSE 0 END) AS pv,
    SUM(CASE WHEN behavior = 'fav' THEN 0.1 ELSE 0 END) AS fav,
    SUM(CASE WHEN behavior = 'cart' THEN 0.1 ELSE 0 END) AS cart,
    SUM(CASE WHEN behavior = 'buy' THEN 0.1 ELSE 0 END) AS buy
FROM 
    taobao
WHERE date < '2017-12-04' AND date > '2017-11-23'
GROUP BY 
    hour
ORDER BY 
    hour
""").toPandas()
```
<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Model_2/HAB.png">
   <img src="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Model_2/HAB.png" alt="drawing" style="width: 60%;"/>
   </a>
   <figcaption>Fig 3: Hourly Active Behaviors histogram </figcaption>
</div>

###### Hourly Active Users distribution

```py
df_AverageHAU = spark.sql("""
SELECT 
    hour,
    ROUND(COUNT(DISTINCT user_id)/10, 0) AS Average_HAU
FROM 
    taobao
WHERE date < '2017-12-04' AND date > '2017-11-23'
GROUP BY 
    hour
ORDER BY hour
""").toPandas()
```

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Model_2/HAU.png">
   <img src="{{site.url}}/assets/2021-05-21-Taobao_Behavior_Analysis_Model_2/HAU.png" alt="drawing" style="width: 60%;"/>
   </a>
   <figcaption>Fig 4: Hourly Active Users histogram </figcaption>
</div>

#### 2. Retention

---
Copyright @ 2021 Zizhun Guo. All Rights Reserved.

