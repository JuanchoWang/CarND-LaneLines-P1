# **Finding Lane Lines on the Road** 

## My Writeup for Submission

### Xiao Wang, 02.04.2020

---

**Updated on 02.04.2020**
First writeup finished.

[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve.jpg

[image2]: ./test_images_output/solidYellowLeft.jpg

---

### Reflection

### 1. Pipeline Implementation

My pipeline to process a single image consisted of 5 steps. 
* First, RGB image is converted to grayscale.
* Then a gaussian blur is applied to the gray image and the edges are detected by a Canny Detector.
* Next, a trapezoid-shape ROI is determined as a mask. The two lower vertices are the bottom-left and -right corners and the upper vertices are determined by image scale multiplied by offset factors.
* Afterwards, hough transform is utilized in the ROI to provide lots of line segments.
* At last, those line segments are averaged and extrapolated to determine two lane lines

Funtion draw_lins() is modified a lot. The line segments given by hough transform are divided into two groups, left and right, according to their slopes. In each group, the end points of all line segments are considered as scatter points and a fitted line is estimated with linear regression using np.polyfit(). This line reaches the boundary of given ROI.

![alt text][image1]

This pipeline works well in the first short video. In the second video, abnormal slopes of lane lines can be seen in some inconsecutive frames. The performance on the challenge video is quite bad.

### 2. Shortcomings

**In good cases like the first two videos**
* The edges of far dashed lane line can not always be well detected. Due to missing the relevant line segments, the fitted line is more impacted by the closer segments, which causes a slightly wrong slope.
See image below.

![alt text][image2]

* if the closer parts of dashed line are mostly dots or short marking segments, this lane will be hardly detected or with bad performance.

**In tough case like the challenge video**
* The lane has more curvation and shall be also a fitted curve.
* The shadows, stains and reflective dots should be considered as noise and suppressed.

### 3. Suggest possible improvements to your pipeline

**considerable improvements**
* The parameter tuning is still and always a problem. How to select the best set of parameters deserves more work. It is also helpful if the parameters can be systematically managed.
* A dynamic ROI is needed if the ego car is turning or driving in a curve. Anyway, the camera and car calibration data must be introduced in the pipeline.

**compromised improvements**
* A manually set threshold for slopes of line segments might lead to better performance but may not apply to real world or more data. Discussion necessary.  