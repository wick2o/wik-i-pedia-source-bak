---
title: "A Shapeoko Vacuum Table Design"
date: 2014-10-10
tags:
  - howto
  - general
---

I was trying to cut out some shapes from some 1/16" thick acrylic the other day when after a few cuts the material seemed to be lifted by the drill.  The cutout part would also start to move as the drill reached the end of its travel. This is when it hit me, having a vacuum table would sure be nice!

<!--more-->

**The Design**

I'm not much of a master carpenter so I decided to use my shapeoko to make itself a table rather then cutting out all the parts and properly gluing and clamping them together.  I knew it may take longer, but it was easier for me to design and then mill and then just glue a base on later.  

Using some 3d software I created a model of what I wanted it to look like and made sure all my measurements were corrected.

**The Machining**

After exporting the model to a STEP file format I was able to import it into some CAD/CAM software called [HeeksCAD](https://sites.google.com/site/heekscad/). This is where one would specify the drill sizes, pockets, and paths to machine.  It then generates Gcode that the shapeoko can understand.

**The Results**

The finished results work out great, I am now able to clamp down on my material without worry of the middle bowing, or flexing while engraving.  I'm using a shopvac as my air sucking source, so depending on the size of your shopvac your mileage may vary.
All files can be viewed on my github page (including a stl file that you can zoom, pan, and rotate) [here](https://github.com/wick2o/shapeoko/tree/master/vacuum%20table).

