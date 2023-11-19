---
layout: post
title:  "游戏商业化运营-活动推送转化报表 Cracking the activity conversion report task"
date:   2023-11-17 09:33:30
author: Zizhun Guo
authorCN: 郭子谆
category: Projects
categories: jekyll update
visible: 1
porject: 1
---

<br>

---

<br>

- [1. 任务介绍 Task Introduction](#1-任务介绍-task-introduction)
    - [1.1 目的](#11-目的)
    - [1.2 报表产出形式](#12-报表产出形式)
- [2. 日志表介绍 Log Table Introduction](#2-日志表介绍-log-table-introduction)
    - [2.1 活动推送表 Activity Push Log](#21-活动推送表-activity-push-log)
    - [2.2 用户登录表 User Login Log](#22-用户登录表-user-login-log)
    - [2.3 用户订单表 User Order Log](#23-用户订单表-user-order-log)
- [3. 挑战 Challenge](#3-挑战-challenge)
- [4. 实现方案](#4-实现方案)
    - [4.1 产出要求和解决方法](#41-产出要求和解决方法)
    - [4.2 活动推送用户登录订单明细表开发流程设计](#42-活动推送用户登录订单明细表开发流程设计)
    - [4.3 SQL代码](#43-sql代码)
    - [5. 执行工具](#5-执行工具)
    - [6. 误差评估](#6-误差评估)


#### 1. 任务介绍 Task Introduction

###### 1.1 目的

对于手机游戏来讲，活动礼包推送在游戏商业化运营中所占比重巨大，在正确的时间给正确的人推送正确的礼包（价格和内容）是需要技巧经验的技术活，我们不能期盼着用户始终对产品不离不弃，哪怕是我们认定的忠实用户，也需要不断的激励和回馈。因此，我们需要对活动推送的转化效果进行监控，以便于我们对活动推送的效果进行评估，从而优化活动推送的效果。

###### 1.2 报表产出形式

| push_date | level_group | goods_price | total_users | login_users | paid_users | total_revenue | paid_percentage |
|---|---|---|---|---|---|---|---|
| 2023-11-11 | 1-5 | 6 | 1000 | 500 | 100 | 600 | 20% |
| 2023-11-18 | 1-5 | 6 | 1200 | 600 | 130 | 780 | 20% |

以上报表为某次活动推送的转化报表，其中：
push_date：活动推送日期
level_group：活动推送等级组
goods_price：活动推送礼包价格
total_users：活动推送用户总数
login_users：活动推送用户中登陆用户数
paid_users：活动推送用户中付费用户数
total_revenue：活动推送用户中付费用户的总收入
paid_percentage：活动推送用户中付费用户占比

对于案例中2023-11-11和2023-11-18两次活动推送，该报表记录了两次活动推送的转化效果，其中2023-11-11的活动推送等级组为1-5，礼包价格为6，活动推送用户总数为1000，登陆用户数为500，付费用户数为100，付费用户总收入为600，付费用户占比为20%。同理，2023-11-18的活动推送等级组为1-5，礼包价格为6，活动推送用户总数为1200，登陆用户数为600，付费用户数为130，付费用户总收入为780，付费用户占比为20%。

如何产出该报表，是本文的主要内容。

#### 2. 日志表介绍 Log Table Introduction

###### 2.1 活动推送表 Activity Push Log

日志表: ods_game_activity_push_log

| uid  | level | createtime         | endtime           |
|------|-------|--------------------|-------------------|
| 1001 | 5     | 2023-11-18 08:00:00| 2023-11-18 08:30:00|
| 1002 | 7     | 2023-11-18 09:00:00| 2023-11-18 09:45:00|
| 1003 | 10    | 2023-11-18 10:00:00| 2023-11-18 11:00:00|
| 1004 | 12    | 2023-11-18 11:15:00| 2023-11-18 12:15:00|

该日志表记录了活动推送的用户，等级组，生效时间和失效时间，其中生效时间和失效时间是活动推送的生效时间区间，例如1001用户在2023-11-18 08:00:00到2023-11-18 08:30:00这个时间段内，可以看到活动推送的内容，而1001用户在2023-11-18 08:00:00之前或者2023-11-18 08:30:00之后，都无法看到活动推送的内容。

###### 2.2 用户登录表 User Login Log

日志表: ods_user_login_log

| uid  | logintime            | logouttime           |
|------|----------------------|----------------------|
| 1001 | 2023-11-18 07:45:00  | 2023-11-18 08:35:00  |
| 1002 | 2023-11-18 08:50:00  | 2023-11-18 09:50:00  |
| 1003 | 2023-11-18 09:55:00  | 2023-11-18 11:05:00  |
| 1004 | 2023-11-18 11:10:00  | 2023-11-18 12:20:00  |
| 1001 | 2023-11-18 12:30:00  | 2023-11-18 13:00:00  |

该日志表记录了用户的登陆和登出时间，其中登陆时间和登出时间是用户的登陆时间区间，例如1001用户在2023-11-18 07:45:00到2023-11-18 08:35:00这个时间段内，登陆了游戏，而1001用户在2023-11-18 07:45:00之前或者2023-11-18 08:35:00之后，都没有登陆游戏。

###### 2.3 用户订单表 User Order Log

日志表: ods_order_log

| uid  | createtime         | price | good         |
|------|--------------------|-------|--------------|
| 1001 | 2023-11-18 08:05:00| 6 | Game Credits |
| 1002 | 2023-11-18 09:10:00| 648 | Expansion Pack|
| 1003 | 2023-11-18 10:20:00| 128 | Power Boost  |
| 1004 | 2023-11-18 11:30:00| 6.00  | Cosmetic Item|
| 1001 | 2023-11-18 08:40:00| 18| Subscription |

该日志表记录了用户的订单，其中订单时间是用户的订单时间，例如1001用户在2023-11-18 08:05:00购买了6元的游戏币，而1001用户在2023-11-18 08:05:00之前或者2023-11-18 08:05:00之后，都没有购买过6元的游戏币。

#### 3. 挑战 Challenge

- SQL实现角度
  - 时间区间重合判定：需要对个时间点进行范围判断
  - 转换涉及到多种类型的用户行为埋点日志：逻辑相对比普通单表报表指标复杂
  - 没有推送批次号：只能通过日期分组，日期需要creattime换算
- 性能角度
  - 扫描量巨大：需要对每个用户的每个批次的生效时间区间进行判定
  - 产出时间要求：需要在有限时间内产出日报
  - 集群资源有限：需要在有限的集群资源下4核32g，保证产出效率

#### 4. 实现方案

###### 4.1 产出要求和解决方法

产出要求
- 所有字段均可通过SQL实现
- 有限时间内产出日报
- 在有限的集群资源下，保证产出效率

解决方法
- 数据建模分层：现生成明细表再基于明细表产出日报
- 数据查询分片：按照批次分片，每个批次单独产出明细表，每两个批次对比
- 自动化定时任务：使用crontab定时任务，每天凌晨自动产出明细表和日报
- 优化SQL：
  - 使用窗函数，减少数据扫描量
  - 使用分区表，减少数据扫描量
  - 使用临时表，减少数据扫描量
  - 使用SQL生成工具
    - 减少人工编写SQL的时间
    - 减少人工编写SQL的错误率
    - 减少人工编写SQL的重复率

###### 4.2 活动推送用户登录订单明细表开发流程设计

<div style="text-align: center;">
<a href ="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/2023-11-monetization-activity-push-conversion-report/process_diagram.jpg">
<img src="https://img-bed-1317278737.cos.ap-shanghai.myqcloud.com/post/2023-11-monetization-activity-push-conversion-report/process_diagram.jpg" alt="drawing" style="width: 100%;"/>
</a>
<figcaption> 流程图 </figcaption>
</div>

###### 4.3 SQL代码

```sql
-- SQL 1
CREATE TABLE db.activity_push_batch_uid AS
SELECT
    uid
    ,level
    ,createtime
    ,endtime
    ,dense_rank() over(order by hour(createtime), minute(createtime)) as batch
FROM
    db.ods_game_activity_push_log
WHERE
    dt = '2023-10-10'
order by
    createtime asc;
```
SQL 1: 使用活动推送日志'2023-10-10'的分区表，并按照createtime排序，生成批次号，统计粒度为**小时-分钟**

```sql
-- SQL 2
CREATE TABLE if not exists db.activity_push_batch_createtime AS
SELECT
    batch
    ,createtime
    ,endtime
FROM
    (
        SELECT
            createtime
            ,endtime
            ,batch
            ,row_number() over (partition by hour(createtime), minute(createtime) order by createtime asc) as rank
        FROM
            db.activity_push_batch_uid T
            ) t
WHERE
    t.rank = 1
;

```
SQL 2: 使用SQL 1生成的批次号，按照批次号分组，取每组第一条记录的createtime和endtime，作为该批次的生效时间区间


```sql
-- SQL 3
CREATE TABLE db.bv_logint_logout uid AS
WITH A as (
SELECT 
    uid
    ,logintime as login_logout_time
FROM
    db.ods_user_login_log
WHERE
    dt >= '2023-10-10' and dt < '2023-10-13'
UNION
SELECT 
    uid
    ,logouttime as login_logout_time
FROM
    db.ods_user_login_log
WHERE
    dt >= '2023-10-10' and dt < '2023-10-13'
)

SELECT
    DISTINCT 
    T.batch
    ,A.uid
FROM
    A
INNER JOIN
    db.activity_push_batch_createtime T
on
    A.login_logout_time >= T.createtime
    and A.login_logout_time <= T.endtime
;

```
SQL 3: 使用玩家登陆日志'2023-10-10'到'2023-10-13'的分区表, 得到登陆登出时间字段，使用SQL 2生成的批次号，过滤出每个批次的登陆登出记录，得到每个批次的登陆用户

```sql
-- SQL 4
CREATE TABLE guozizhun.game_god_grant_push_batch_order_20231020 AS
WITH A AS (
    SELECT
        uid
        ,createtime
        ,price
        ,goodid
    FROM
        db.ods_order_log
    WHERE
        dt >= '2023-10-10' and dt < '2023-10-13'
)
,
B AS (
    SELECT
        A.uid
        ,T.batch
        ,A.createtime
        ,A.price
        ,A.goodid
    FROM
        A
    INNER JOIN
        db.activity_push_batch_createtime T
    on
        A.createtime >= T.createtime
        and A.createtime <= T.endtime
)

SELECT
    A.*
    ,B.batch
    ,B.uid
    ,B.createtime as ordertime
    ,B.price
    ,B.goodid
    ,DATEDIFF(DATE(B.createtime), DATE(A.createtime)) as days_from_createtime
FROM
    guozizhun.bv_logint_logout A
LEFT JOIN
    B
on
    A.uid = B.uid
    and A.batch = B.batch
;

```
SQL 4: 使用玩家订单日志'2023-10-10'到'2023-10-13'的分区表, 得到订单时间字段，使用SQL 2生成的批次号，过滤出每个批次的订单记录，通过left join得到对应每个批次的订单记录，得到每个批次的订单用户

###### 5. 执行工具

- QuestBee beeline cmd generating tool
- Hive
- crontab
- Microsoft Excel

###### 6. 误差评估

按照小时-分钟分组，并取每组第一条记录的时间作为该组时间，会造成误差，根据具体产出任务需要统计登陆转化和订单转化，

参考以下文档对误差定义，也可求助于大模型
[https://docs.citrix.com/zh-cn/citrix-virtual-apps-desktops/director/data-retention.html](https://docs.citrix.com/zh-cn/citrix-virtual-apps-desktops/director/data-retention.html)

- 误差人数
  - 定义
    - 正确人数：时间=生效时间
    - 误差人数：时间>生效时间
  - 影响
    - 误差人数的比例和分布
- 考虑因素
  - 该小时分钟的订单分布
  - 该小时和分钟登陆分布
- 如何计算
  - 发生误差的用户的行为发生率占所有行为发生的比例
  - 加权平均误差：加权平均误差=∑(时间偏差×该偏差的数据占比)
  - 误差人数占比：误差人数占比=误差人数/总人数


---
Copyright @ 2021 Zizhun Guo. All Rights Reserved.
