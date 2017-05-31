# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on my work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/test1.jpg "Grayscale"
[image2]: ./test_images_output/test2.jpg "Blur"
[image3]: ./test_images_output/test3.jpg "Edges"
[image4]: ./test_images_output/test4.jpg "Mask_Edges"
[image5]: ./test_images_output/test5.jpg "Lines"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

* Convert images to grayscale.
![alt text][image1]

* Apply Gaussian Blur to the image.
![alt text][image2]

* Apply Canny Edge Detection
![alt text][image3]

* Mask Edges According to Region of Interest
![alt text][image4]

* Detect Lines Using Hough Transform
![alt text][image5]

Here I am going to explain the changes I made to the drawing function.
The first step in the drawing function is to eliminate horizontal lines. A threshold is chosen and any lines that have a slope less than the threshold is treated as "horizontal" and discarded. In my implementation, the threshold is 0.4.
Next, lines are categorized as "left" or "right" based on their slopes. Positive sloped lines belong to the right lane and negative sloped lines belong to the left lane.
However, just because a line has a positive slope does not gurantee that it lines on the right lane. It could be a line on the left lane too due to image distortion, lighting or bad lane marking, and vice versa.
To overcome this issue, statistical outliers need to be eliminated from each lane.
To accomplish this, 75% and 25% quartiles of each lane is computed based on their x coordinate and then the interquartile range (IQR) is computed. If a line segment has an end point that is 1.5 times IQR outside of the 25% or 75%, it is treated as an outlier and thus is discarded from the collection of lines for each lane.
After the above procedures, we have line segments that correctly fall on left and right lane respectively.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
