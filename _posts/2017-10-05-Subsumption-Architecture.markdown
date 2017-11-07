---
title: "Subsumption Architecture"
layout: post
date: 2017-10-05 16:36
image: /assets/header/forest.jpg
headerImage: true
projects: true
tag:
- projects
category: project
author: zizhguo
---

<h1>Describtion</h1>

## Environment

A simulation island, where there are low poly trees and grasses, rocks(Obstacles), wolfs (whose name is wolfie, a duplicable aggressive predator, the main character).

---

## Sensory

For those Chicken(Food) and Wolves(Character), sensory system is built under ObjectsPooler, by using For Loop to traverse all objects (which being activated in Hierarchy) to get relations between game objects.

---

## Subsumption Architecture

![Illustration Image]( {{site.url}}/assets/images/subsumption/sub-arc.png)


Layer 0: Basic sonar sensory system that implement obstacle avoidance(behavoir) function  to wolf. 
Layer 1: Wander  behavior added. Wander behavior can subsume avoidance behavior.
Layer 2: By traversing objects pooler, if distance between wolf and chicken less than a number, the seek behavior would inhibit wander behavior, (wolf seek to chicken). If there’s another wolf existing within the trigger distance, decider would turn wolf into combat behavior by inhibiting seek behavior and wander behavior.
Layer 3: Health system added. If the health portion of wolf is under 30, health would inhibit all other behaviors except avoidance behavior, and turn wolf into flee behavior, which would subsume Layer 0. If there’s no enemy (HP higher than him), Flee behavior turn to recover model. 

## Steering Control

---
### Wander
 

<div class="side-by-side">
    
    <div class="toleft">
	![Illustration Image]({{site.url}}/assets/images/subsumption/wander.PNG)
        
    <figcaption class="caption"> by Zizhun</figcaption>
    </div>

<div class="toright">
        <p>By using Vector.Reflect() function from Unity.Engine Library can character avoid obstacle with raycast the obstacle that is in the front direction of character.</p>
    </div>
</div>

### Avoidance

<div class="side-by-side">
    <div class="toleft">
        <a href="{{site.url}}/assets/images/subsumption/reflect.PNG"><img src="{{site.url}}/assets/images/subsumption/reflect.PNG" alt="image">
</a>
        <figcaption class="caption">from Unity Manual</figcaption>
    </div>
<div class="toright">
        <p>Create a circle parallel to ground surface in front of character. 
Randomize the position of pointer object, and Vector that starts from character to pointer object would be created and and truncation and direction of Vector would be using for create the acceleration and moving direction.</p>
    </div>
</div>

### Seek

Straight up going forward to the target.
4.Flee, by following the flee behavior Vector, that the direction of Vector starts from target enemy to character itself.

---

## Architecture works for multiple animals

Each script attached on animal are working for the animal where it attached, because they all inherited from Monobehavior Class.

## Pros and Cons

Pros:
1.	According to the subsumption thesis from MIT, this architecture is firstly created to serve for robot system. Any response to behavior changing are all made by robot itself, which sometimes is different than game development, because scripts can be attached to other game objects rather than character itself to implement the same function. 
So, all scripts are part of robot making the complete system only serve for character itself. 
2.	Unlike finite state machine, subsumption architecture can make behaviors subsumes together. Like design integrated circuit I used to, I made  each behavior script have a specific variable that only has two value, 1 and 0. And I will put all those values (digits)on a single binary variable, which defines how character’s motor does.  
It has 6 digits, each digit represent a specific behavior. If wander behavior is activated, the digit that represent wander behavior would be value of 1, so motorCodes would be 01 00 00, which equals to 16. If Avoid behavior is activated, the digit that represent avoidance behavior would be value of 1, so motorCodes would be 10 00 00, which equals to 32.
Then comparing the equal of motorCodes to 2, 4, 8, 16, 32 to determine which motor behavior would be used to control character to move as that behavior.
Cons:
As long as the behaviors getting complex and multiple, the relationship become tough to handle, because there would be more situation could be possibly happen. Things would make motorCodes equal to some other value like 01 10 00, which there’s no specific behavior apply to this value. 
And it is hard to extend programs to be more complex. Adding one more behavior need re-design the architecture, and re-build new relation to old behaviors, which cost a lot times.

---

## Problems

### Unity Engine:
Learn new things like, the difference between collider and trigger. NEW function, Vector.Reflect(). Handle more situation evolving multiple colliders by assigning to specific tag or layer. More stuff about math (review those high school math knowledge again). Codes structure.

### Subsumption:
Now my codes only works for specific state changing (not technically, it is still subsumption architecture because each scripts works for separate function and runs since the program starts), like 10 00 00, 01 00 00, 00 10 00. Certainly I will make it subsumed on codes, like 11 00 00 ,which represent wander but also avoid obstacles, or 10 11 00, obstacles avoidance and found the chicken and attack the enemy that close to chicken.
