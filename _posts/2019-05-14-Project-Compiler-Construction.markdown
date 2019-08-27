---
layout: post
title:  "Compiler Construction: Source-to-source Compiler in Clojure"
date:   2019-05-14 19:34:06
author: Zizhun Guo
category: Projects
categories: jekyll update
---
[Outline]

Accomplished using Clojure to implement a compiler of a Haskell-type language Alto, of which the code generated applys in Java Virtual Machine. 

Implemented a LL(1) parser that constructed with two stacks structure for semantic analysis, and 
first and follow set for tree generation. 

Optimized the declaration tree in type checking step by transform into a new tree to simplification, i.e. reduced the number of forms, annotate calls and tail calls.


LL(1) parser (generator), which is implemented with First set and followset optimized with curried functions. 



[Images from assignment instruction specificlly of the larger Grammar extension]



为什么compiler？

希望学习编译器构造来深入了解编程语言，了解Lisp语言来学习命令式编程思想(通过Clojure:1 JVM可编译 2. 更简洁he condense； 3.是Lisp方言)。

...

