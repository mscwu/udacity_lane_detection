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
[image6]: ./test_images_output/test6.jpg "Masked_Lines"
[image7]: ./test_images_output/test7.jpg "Result"

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
The next step is simple, fit a straight line using all the line segments collected before and draw the line from the bottom to the top. 

* Mask the Straight Line Again Using Region of Interest
![alt text][image6]
The reason to mask the image after the line is drawn is that in this way, our pipe line is independent of the image size. A line is always drawn based on the lanes detected and the region of interest is applied on a case by case basis.

* Overlay the Detected Lane with the Original Image
![alt text][image7]

### 2. Identify potential shortcomings with your current pipeline
There are some shortcoming in this pipeline.  
* This pipeline is not suitable for curvy roads.
Since only a 1st order line if fitted instead of splines, the underlying assumption is that the lane is straight or the car is on a highway with very large turining radius such that the lane can be approximated as a straigh lane. With local roads that is not regular or even roundabouts, the pipeline will be compromised.

* This pipeline is not suitable for roads with cracks between lanes and bad markings.
The pipeline treat edges within the region of interest as lane. Even though some filtering is applied to make the pipeline more robust, a crack parallel to the lane marking or very vague lane marking is beyond the capability of the algorithm.


### 3. Suggest possible improvements to your pipeline

* Preprocess the Image
It is possible to tune the image, for example, increase contrast, adjust exposure and brightness, shadows, etc such that the image is more friendly for lane detection.

* Running Average of Previous Lanes
Since we don't expect sharp change of lane directions in general, it will be helpful is the lane detected from previous frame is memorized and is weighted into the calculation of the next frame. Two benefits of this will be, in case where a lane is failded to be detected, previoud lane is used until the next available lane is detectec. Second, the lane transition will be smoothed from frame to frame.
