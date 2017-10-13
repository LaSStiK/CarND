# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted next steps:

I convert image to gray.
Then I set gaussian blur. Apply gradient using Canny and after select area of interest using 4 angels area.
After all i find lines with hough method and draw lines on original image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by adding next steps:
For each line I calculate Rase and Slope coefficients. Then I split lines with positive and negative Slopes to two groups. In each group I calculate avarage Slope and Run coeficients. Then using formulaY = Slope*X + Raise I fixed to points by Y: bottom of image and middle and calculate X for points in both groups. 


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there is more than 2 lane lines in focus area. 

Another shortcoming could be when we face stopline. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to to sort out some lines. For example horisontal on
