---
layout: post
title:  "Personal Website"
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 0
---

用处：
展示个人的信息，所以首先展示的信息是最重要的，个人网站的设计应该放大信息的传递的有效性，设计师可以展示个性，工程师可以展示信息，所以高校有的教师喜欢用plain html来发布课程信息。这完全取决于个人的诉求，好的设计可以有效地增强信息传递效率，同时避免用户因为不好的设计而浪费时间，但也不完全是这样，有时候出去业务考虑也会需要增强用户浏览网页的粘性。所以网页的功能和目的是最重要的。

我个人的网站的用途主要还是用来展示个人项目，所以信息的传递非常重要，从我开始规划制作网页，最简单直接的方法就是纯html，这满足了我的基本需求，但是对于信息的传递会有不好的影响，因为我需要考虑到查看我网页的受众，多数同学家人还有招聘方面的人员，所以网站需要基本的吸引力和亲和力能够让人们首先停留，其次有效的的信息传递就足够了。

根据markdown file in the root working directory, 启动服务器会根据当前markdown文件提供的信息生成网页，提供的信息由title,category提供，配置文件会根据提供信息生成设计好的网页。
所以开发者需要**定义好**每一个网页的属性。

本质就是一个静态网页生成器，开始在服务器部署时会根据提供的配置信息直接一次性生成所有网页，每一次更新博客，都是相当于在GitHub 服务器上重新部署了一次。
