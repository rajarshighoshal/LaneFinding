# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[image1]: ./examples/laneLines_thirdPass.jpg "Target Image"
[image2]: ./test_images/solidWhiteRight.jpg "Starting Image"
[image3]: ./examples/line-segments-example.jpg "Intermediate Image"

---

### Reflection

### 1. Descrption of pipeline
I started with an image like this :- 

![alt text][image2]

My pipeline consisted of 6 steps. These steps are :- 
1. Applying Gaussian Blur on the image to clear out noises
2. Applying colour threshold to get only relevant colurs which could be lane line colours and clear out everything else
3. Applying Cann edge detecting algorithm to detect stark contrasts on the image
4. Applying region of interest so that we are dealing with only the area where there is the possibility to find lane ine
5. Applying Hough Transformation to the image so that it can convert individual points in the image to line.
6. Applying all thses above transformations step by step on the origional image and then use the created image to overlap on the original image to create masked version of the starting image.

After All this operations the intermeiate image looks like this :- 

![alt text][image3]

I needed to change the `draw_lines` function in the following ways to get persistant lines which cover the whole road:-
* Eliminate line if its angle magnitue to the base is more that 70 degrees or less than 30 degrees . These numbers are derived based on both left and right lanes are going to meet at vanishing point.
* The line points are categorized into left or right based on angle direction. If it is positive it belongs to left lane, if it is negative it belongs to right lane.
* I found multiple candidates for both left and right lanes. I selected one line from each category based on the minimum angle difference between pair of lines.
* I used line pair to extrapolate the line markings by finding the line intercepts and vanishing gradient points.

After applying all thses operations final image looks something like this :-

![alt text][image1]




### 2. Potential shortcomings of the pipeline

* It can only detect lanes in staright line at this point.
* It is highly dependent on the position and colour of the images as a result will only work with the weather exactly like the example images.
* Also, as it has high position dependence this won't work in case the road looks differnt than that of example image.
* The algotihm spectacularly fails when there is a relatively sharp trun, it only works with very good staright roads.
* It also won't work if there are much more potholes and scratches present in the road.


### 3. Possible improvements 

* One possibility is to converting the whole image into HSV space and then applying the algorithms and figuring out parameters.

