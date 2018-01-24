# **Finding Lane Lines on the Road** 

## Udacity Self-Driving Car Engineer Nanodegree. Project 1

---

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the work in a written report

---
## Images below shows the result of finding lane lines:

Origin image example | Modified image example
------------ | -------------
![Origin image example](/examples/solidWhiteCurve.jpg) | ![Modified image example](/examples/solidWhiteCurve_final.jpg)

---
## And this is how it works:

First I need to stangartize input images. To do that, I resize image to the size of 540x960
A lane can be drawn in any color, so I convert image to grayscale. Then I apply Gaussian blur for suppressing noise and spurious gradients by averaging. 

![Gray image example](/examples/solidWhiteCurve_gray.jpg)

The way to find edges in computer vision is finding a gradient for every pixel, so if a color of a given pixel changes too fast, this pixel is probably a part of edge. To apply this technique, I use Canny edge detection algorithm. 

![Canny image example](/examples/solidWhiteCurve_canny.jpg)

Before considering which edges are lane lines, I need to "select" a polygonial region in front of the camera, where lane lines often are. 

![Masked image example](/examples/solidWhiteCurve_masked.jpg)

Inside this area it is easy to find strict lines, and Hough Transform is helpful here.

![Hough Transform image example](/examples/solidWhiteCurve_lines.jpg)

At this point I found all possible lines in the selected region. And in order to draw a single line on the left and right lanes, I modified the draw_lines() function this way:
1. Found all lines which are supposed to be a left lane line. 
2. Found all lines which are supposed to be a right lane line. 
3. For both set of lines I found the mean of the lowest X-value and the mean of the highest X-value. Having Y-coordinates for those points, I can draw one solid line between them.

![Solid_line image example](/examples/solidWhiteCurve_final_lines.jpg)

For the 3rd, challenging video, I made some changes in the function which draws one solid line. I cut the rectangle between the hindrance at the bottom and the bending point of the lines. It seems like it works... for the most parts of the video.

---
## List of fixed shortcomings:

- ~~**The lanes sometimes disappear, especially if origin lines are short (blinks).**~~ Solved by caching previous lane.
- ~~**Sometimes (in the 3rd video), it draws line like this..**~~

![Bad line example](/examples/bad_line.jpg)

Solved by measuring slope of left and right lanes. If the slope of a new line differs too much from previous ones, use cached line.

---

### 2. Potential shortcomings with the current pipeline

The pipeline works fine for most parts of videos, but there are some issues:
1. When the border casts a shadow line biases a little.

---

### 3. Possible improvements 

Smoothen brighteness so shadows will not effect on edge detection
