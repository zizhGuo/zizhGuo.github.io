---
title: "Github Page学习心得及总结"
layout: post
date: 2017-07-13 16:36
image: /assets/header/forest.jpg
headerImage: true
tag:
- blogtest
category: blog
author: zizhguo
---

### 题记
>闲时发现github page功能完备，甚是有趣，即可远观当作个人博客之用，又可近处记录生活点滴之需，故花费些许时学习些皮毛前端之术，本文为学习心得及笔记，如若不对，还请众看官指点一二，非常感激。

---

<p align="center">
<h1>2017年7月09日</h1>
</p>

## Tutorials on Youtube
[视频链接](https://www.youtube.com/watch?v=iWowJBRMtpc&list=PL0CB3OvPhDA_STygmp3sDenx3UpdOMk7P&index=5)

<iframe width="560" height="315" src="https://www.youtube.com/embed/iWowJBRMtpc?list=PL0CB3OvPhDA_STygmp3sDenx3UpdOMk7P" frameborder="0" allowfullscreen></iframe>

## Themes

Themes是已经通过jekyll搭建好的模板，包含了所需要的各种配置文件，只需要修改config.yml还有post里面的文章即可更换内容，但是规划形式被限制，需要自己修改或者重新配置。

## 模板
[Github模板列表](https://github.com/jekyll/jekyll/wiki/Themes)
我选用的模板:
[Indigo](https://github.com/sergiokopplin/indigo)|[Demo](http://koppl.in/indigo/) 

So simple模板是一个文档完善的模板，里面的内容可以借鉴
[So simple](https://mmistakes.github.io/so-simple-theme/articles/sample-post-images/)

tale 模板是我一个比较喜欢的极简风格模板，但是没有头像，所以暂时不用：
[Tale](https://chesterhow.github.io/tale/)

## Post&Markdown
post新博文用的语言是markdown，通过code editor工具来实现创建新文件，也可以在github网站上创建新文件，但是网页编程会出现跳行问题（不知道是不是edge浏览器的问题），网络推荐用sublime。同时谷歌搜索markdown editor会出现一些在线online 编辑器，可以实现左右对照。

Sublime官网：[https://www.sublimetext.com/](https://www.sublimetext.com/)

Markdown官网：[https://daringfireball.net/](https://daringfireball.net/)

Markdown Editor on Visual Studio: [https://marketplace.visualstudio.com/items?itemName=MadsKristensen.MarkdownEditor](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.MarkdownEditor)

## Jekylly
[Jekyll中文](http://jekyllcn.com/) | [Jekyll 英文](http://jekyll.com/)

### 一些技巧
- Index为主页，放在根目录，以其他页面命名的页面放在根目录,则会对应生成相应的URL

- 一些技术博客：[http://blog.csdn.net/marksinoberg/article/details/51375953](http://blog.csdn.net/marksinoberg/article/details/51375953)
[http://www.cnblogs.com/crazyant007/p/4220066.html](http://www.cnblogs.com/crazyant007/p/4220066.html)
[http://krisyu.github.io/](http://krisyu.github.io/)(此博客后面有作者搜集的其他技术博客列表)

## Configure
[https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/)

## 总结
如果自己搭建的话，我认为按照步骤，利用cmd 输入bash，把自己的repo 给clone到本地，然后cd到指定目录，然后利用jekyll指令，此时再使用sublime这种code editor来创建文件，由于如果用command prompt集成git，所以可能需要用到git指令吧，我还没有看视频，需要参考我收藏的youtube频道里的两个视频来做参考。如果我猜测的没错的话，回了jekyll利用markdown就可以不需要使用html 或者CSS来写网页，它会自动转化，就算是连网站架构也是不需要。可能会需要liquid语言做模板（jekyll网页模板专区有提到。
{% highlight html %}
<img class="image" src="{{ site.url }}/assets/images/markdown.jpg" alt="Alt Text">
{% endhighlight %}

---

<p align="center">
<h2>2017年7月12日</h2> 
</p>

学会了一些html和markdown的基本语法，可以顺利行文书写博客，包括插入图片，图片排版，插入emoji表情（需要根据图片resolution来选择），根目录下各文件夹配置文件功能部分摸清，layouts里面是页面的排版，排版文件为html，可以include别的文件，类似oop编程原理。posts里为博文，不需描述。includes里为每种页面中不同模块，比如作者模块，header模块，footer模块，也是html编写，现在还需了解sass文件价的功能，以及如何开始从头配置网站，而不是先利用模板再改。
图片不仅可以引用个人上传，还可以引用其他网站图片，只要是有url就可以使用，需要注意site.url 代表zizhguo.github.com 这个可以在config 文件里进行配置
Site.picture代表个人可以在自行配制
格式类似
{% highlight html %}
{{site.url}}/{{site.picture}}
{% endhighlight %}

最简单明了的方法是
{% highlight html %}
xxx.github.com/assets/profile.jpg
{% endhighlight %}

---

<p align="center">
<h1>2017年7月13日</h1> 
</p>

一篇相当详细的中文博文阐述SVG矢量图片的介绍和使用：
[http://www.w3cplus.com/svg/how-to-create-an-icon-system-using-svg-symbols.html](http://www.w3cplus.com/svg/how-to-create-an-icon-system-using-svg-symbols.html)

获得SVG的方法，在图片上点击右键选择inspect element

SVG资源：
[https://icomoon.io/app/#/select](https://icomoon.io/app/#/select)
[https://simpleicons.org/](https://icomoon.io/app/#/select)
