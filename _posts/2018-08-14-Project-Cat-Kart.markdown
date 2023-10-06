---
layout: post
title:  "🎓👾游戏设计与开发专业 毕业顶点项目：虚拟现实多人实时竞速射击游戏-猫咪飞车：开始 Game Design & Development Capstone Projects: VR Multiplayer Real-time Interaction Game Application"
date:   2018-08-14 19:34:06
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/title.png" alt="drawing" style="width: 100%;"/>
</div>

<!-- MarkdownTOC -->

- [Project Repo Link](#project-repo-link)
- [**1. The nature of this project**](#1-the-nature-of-this-project)
  - [**1.1 What is the game about?**](#11-what-is-the-game-about)
  - [**1.2 What are the reasons for this project getting born?**](#12-what-are-the-reasons-for-this-project-getting-born)
  - [**1.3 What are the those problems that has been solved by this project?**](#13-what-are-the-those-problems-that-has-been-solved-by-this-project)
  - [**1.4 How are the project under developed?**](#14-how-are-the-project-under-developed)
- [**2. Implementation detail**](#2-implementation-detail)
  - [**2.1. Why Unity 3D?**](#21-why-unity-3d)
  - [**2.2. Why Visual Studio?**](#22-why-visual-studio)
  - [**2.3. Why C-sharp\(C#\)?**](#23-why-c-sharpc)
  - [**2.4. Why SourceTree?**](#24-why-sourcetree)
  - [**2.5. What are the major tech problems that I met through development?**](#25-what-are-the-major-tech-problems-that-i-met-through-development)
    - [**Set up the deve-tools for efficiency**](#set-up-the-deve-tools-for-efficiency)
    - [**Make it online to implement basic functionality**](#make-it-online-to-implement-basic-functionality)
    - [**Clearify what game design features that would utilize the UNet API \(or would need to go on internet\)**](#clearify-what-game-design-features-that-would-utilize-the-unet-api-or-would-need-to-go-on-internet)
    - [**Optimization**](#optimization)
- [**3. Personal Contribution**](#3-personal-contribution)
  - [**3.1. What are the jobs I was in charge for?**](#31-what-are-the-jobs-i-was-in-charge-for)
  - [**3.2. Be more specific?**](#32-be-more-specific)
    - [**Implemented the functions of real-time interaction between VR end and PC end through internet**](#implemented-the-functions-of-real-time-interaction-between-vr-end-and-pc-end-through-internet)
    - [**Coordinated with three other artists and two sound producers**](#coordinated-with-three-other-artists-and-two-sound-producers)
    - [**Performed as the lead designer for multiplayer game mode**](#performed-as-the-lead-designer-for-multiplayer-game-mode)
    - [**Film Editor? Marketing?**](#film-editor-marketing)
- [4. Extra Info](#4-extra-info)

<!-- /MarkdownTOC -->

<br>

This article would mainly talk about the **nature** of my capstone project with essential **ideas** and **thoughts**.

<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/poster.jpg" alt="drawing" style="width: 70%;"/>
   <figcaption>Catcart: The Begining Poster</figcaption>
</div>

<br>
#### [Project Repo Link](https://github.com/zizhGuo/Catcart-Invasion)


<!-- # Cat Kart
>  The project is meant for utilizing all knowledges acquired throughout two-year studying at RIT School Interactive Games and Media to build up a final game project that is fun to play, so that students are able to develop game application in the very professional ways. Mission accomplished! 
>
> --- before read


> This is a year-long capstone project for a two-year master's program of Game Design and Development at RIT. The project is meant for utilizing all knowledges acquired throughout two-year studying to create a indi-game. The whole process involves varied skills that are necessary for later engineering work, i.e. technologies, tools, cooperation, etc.  -->

<br>

### **1. The nature of this project**
<br>

##### **1.1 What is the game about?**
This project is the multiplayer part of the game application CatCart: The Beginning, a VR driving and combat experience. The original singleplayer mode is focused on maximizing the experience of Virtual Reality, whereas the multiplayer part are heavily lying on the cross-platform interation between VR player and Non-VR player. 

<br>

<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/img-01.jpg" alt="drawing" style="width: 70%;"/>
   <figcaption>Singleplayer mode</figcaption>
</div>

> Original Catcart Singleplayer Mode:
> The Catcart: Begining allows the player to have a laser pointer as one of its controllers and drives the CatCart with it, by pointing the laser on the ground in front of the CatCart. The CatCart has a basket with nine cats in it, which are the player’s nine lives. The enemy drones try to steal the cats from the player. With their second controller, the player is able to do ranged attack and shoot alien drones with the help of a raygun as their other controller, and melee attack when converted into a cat-paw. The player is also equipped with a HUD, that displays the player’s stats and indicates danger.

<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/img-02.jpg" alt="drawing" style="width: 70%;"/>
   <figcaption>Game Object: Cart</figcaption>
</div>
<br>
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/img-03.jpg" alt="drawing" style="width: 70%;"/>
   <figcaption>Game Object: The Shooter</figcaption>
</div>
<br>

The difference is that in Multiplayer game mode, it has two players experiencing in the same time. Like the singleplayer mode, the VR player is able to drive and shoot, while the non-VR player are set to have a top-down view for being able to interact with varied props in the scene. Both players are targeting each other by using their own abilities. The goal for the VR player is to drive the catcart to the finish line, while the non-VR player plays the role to manage to prevent it. 

<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/img-04.jpg" alt="drawing" style="width: 70%;"/>
   <figcaption>VR player in game view</figcaption>
</div>

<br>

<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/img-05.jpg" alt="drawing" style="width: 70%;"/>
   <figcaption>Non-VR player in game view</figcaption>
</div>

<br>

"This project" represents the multiplayer game mode of Catcart: Begining in the rest of this article.




<br>

##### **1.2 What are the reasons for this project getting born?**
The project is meant for utilizing all knowledges acquired throughout two-year studying at RIT School Interactive Games and Media to build up a final game project. So first of all, it is a meant for degree certification. There are three professors that made of the committee gourp to track the teams' development processes and assees the qualification whether the project are legitly good. One of committee member was also the instructor of Capstone Design and Capstone Porject courses, so that she can surpervise the development by offering suggestion and feedbacks. Students are also in charge for quality assurance by organizing play-test and postmortem review. Three approvals are necessary in order to pass the defense at end of masters' program's year.

[+ personal reason]

<br>

##### **1.3 What are the those problems that has been solved by this project?**

This project created a new gameplay(definition: the way it is played) that allows VR player and non-VR player interacts with each others in the same time at different scenes. The scenes for non-VR player are highly abstructed with only interactive game objects being kept and basic background architectures, it is optimized by lower down the frame rate for non-VR player. That being said, by sharing the same space coordinates through Unity server, all players have no problem of not being able to exchange information. 

See Part 2 for more details.

<br>

##### **1.4 How are the project under developed?**

We employes the agile methodology for development processes. 

<br>


### **2. Implementation detail**

https://github.com/zizhGuo/Catcart-Invasion

Tools & Language: Unity 3D, Visual Studio, C#.

Version Control: SourceTree

<br>

##### **2.1. Why Unity 3D?**
Since the singleplayer game mode is developed using Unity 3D, so there has no reason to switch to another game engine. Plus Unity3D's Networking has a "high-level" scripting API, so it is convenient to use such feature to develop multiplayer part. 
作为一个免费游戏引擎，Unity满足了游戏引擎的基本需要，对于不需要极度优化的中小型独立游戏是一个不错的选择。

<br>

##### **2.2. Why Visual Studio?**
The unity 3D embeds a default IDE script editor called Mono, in order to keep the same development pipeline as the rest of the team, it remains on visual studio for developing jobs. Unity引擎允许VS使用其debug功能来暂停游戏和继续游戏，从而找出错误点。

<br>

##### **2.3. Why C-sharp(C#)?**
Unity 3D uses C# to program. 

For more info about Unity, check this [link](https://unity3d.com/learning-c-sharp-in-unity-for-beginners)

[+ personal reason]

<br>

##### **2.4. Why SourceTree?**
Sourcetree is a free Git desktop client, it is basically the **git** + UI. 

<br>

##### **2.5. What are the major tech problems that I met through development?**


###### **Set up the deve-tools for efficiency**

The developing pipeline was the first major problem I solved that made a difference. Since it it was rare to find the games that have the similar forms like what we was developing (one project with two different scenes running and interacting in the same time), the regular development pipeline (you have to build out the application first in order to test two ends in the same time) would be time-costing which made the whole development at low efficiency. So it was necessary to make the testing process as much convinient as possible. The solution was to open the Unity project twice. In order to make it possile to test the VR and non-VR games **simultaneously** on the same system, there firstly needs to have the Unity Networking setting supports the VR, and then running the VR scene and starting the non-VR scene.

<br>

###### **Make it online to implement basic functionality**

 By implementing default **Unity Networking High-level API (UNet)**, it was possible to make the game online. These are some features that the UNet provides:
 - Network Manager (Local)
 - Unity Multiplayer Service 
 - MatchMaker (Online)

<br>
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/img-07.jpg" alt="drawing" style="width: 70%;"/>
   <figcaption>UNet: NetworkManager</figcaption>
</div>
<br>
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/img-08.jpg" alt="drawing" style="width: 70%;"/>
   <figcaption>The matchmaker</figcaption>
</div>
<br>

<div style="text-align: center;">
<iframe width="560" height="315" src="https://www.youtube.com/embed/lVFInbOzVGU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
 
 <br>

###### **Clearify what game design features that would utilize the UNet API (or would need to go on internet)**

There were many interactable game objects (the props in the scene that needs to be conmunicated for both players through internet) that needs to be implmeneted. Implementing each of them would involve a lot "nasty" jobs, so it was necessary to find the common parts among them all and do testing jobs ahead.

    Workflow:
    List out props -> find common features (online) -> make prototypes & code
    -> testing in the demo scene -> apply to different props
    -> modify for different props' unique features for game design

<br>
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/img-09.jpg" alt="drawing" style="width: 70%;"/>
   <figcaption>The matchmaker</figcaption>
</div>
<br>
<div style="text-align: center;">
     <iframe width="560" height="315" src="https://www.youtube.com/embed/BMfUxBqbj3M?start=26" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>



 <br>

###### **Optimization**

Since both VR scene and non-VR scene contains a great amount of game objects, where some of them are for backgrounds while others are for interaction, optimization should take into account. For non-VR scene, the camera view was locked into the air above floor, there could hide the details of buildings by only placing the rough modules. For VR scene, by applying LOD component, the scene only rendered the game objects that close to the player with most amount of details.

 <br>



### **3. Personal Contribution**
<br>
##### **3.1. What are the jobs I was in charge for?**

I was in charge for basically everything about the **technical jobs** for multiplayer game mode, the most of works of **designs** and **game testing**. I was also the core designers participating for Catcart: Begining singleplayer mode.

<br>

##### **3.2. Be more specific?**

For technical jobs, I basicically implemented all features that was required for the multiplayer mode that was everything about UNet API. Since the game was modified based on the singleplayer mode, there also had some migration works that was needed to be done before set off, including reading historic codes and connects to the current project. My works also included all major problems' solution that was talked about in the second chapter. I also did level designs, for the multiplayer mode was heavily based on the interaction, the game needed to be balanced in case the one of two players gets tedious. 

<br>

###### **Implemented the functions of real-time interaction between VR end and PC end through internet**

The UNet API provides very limited functions for internet-based information exchange, it has **transform** data that can be transmitted by default with the game object attaching the UNet transform component. In order to achieve much advanced functions, it is necessary to write own scripts to make it happen, which need the developer to test UNet documentation to test the interfaces needed. It was time consuming but fun progress. It was the only way to achieve the goal without implementing their own lower-leveled implementation with Sockets. For our project, these jobs are enough to make it work, considering the limited amount of time estimation.

###### **Coordinated with three other artists and two sound producers**

Since sound elements and arts elements are two of essential parts for game application project, we contacted and had worked with other five persons as we firstly had our game prototype delivered. It was a good and inspiring experience working with others besides the core developing members. I was in charging for helping coordinating with other sources by attending activities from other collges to look for artists and sound designers. The college of Interactive Game and Media provided many chances for us to engage with other people. 

We applied **Agile Methodology** on our development, of which we picked up from our Game Development Processes course. It went well for each iteration, and through weekly meeting, we were able to fix bugs and set new progressive goals. 

###### **Performed as the lead designer for multiplayer game mode**

Multiplayer level design was different with the singleplayer one, for it involves real-time interactions with high frequent gameplay operation. It was necessary to balance the gameplay between two players. In previous part that has mentioned the multiplayer game mode, there had not many cases that can be referenced and learned because its new design. So the way I took for building such levels was to firstly having the concept about the whole game progress, and listed all possible interesting interaction that could happen during the game time; Making the whole level divided into different modules so that they could be tested separately and at lasted to combine the modules together with most satisfied orders. 

To assess how the level performs, it was necessary to quantify the game progress and also to observe how engaging the players are. So I build up couple of times of game testing session and asked my friends to test. I got some results, i.e. the average level finishing time is around 8 mins and only half of VR players can beat the game. 

<br>
<div style="text-align: center;">
<iframe width="560" height="315" src="https://www.youtube.com/embed/WFsDqBNwTpo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

<br>
<div style="text-align: center;">
   <img src="{{site.url}}/assets/img-catcart/img-06.jpg" alt="drawing" style="width: 70%;"/>
   <figcaption>Level Design</figcaption>
</div>
<br>

###### **Film Editor? Marketing?**

Yes, we did have jobs for making marketing video to attract our future venture capitalists...uh...haha.just for kidding, we were making videos to attract artists and sound designers. We were also able to make it to the **BostonFIG Fest 2018** Indi-game competetion and make it possible to showcase our game there. 




<br> 

### 4. Extra Info

[blog article]()





