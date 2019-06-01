---
title: "How to open the same Unity project twice"
layout: post
date: 2018-02-20 11:58AM
image: /assets/header/forest.jpg
headerImage: true
tag:
- Daily life
category: blog
author: zizhguo
---

[Original Link](http://www.chadweisshaar.com/blog/2016/04/05/how-to-open-the-same-unity-project-twice/)

1. Having a current project you are working on. i.e. ABC
2. Copy the whole project directory and paste it to any other place. i.e."D:\ABCcopy"
3. Delete the Assets folder.
4. Link the original Asset directory to the new directory
	a. Open with "cmd" in command prompt
	b. "cd" to new location directory. i,e,"D:\ABCcopy"
	c. type "mklink /j " with the old assets folder location, i.e."D:\ABC\Assets"

##This is a good way to test the Unity project involving heavily development of Unity Networking, so that there's no need to build out project each time for testing. Very Useful!

##Pay specificly attention to this:

Changes made in one scene would no be mapped in another scene. Just try to refresh it by click other stuff and toggle back.


![Illustration Image]( {{site.url}}/assets/images/DoubleUnity/DuplicateUnityScene.png)