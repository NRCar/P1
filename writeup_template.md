# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps:
For each image to be processed.

<img src="https://github.com/NRCar/P1/blob/master/stages/0_solidWhiteCurve.jpg" height="270" width="480" />

First, I converted the images to grayscale and applied a guassian blur with the kernel size of 5 .. 
This results in a relatively noise free grayscale image that is ready for the next steps.

<img src="https://github.com/NRCar/P1/blob/master/stages/1_fixed_gray.jpg" height="270" width="480" />

Then I applied the canny edge detection function with a low threshold of 100 and a high threshold of 200 with gives us
the edges on the image.

<img src="https://github.com/NRCar/P1/blob/master/stages/2_canny_edges.jpg" height="270" width="480" />

Then we select a polygon to mark a region of interest that helps us narrow down where the lines are likely to be.
I picked the Quadrilateral with the vertices (50, 550), (415, 330), (590, 330), (920, 550), using guesswork and then adjusting.
This would give us somelike like:

<img src="https://github.com/NRCar/P1/blob/master/stages/3_region.jpg" height="270" width="480" />

Then we use the Hough transform to detect the lines or line segments that mark the road.
This results in a segmented line. The parameters used are :
* rho = 1 # distance resolution in pixels of the Hough grid
* theta = np.pi/180 # angular resolution in radians of the Hough grid
* threshold = 20     # minimum number of votes (intersections in Hough grid cell)
* min_line_len = 10 #minimum number of pixels making up a line
* max_line_gap = 40    # maximum gap in pixels between connectable line segments

In order to draw a single line on the left and right lanes and mark the road in *green* , I modified the draw_lines() function by 
First sorting the lines detected by the hough transform into those that are likely to be in the left and right lanes based on the 
sign of the slope. We also take care to ignore lines that seem horizontal, i.e with slope m such that -0.08 <= m <= 0.08.

Then we call a new function "fit_line" to generate a linear fit line.
We then return the points of intersection of theses liner fit lines with the top and bottom of the image and draw lines joining them.
We also fill region inside in *green*

<img src="https://github.com/NRCar/P1/blob/master/stages/4_hough_lines_poly_fit.jpg" height="270" width="480" />

After them we again use the same region of interest as before (which we used to narrow down the lanes) to clear out the rest of the lines. This when combined with the original image gives us the lane detected image

<img src="https://github.com/NRCar/P1/blob/master/stages/5_solidWhiteCurve_procesed.jpg" height="270" width="480" />


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
