---
layout: post
title:  "无服务器应用程序接口开发 Serverless App development REST API- AWS Lambda + API Gateway + DynamoDB"
date:   2020-05-14 10:20:32
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---

Author：Zizhun Guo
Contact: zg2808@rit.edu
Phone: (585)284-0464
Date: May 14th 2020
<br>
**Technology stack：**
Serverless
AWS S3/Lambda/API Gateway/DynamoDB
Node.JS
**Tools:**
VSCode/terminal
System:
Windows/UBuntu

1. Terms
**serverless:**
朴素的理解：
这个serverless类似是一个集成框架，类似提供蓝图功能帮助AWS CloudFormation快速搭建模块，这样可以快速搭建部署不同的服务，比如静态/动态网站，restAPI等等。[Serverless Github示例工程](https://github.com/serverless/examples)。如果单独部署，比如lambda部署function测试，API Gateway部署GET/POST激活特定的lambda function，AWS S3单独传打包好的项目文件去部署，然后链接数据库等等。这个serverless有个ymal的配置文件可以帮助配置整个项目，部署前需要安装AWS CLI在电脑上用来激活IAM的身份验证。
专业性理解：
按需使用，让开发专注于业务，按内存时间计费，依赖特定云平台及服务(Lambda)。
![serverless-spa-architecture](https://i.imgur.com/GqM0BXF.png)
**Image 1: Serverless SPA架构**

**Lambda**
[如何看待 AWS Lambda ？ - 知乎](https://www.zhihu.com/question/29490143)
FaaS(Function as a Service)，直接部署function到AWS上，不需要理会任何关于服务器方面的工作，都交给云平台了。

**API Gateway**
> Amazon API Gateway 是一种完全托管的服务，可以帮助开发者轻松创建、发布、维护、监控和保护任意规模的 API。所有API的请求入口。
**DaynamoDB**
> Amazon DynamoDB is a fully managed proprietary NoSQL database service that supports key-value and document data structures and is offered by Amazon.com as part of the Amazon Web Services portfolio. 

**AWS S3**
> Amazon S3 or Amazon Simple Storage Service is a service offered by Amazon Web Services that provides object storage through a web service interface. 
存储桶bucket, 可以存储对象读取写入，类似云盘。
2. SETUP IAM
add user
sets programming access
sets attach current policy as AdminnistratorAccess
gets Access key ID and secret access key
bind it to environment so that serverless can use it to access Lambda & API Gateway
![01](https://i.imgur.com/OmXzdWA.png)
**Image 2: setup Credential for serverless**


3. Serverless Install
两种方法：
方法一：
用git clone下来实例project，然后cd到目录下操作。
方法二：
安装命令
```
serverless install -u https://github.com/serverless/examples/tree/master/aws-node-rest-api-with-dynamodb -n aws-node-rest-api-with-dynamodb
```

4. 查看配置文件和JS代码 

**serverless.yml**
```
provider:
  name: aws
  runtime: nodejs10.x
  environment:
    DYNAMODB_TABLE: ${self:service}-${opt:stage, self:provider.stage}
  iamRoleStatements:
    - Effect: Allow
      Action:
        - dynamodb:Query
        - dynamodb:Scan
        - dynamodb:GetItem
        - dynamodb:PutItem
        - dynamodb:UpdateItem
        - dynamodb:DeleteItem
      Resource: "arn:aws:dynamodb:${opt:region, self:provider.region}:*:table/${self:provider.environment.DYNAMODB_TABLE}"
```

```
functions:
  create:
    handler: todos/create.create
    events:
      - http:
          path: todos
          method: post
          cors: true
```
```
resources:
  Resources:
    TodosDynamoDbTable:
      Type: 'AWS::DynamoDB::Table'
      DeletionPolicy: Retain
      Properties:
        AttributeDefinitions:
          -
            AttributeName: id
            AttributeType: S
        KeySchema:
          -
            AttributeName: id
            KeyType: HASH
        ProvisionedThroughput:
          ReadCapacityUnits: 1
          WriteCapacityUnits: 1
        TableName: ${self:provider.environment.DYNAMODB_TABLE}
```

**get.js**
```js
'use strict';

const AWS = require('aws-sdk'); // eslint-disable-line import/no-extraneous-dependencies

const dynamoDb = new AWS.DynamoDB.DocumentClient();

module.exports.get = (event, context, callback) => {
  const params = {
    TableName: process.env.DYNAMODB_TABLE,
    Key: {
      id: event.pathParameters.id,
    },
  };

  // fetch todo from the database
  dynamoDb.get(params, (error, result) => {
    // handle potential errors
    if (error) {
      console.error(error);
      callback(null, {
        statusCode: error.statusCode || 501,
        headers: { 'Content-Type': 'text/plain' },
        body: 'Couldn\'t fetch the todo item.',
      });
      return;
    }

    // create a response
    const response = {
      statusCode: 200,
      body: JSON.stringify(result.Item),
    };
    callback(null, response);
  });
};
```

query id, 如果有则返回200及内容，没有则返回501异常。

5. 部署Deploy
5.1 安装dependencies
```cmd
npm install
```
5.2 部署
```cmd
serverless deploy
```
![07](https://i.imgur.com/VNz5bnq.png)
**Image 3: Deployed serverless**

6. 结果及测试
利用ubuntu测试指令：
```cmd
curl -X POST https://szye94dpr1.execute-api.us-east-1.amazonaws.com/dev/todos --data '{ "text": "Zizhguo serverless" }'
```

![08](https://i.imgur.com/CAJo8y7.png)
**Image 4: Test POST Create statement**
有返回结果，登录AWS DynamoDB验证结果：
![09](https://i.imgur.com/6V2dLYE.png)
**Image 5: Succesful return the created record**

数据有添加，说明添加数据没有问题，其他的REST API分别是：
```
  (已测试)POST - https://szye94dpr1.execute-api.us-east-1.amazonaws.com/dev/todos
  GET - https://szye94dpr1.execute-api.us-east-1.amazonaws.com/dev/todos
  GET - https://szye94dpr1.execute-api.us-east-1.amazonaws.com/dev/todos/{id}
  PUT - https://szye94dpr1.execute-api.us-east-1.amazonaws.com/dev/todos/{id}
  DELETE - https://szye94dpr1.execute-api.us-east-1.amazonaws.com/dev/todos/{id}
```

7. Summary
More Serverless examples:
Github Repo: [https://github.com/serverless/examples](https://github.com/serverless/examples)
![10](https://i.imgur.com/oatAIep.png)
**Image 6: More examples**


8. Reference:

https://www.serverless.com/framework/docs/providers/aws/guide/credentials/

https://serverless.ink/#serverless-%E7%9A%84%E9%97%AE%E9%A2%98

https://zhuanlan.zhihu.com/p/31090697

https://serverless.ink/#%E4%BB%80%E4%B9%88%E6%98%AF-serverless-%E6%9E%B6%E6%9E%84

https://www.serverless.com/framework/docs/providers/aws/guide/credentials/

<tr>Zizhun Guo@All rights reserved</tr>
