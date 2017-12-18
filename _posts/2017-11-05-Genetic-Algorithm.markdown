---
title: "Genetic Algorithm"
layout: post
date: 2017-11-15 16:36
image: /assets/images/markdown.jpg
headerImage: true
projects: true
tag:
- projects
category: project
author: zizhguo
---

# 1.	Your creature implementation details
## 1.1	Creature and obstacles
In this giant cubic space, there would be couples of asteroids floating and spinning (See figure1.2). There would be 10 spaceships (See also figure1.2) from current population as assigned creatures who would be spawned at center bottom of cubic scene at beginning of each simulation. Each spaceship has a single chromosome which contains 10 genes whereas each gene represents a specific location as form of Vector3 implemented in Unity. (See figure1.1)
![Illustration Image]({{site.url}}/assets/images/GA/01.png)
![Illustration Image]({{site.url}}/assets/images/GA/02.png)
![Illustration Image]({{site.url}}/assets/images/GA/03.png)

---

# 2.	The outcomes of what you did
When program starts, the spaceships spawn from the center point on the bottom and then follow paths their chromosome regulates. As they going cross and zigzagging upwards, some spaceships clashes when they hit on the asteroids. The lasting time for them end.
## 2.1 Simulation mechanism
A complete circulation of simulation demands each spaceship go through each position which is represented by each gene its chromosome has. For a single circulation, ten spaceships have ten unique and randomly generated chromosomes. The navigation speed for each spaceship is same, so it would be cost more time for whom has longer distance to go through. Counter would be initiated only if its master starts to navigate, and it ends by ship clashing or reach the top end layer. As long as all spaceships end navigation, the simulation progress ends, which means this time, circulation ends too.
![Illustration Image]({{site.url}}/assets/images/GA/04.png)
![Illustration Image]({{site.url}}/assets/images/GA/05.png)

---
# 3.	The technical details of what I did
## 3.1 Finite State Machine
To avoid cross-running between simulation and iteration, FSM is involved. It can be only one state running each time, and states machine codes are structured at main Update() function which runs every frame. Since there are only two states necessary, so FSM is declared as Enum rather than abstract class.
![Illustration Image]({{site.url}}/assets/images/GA/06.png)
![Illustration Image]({{site.url}}/assets/images/GA/07.png)

## 3.2 Assembly line
While at iteration state, GA process has several steps to run through: Fitness, selection, crossover and mutation, so each step can only be running after the former one got done. (All function manipulates the value of chromosome which maintain single instance). So I involved amount of boolen flags which matches the amount of step’s function to assembly control the order running steps.
![Illustration Image]({{site.url}}/assets/images/GA/08.png)
![Illustration Image]({{site.url}}/assets/images/GA/09.png)

## 3.3 ECS vs OOP?
Since the main goal of this GA program is to implement Darwin’s concept of evolution, so from the perspective of scripts, I didn’t make much specific scripts which means to work for each spaceship based on the thought of ECS (Entity Component System). 
So I have a script named as Motor, which controls all spaceship motor system. 
The only script I attach on each spaceship is a script called CollisionTest (ignore the later “Test”, I didn’t change name back, my bad), because The collision detection for game object are regulated, so it can only work for single game object.

## 3.4 Decoupling
For each step of iteration function, the manipulation is onto Vector3 scope, so that decoupling each minor steps is necessary. It’s not only easy to debug, but also would not be sacrificing too much performance. 
![Illustration Image]({{site.url}}/assets/images/GA/10.png)

## 3.5 GA implementation:
### 3.51 Fitness function
![Illustration Image]({{site.url}}/assets/images/GA/11.png)

t(i)  represents survival time for each spaceship; P(i) represents its probability to be selected as parent; C(i) represents accumulated for Random.NextDouble();
### 3.52 Selection
By comparing the generated 10 random number that are ranged from 0 to 1, the index of selected chromosome would be recorded if its value is greater less than C(i).
### 3.53 Crossover
Circulate to crossover selected parents. The amount of parents is determined by parameter called crossover rate ∂. 
If the random number’s value less than ∂, the corresponding index would be recorded, which means this chromosome is been selected.
![Illustration Image]({{site.url}}/assets/images/GA/12.png)
Randomly generate same amount of cut-off parameter with its value ranged from [1, length - 1] to determine where to cut chromosome into two pieces to do crossover.
Assign the front part and back part to two new lists or two-dimensional arrays (I used arrays because just for less debugging problem, however, it may have low performance because of for-loop assignment).
For example:
Condition: cut-off parameter C(0)=1; (it means keep chromosome[0] first gene[0], and attach last 9 genes from chromosome[4] on the back)
![Illustration Image]({{site.url}}/assets/images/GA/13.png)
### 3.54 Mutation
There’s a parameter called mutation rate which determines how many the mutation genes are. If the new generated Vector3 gene would replace the gene whose y value are matched, because for each chromosome, it contains genes which have disparate value of y due to the layer system of program.
![Illustration Image]({{site.url}}/assets/images/GA/14.png)
---
# 4.Any problems you had with the assignment
## Problem: Evaluation of Fitness 
The longer time the spaceship survives, the higher chance it would be selected as parent. So time is essential for evaluation.  I once thought about whether involves positions as well, but positions are genes which should not be mixed with fitness parameter.
## Problem: Code structure
The structure I implemented is not best one, and somehow looks weird. It did not purely follow OOP or ECS or any other design mode, which needs to be re-constructed if I have time.
The goal is to optimize the performance, reduce codes redundancy and improve scalability.
## Problem: Object pooling
For the early staged test, I didn’t use pooling, so as program went on, I just let it slide. The problem causes my reference code part at beginning is really redundant, I need to drag all game objects to Inspect Window to reference it, which is bad. 
Dynamic loading the game object would be better, and with objects pooling, the program would not be lagging by the reason of multiple instantiate and destroy game objects. That being said, I didn’t instantiate or destroy any game objects at every iteration. In contrast, I active and de-active them.
## Problem: Decoupling the procedure
Since some manipulation on gene are involving generate and replace the value of Vector, the algorithm apply for it should be very accurate and easy-mistake-making. I used List<Vector3>[] as data type to hold a group of chromosomes, so it would be little complicated to get one Vector3 value from it. 
To be honest, my program is delicate, since a simple index mistake can make whole program pause or crash sometimes, precision is much more important than any other technique in this program. Especially when it comes to combine front segment of gene with back segment of gene. A simple value mistake can cause the spaceship which attached on stock in the air, so the its flag that indicate its simulation end would not be set to true, leading the whole program stock. And the weird floating scene is the most common scene I’ve ever seen during my whole debugging time. 
