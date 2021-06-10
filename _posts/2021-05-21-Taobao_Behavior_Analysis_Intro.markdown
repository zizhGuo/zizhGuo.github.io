---
layout: post
title:  "Alibaba Taobao User Behaviors Analysis: Dataset(1 billion records) and Preprocessing"
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

#### 1. Dateset preprocessing

**Quick recap about the dataset**

|Field   |Explanation   |
|---|---|
|User ID   |An integer, the serialized ID that represents a user   |
| Item ID  |An integer, the serialized ID that represents an item   |
|Category ID   |An integer, the serialized ID that represents the category which the corresponding item belongs to   |
|Behavior type   |A string, enum-type from ('pv', 'buy', 'cart', 'fav')   |
|Timestamp   |An integer, the timestamp of the behavior   |

|Behavior   | Explanation  |
|---|---|
|pv   | Page view of an item's detail page, equivalent to an item click  |
|fav   | Purchase an item  |
|cart   |Add an item to shopping cart   |
|buy   |Favor an item   |

| Dimension  | 	Number  |
|---|---|
|# of users   |987,994   |
|# of items   |4,162,024   |
|# of categories   | 9,439  |
|# of interactions   |100,150,807   |

**Load the CSV dataset as Spark DateFrame using Pyspark**

```py
import findspark
findspark.init('/home/zizhun/spark-3.1.1-bin-hadoop2.7')
from pyspark.sql import SparkSession

# Load csv file into spark dataframe
df = spark.read.csv('UserBehavior.csv')

# Change field names
df = df.withColumnRenamed("_c0","user_id") \
        .withColumnRenamed("_c1","item_id") \
        .withColumnRenamed("_c2","category_id") \
        .withColumnRenamed("_c3","behavior") \
        .withColumnRenamed("_c4","timestamps")

```

**Check out the schema and partial view**
```
root
 |-- user_id: string (nullable = true)
 |-- item_id: string (nullable = true)
 |-- category_id: string (nullable = true)
 |-- behavior: string (nullable = true)
 |-- timestamps: string (nullable = true)

+-------+-------+-----------+--------+----------+
|user_id|item_id|category_id|behavior|timestamps|
+-------+-------+-----------+--------+----------+
|      1|2268318|    2520377|      pv|1511544070|
|      1|2333346|    2520771|      pv|1511561733|
|      1|2576651|     149192|      pv|1511572885|
|      1|3830808|    4181361|      pv|1511593493|
|      1|4365585|    2520377|      pv|1511596146|
|      1|4606018|    2735466|      pv|1511616481|
|      1| 230380|     411153|      pv|1511644942|
|      1|3827899|    2920476|      pv|1511713473|
|      1|3745169|    2891509|      pv|1511725471|
|      1|1531036|    2920476|      pv|1511733732|
+-------+-------+-----------+--------+----------+
only showing top 10 rows

```

**Transform timestamps from Unixtime to date**
The original timestamp is in format of Unixtime, therefore transforming it into 6 new readable field as datetime, date, month, day, hour and dayofweek. 
```py
from pyspark.sql.functions import (dayofmonth, hour,
                                  dayofyear,month, dayofmonth,
                                  year,weekofyear,
                                  format_number, date_format, to_date, dayofweek)

df1.select(dayofmonth(df1.datetime)).show()

df1 = df1.withColumn('date', to_date(df1.datetime)) \
            .withColumn('month', month(df1.datetime)) \
            .withColumn('day', dayofmonth(df1.datetime)) \
            .withColumn('hour', hour(df1.datetime)) \
            .withColumn('dayofweek', dayofweek(df1.datetime))  
```
```
Results:
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


**Discover dataset on the range of date**

```SQL
SELECT Date, n_interactions
FROM
    (SELECT date as Date, COUNT(user_id) as n_interactions
    FROM taobao
    GROUP BY date
    ORDER BY date)
WHERE n_interactions > 10000
```
```
         Date  n_interactions
0  2017-11-24         3453235
1  2017-11-25        10598765
2  2017-11-26        10496631
3  2017-11-27         9985084
4  2017-11-28         9987905
5  2017-11-29        10350799
6  2017-11-30        10542266
7  2017-12-01        11712571
8  2017-12-02        14057989
9  2017-12-03         8946657
```
The distribution shows most of the interactions are conducted between *2017-11-24* to 2017-12-03 (10 days).

**Create TempView as Taobao from records based on the distribution**
```py
df1.createOrReplaceTempView("taobao")
```


---
Copyright @ 2021 Zizhun Guo. All Rights Reserved.

