---
title: "BeatBlade - Beats Detection Algorithm"
layout: post
date: 2017-5-05 16:36
image: /assets/header/forest.jpg
headerImage: true
projects: true
tag:
- projects
category: project
author: zizhguo
---

Beatblade is an semester-long independent study project, it was all built based on an orintal VR action game (Blade).

I was in charged for implementing beat detection algorithm into the game, so that to create a music game mode that player need to slash the enemy (to eliminate the enemy) by follow the beats of the background music (to get high score). 

The same type of musical game can be found everyone, yet the majority of them with its level created by so-called mapper, do not have a auto beats detection system: using codes to detect the background music before running the game, in order to create levels aligning with tempo.

Such beats detection needs the codes not only can be ran in real time, but to have an ability to process stream of data behind each music file. Since it was my first time to actually implement my undergrate knowledge - signal processing, into the actual project, I determined to set the goal that the program can process WAV file and get result data stream which can be transferred in Unity 3D.

The first step is to find a library to decode audio file, so I chose [NAudio](https://naudio.codeplex.com/), an open source .NET audio and MIDI library, to handle this task. It performs well in .Net project, and it can produced tens of millions of voltage infos that represents the analog audio signals. With simple calculation with the sampling rate (such as 44100 HZ), and duration of the music file, it results the supposed amount of data. Then, based on the papar called [Beat Detection Algorithems](http://www.flipcode.com/misc/BeatDetectionAlgorithms.pdf), by comparing instant sound energy with average sound energy, the codes can output an array made of 1 and 0 (when it goes to 1, it means beats, and 0 means there's no beat whatsoever).

Here are the 5 experiments of VLogs that present the project development.

<iframe width="560" height="315" src="https://www.youtube.com/embed/GIWE0LQS_ew" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/BowUaL8DYlw" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/AdEK7cX3Tm4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/7vqLfrV_JA0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/v8Jw8rfWWIM" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>