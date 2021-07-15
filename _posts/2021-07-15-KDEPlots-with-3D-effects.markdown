---
title: "First Blog Post of the Group - Creating some 3D effects on kdeplots."
layout: post
---

Have you ever looked at the works of Owen Phillips, Tony El Habr and Hugh Klein and wondered : how to re-create their heatmaps that come with a 3D effect ? I did for sure. Tony uses R, and I wanted something for my fellow Pythonistas. And of course, the EUROs brought forth the running joke about "Temple of xG" and how long-range bangers are demolishing it. So, I created this ![Image](https://Friends-of-Scraping.github.io/images/Example1.png).

In this post, I will briefly describe how I created this. As for acknowldegments, apart from the above 3 whose heatmaps I drew the inspiration from, I have to thank my flatmate Yi Xue Chong for discussing this, bouncing a couple of ideas back and forth and finally coming up with this technique. 

Firstly, you want to create the kdeplot and extract the lines. The following can grab the lines for you.

```tsql
my_kde = sns.kdeplot(x=x, y=y, weights=xg, ax=ax,cmap=cmap, levels=7)
lines = my_kde.collections
```
