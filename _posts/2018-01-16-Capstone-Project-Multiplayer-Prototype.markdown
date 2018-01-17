---
title: "Capstone project: Catkart - Multiplayer prototype"
layout: post
date: 2018-01-16 16:36
image: /assets/header/forest.jpg
headerImage: true
projects: true
tag:
- projects
category: project
author: zizhguo
---

<h1>Describtion</h1>

<iframe width="560" height="315" src="https://www.youtube.com/embed/69PiSBPEcsg" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

---

"Catkart: in the begining", an a-year long project, is our Capstone Project. It has four tech guys in our team that make contribution on this project. We are dedicated to create a brand new VR game experience that by implementing new navigation method, that player employs lazer pointer to guide the kart to steer while driving, our game can stands out among all same type of games.

Multiplayer game mode is what I am in charging of, that by using Unity default and embeded tool - UNet, to implement our basic designs in regards to Multiplayer gameplay, that to accommodate the disparate experiences mixed up (VR and non-VR) across platforms.

The prototype is established based on two game scenes, in which from YouTube video clip, the left side is non-VR player's view, top-down sight that player can spawn game objects in 2D scene (3D for now, but it has nothing to do with VR side, since the environmental props are fully duplicated from singleplayer scene), and where right side is VR player, who can be blocked and harassed by the spawned objects placed by non VR player. VR player, as one client, in the same time, is also the host of server. Non-VR player joined the game by connecting to the server on local network. As the firewall of each device is disabled (desktop or mobile), multiplayer function works fine. Moreover, the non-VR game is implemented with UNet HLAPI (High level API), which allows developer save the time from snatching the resources online or exploit other libraries, which make the whole developing process even complicated.

Squad Goals!

