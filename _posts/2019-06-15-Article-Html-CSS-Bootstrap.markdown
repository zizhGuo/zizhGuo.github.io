---
layout: post
title:  "Learning Notes: HTML+CSS+Bootstrap"
date:   2019-06-15 22:21:06
author: Zizhun Guo
category: Articles
categories: jekyll update
---

### **1. General**
<br>

##### **Resources links**

[Web Design Trend 网页设计趋势](https://cdc.tencent.com/2013/11/06/%E3%80%90%E5%93%81%E7%89%8C%E7%BB%B4%E7%94%9F%E7%B4%A0%E3%80%91%E6%B5%85%E8%B0%88%E5%BD%93%E4%B8%8B%E7%BD%91%E9%A1%B5%E8%AE%BE%E8%AE%A1%E8%B6%8B%E5%8A%BF/)

[Web Design Principles 网页设计理念](https://cdc.tencent.com/2013/11/06/%E3%80%90%E5%93%81%E7%89%8C%E7%BB%B4%E7%94%9F%E7%B4%A0%E3%80%91%E6%B5%85%E8%B0%88%E5%BD%93%E4%B8%8B%E7%BD%91%E9%A1%B5%E8%AE%BE%E8%AE%A1%E8%B6%8B%E5%8A%BF/)

[Ten Creative Personal Websites 十个创意十足的个人网站](http://www.coolsite360.com/resources/article/5a1926a3c8d27f033aa71f48/)

[PenCode-Real-time web design](PenCode.io)

[MDN Leanring Web Design](https://developer.mozilla.org)

[Lorem Ipsum placeholder text](https://loremipsum.io/)

<br>

### **2. HTML**
<br>

##### **HTML – hypertext markup language**
- Define the structure of a webpage
- Put an image here
- Put a form here
- The noun of a webpage

<br>
##### **Some elements and tips**
<br>


```html
General Rule:
<tagName> Some Content </tagName>
```
<br>
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-articles-webdev1/img-01.jpg" alt="drawing" style="width: 70%;"/>
</div>
<br>

- Two categories of elements in HTML:
    - block-level elements
    - Inline elements

Target attribute will be pointed to open a specific tab. If existed, open the link in the existed tab, or open a new one.
To make the text to be bold, instead using <b>, using <strong> which give a little bit more information. I.e. using <em> rather <i>.

```html
A traditional way to make grid border 
<table border="1"></table>

A traditional way to change the resize the image
<img width="500">
```

```
Tips: A way to type same thing in the Sublime Text 
        in multiple places is by holding control and click 
        different places, there would be more cursor generated 
        to type in simultaneously.
```

两种方式实现绑定一个text input到标签：
一种是nest，input在label里面；
另一种是两个并列，通过for在label里，id在input里进行绑定。
Input里的name是传输信息的名字，required是必填信息，pattern是限制位数等
如果是radio的input，可以设置选定之用value
Type不应，input的输入也不一样。
Select name是这个值的mingzi，里面的值就是option，如果想override值，可以在option里用value
Placeholder作为input的默认显示。Title在input里可以做警告信息用。
每一行可以用div来分段，这里的spacing在之后用CSS方便进行存储调控。

<br>

### **2. CSS**
<br>

##### **CSS – cascading style sheets**
- Defines the style of HTML
- Make all text purple
- Give the first image a yellow border
- The adjective of a webpage

<br>
##### **Some Selectors and tips**

Put the <link> at head part of html file to reference a css file.
Selector {color:  rgb(); #FFFFFF, rgba();}
RGBA最后一个位是透明度
Background可以用图片，border一般要设置三个property（像素宽度，风格，颜色），所以一般直接用border来当作property。
对于单独元素想要设置风格的：
1. 加入id attribute，设置比如special，然后再CSS中用#special来当作selector；
2. 加入class attribute，设置比如completed，然后在CSS中用.completed来当作selector。
总结目前来看三种selectors：
1. Element selector，比如li
2. class selector，比如.hello
3. id selector， 比如#name
其他需要讲的是，star，descendant selector，adjacent selector，attribute selector，nth of type
Star： 全部

```html
/*star*/
* {
    border: 1ps solid lightgrey;
}

/*Descendant Selector*/
li a {
    color: red;
}

/*Adjacent*/
h4 + ul {
    border: 4px solid red;
}

/*attribute*/
a[href = "https://google.com"] {
    background: blue;
}

/*Order from top*/
ul:nth-of-type(3){
    background: purple;
}
```
##### **Specifity**

[Specifity Calculator 权重计算器](https://specificity.keegan.st/)
[一篇博客](https://www.w3cplus.com/css/css-specificity-things-you-should-know.html)
[Articles from Zhihu 知乎贴](https://zhuanlan.zhihu.com/p/50322177)

```css
01. *{}                         ====》0
02. li{}                        ====》1(一个元素)
03. li:first-line{}             ====》2(一个元素，一个伪元素)
04. ul li {}                    ====》2（两个元素）
05. ul ol+li{}                  ====》3（三个元素）
06. h1+ *[rel=up] {}            ====》11（一个属性选择器，一个元素）
07. ul ol li.red{}              ====》13（一个类，三个元素）
08. li.red.level{}              ====》21（两个类，一个元素）
09. style=""                    ====》1000(一个行内样式)
10. p {}                        ====》1（一个元素）
11. div p {}                    ====》2（两个元素）
12. .sith {}                    ====》10（一个类）
13. div p.sith{}                ====》12（一个类，两个元素）
14. #sith{}                     ====》100（一个ID选择器）
15. body #darkside .sith p {}   ====》112(1+100+10+1,一个Id选择器，一个类，两个元素)
```
<br>

##### **Fonts**

[Fonts Stack](https://www.cssfontstack.com/)

<br>

##### **Box Model**

<br>
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-articles-webdev1/img-02.jpg" alt="drawing" style="width: 70%;"/>
</div>
<br>

##### **Float**

[cnBlog ](https://www.cnblogs.com/zfquan/p/7793945.html)

It is necessary to clearify the demands before develop the web pages. Naming the different part with specific class for better manage them.

<br>
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-articles-webdev1/img-03.jpg" alt="drawing" style="width: 70%;"/>
</div>
<br>

<br>


### **3. Bootstrap general**
<br>
Bootstrap is an open source toolkit for developing with HTML, CSS, and JS.

They way to use bootstrap in html is to reference the classes from Bootstrap Framework.

```html
<a ref="https://rit.edu" class = "btn btn-info btn-lg">RIT button</a>
```

<br>

##### **Container 容器**

Container 是一个好的容器可以让东西看起来不错。
Form-group class的用处上给与div optimum spacing
form-control class的用处是让输入栏变得好看.
So, all these two groups are mainly serving for making the input good looking.

如果我们想要inline-form的效果，直接在form标签里加入class form-inline就好了。

<br>

##### **Navbar**

Brand封装成一个单独的div，装进nav-header里面，之后移动端的压缩按钮也装在header里面。
Navbar按钮分别封装成一个div可以，也可以是带navclass的ul。（注：左边的按钮为一个封装， 右边的为另一个封装）。

可以伸缩的汉堡包navbar按钮
1. 用div标签的class collapse navbar-collapse封装需要压缩的按钮（设置id）
2. 在nav-header里加入按钮button，设置class为navbar-toggle collapsed.设置target到对应的#id。（ref 官方default nav）。不需要再次封装，放进header就可以。

<br>

#### **Grid System**

Probably the main reason why people use bootstrap.
Mobile collapse looks good.

1. add div class container
2. add div class row
3. add div class col-lg-4 col-md-3 col-sm-6 col-xs-12
列-大小-占比12个格子的多少个

[How to include incons and fonts to Bootstrap 3](https://stackoverflow.com/questions/19608873/how-to-include-glyphicons-in-bootstrap-3)

<br>


### **4. Bootstrap 4**
<br>

Some of the important changes from Bootstrap 3 to Bootstrap 4:
3到4的一些重要改变：
1.  flexbox的使用（float）.
2.  global change：
a.  转换从px到rem来表示primary CSS unit。
b.  Global font size increased from 14px to 16px.
c.  Grid size: 增加最小，移除-xs
3.  Adding the padding shortcuts.
4.  Moved to flexbox. Upgraded the grid class name.
5.  Add cards class, dripped panels, thumbnails, and wells.
6.  No icon font like Glyphicons.
7.  Refactor all components make it less nested.

其实很多东西不需要用style标签来重新设置，比如设置字体居中，字体大小，直接用继承相应的class就好了，比如<div class = “container text-center”

<br>
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-articles-webdev1/img-05.jpg" alt="drawing" style="width: 70%;"/>
</div>
<br>
全部都是继承不同的class来叠加相应的功能。
Class: btn代表这个实现按钮的功能。
Class: btn-warning 代表实现警告样式。
Class: p-sm-5代表padding在浏览器pixels值变小时变成3rem，最小1代表0.25rem。
只有五个等级的大小（1， 2， 3， 4， 5），如果没有等级默认为最小的情况。
设置大小：向右覆盖。

##### **Flexbox**

Two direction:
1.  Main axis (left – right)
2.  Cross axis (top – bottom)

Some classe from Bootstrap 4 to layout the flexbox:

- Justify Content
- Class Justify-content-end, center, between, around
- Align items
- Direction
- Flex-row
- Flex-column

现在是两种方法在同一行平铺元素，一种是之前讲的breakpoint部分，不过那个部是通过设置margin和padding的size来自做到，第二种是flexbox，而且flexbox更广泛的可以用于在box里布局。

<br>
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-articles-webdev1/img-04.jpg" alt="drawing" style="width: 70%;"/>
</div>
<br>

### **Extra Bootstrap 4 notes**

<br>

##### [**div vs section**](https://www.quora.com/What-is-the-difference-between-div-and-section-tag)

##### [**Div vs Span**](https://htmldog.com/guides/html/intermediate/spandiv/) 

差不多，都没有literal means，span是in-line，div是block。

##### [Media Queries](https://www.w3schools.com/cssref/css3_pr_mediaquery.asp)

##### [下划线效果](https://www.w3schools.com/cssref/css3_pr_mediaquery.asp)


