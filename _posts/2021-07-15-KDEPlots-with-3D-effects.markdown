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

Once you have extracted the lines, extract the x and y coordinates of the points on the line, clip anything that goes beyond the boundaries of the pitch, redraw the lines in white. Then, shift the lines (translate along some direction in the x-y plane) and redraw the lines with a dark edgecolor. Finally, fill in the region between the contour lines (the original ones, not the shifted ones) with their respective facecolors (darker for more activity, lighter for less). The following code snippet does that. 

```tsql
for i in range(len(lines)):
    p = lines[i].get_paths()
    for j in range(len(p)):
        v = p[j].vertices
        x = v[:,0]
        x = np.where(x>100,100,np.where(x<0,0,x))
        y = v[:,1]
        y = np.where(y>100,100,np.where(y<0,0,y))
        pitch.plot(x,y,ax=ax,color='w',lw='5',zorder=i)
        for k in range(5):
            xshift = x-(k+1)/10
            yshift = y-(k+1)/10
            xshift = np.where(xshift>100,100,np.where(xshift<0,0,xshift))
            yshift = np.where(yshift>100,100,np.where(yshift<0,0,yshift))
            pitch.plot(xshift,yshift,ax=ax,color='#6f4125',zorder=i)
        ax.add_patch(Polygon(np.column_stack([x,y]),closed=True,color=vars(lines[i])['_edgecolors'][0],
                                zorder=i+1))
```
That's it. Hopefully this helps !! If you find something broken, let us know. 
