---
title: RTR - 05. Shading Basics
date: 2019-07-07 22:06:47
tags: Real Time Rendering
categories: Real Time Rendering
---
#### Determining the appearance of a rendered object:
1. choose a shading model
2. set the values of properties used to control appearance

---

#### factor the shading model into lit and unlit parts
$ c_{shaded} = f_{unlit}(\vec n, \vec v)+ \displaystyle\sum_{i=1}^n c_{light_i}f_{lit}(\vec l_i, \vec n, \vec v)  ( c_{light}$: light color )  $
 




Gooch shading model: 向光暖色调，背光冷色调

---


