# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project I have detected lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  


The Project
---

## Approach
The main aim of the project is to detect road lane markings. I followed below sequence of steps to build the pipeline:
* Noise removal
* Lane mask generation
* Edge detection
* Hough line detection
* Extrapolate lane markings
* Repeat above steps on each image sequence

### Noise removal
To remove sharp noise in image, 3x3 Gaussian kernel smoothing was applied to image. I have experimented with 5x5 kernel but it deteriorated the performance of lane mask generation and edge detection steps.


### Edge detection
I have used Canny edge detection algorithm with (20, 100) threshold values over lane binary mask. 
I ended up using below parameters for edge detection.
```python 
    low_threshold = 30
    high_threshold = 100
```

### Hough line detection
I have applied probabilistic hough transform algorithm on edge map to detect the line shapes. I have experimented with theta, minimum line size, rho and max line gap and threshold values. The hough vote count(threshold) and minimum line size values played key role in removing outliers. I ended up using below parameters for line detection.

```python 
    rho = 1
    theta = np.pi/180
    threshold = 60
    min_line_len = 30
    max_line_gap = 150
```

## Extrapolate lane markings
The Hough line detection algorithm helped to remove lot of edge outliers but produced lot of line points. In ideal world we would expect two line points which represents lane points. Now the challenge is to pick up best two line points which represent the lanes. I came up with following algorithm to detect best line pair which represents lane markings. From car camera point of view left and right lanes show up as parallel lanes which are going to meet at vanishing point.
- Eliminate line if its angle magnitue to the base is more that 70 degrees or less than 30 degrees . These numbers are derived based on both left and right lanes are going to meet at vanishing point.
- The line points are categorized into left or right based on angle direction. If it is positive it belongs to left lane, if it is right it belongs to right lane.
- I found multiple candidates for both left and right lanes. I selected one line from each category based on the minimum angle difference between pair of lines.
- I used line pair to extrapolate the line markings by finding the line intercepts and vanishing gradient points. 

