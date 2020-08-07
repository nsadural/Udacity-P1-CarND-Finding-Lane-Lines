# **Finding Lane Lines on the Road** 

## Project Writeup

### Nikko Sadural

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline for finding road lane lines in an imported image/video consists of the following steps:

1. Initiate the lane-finding process using the process_image() function.
2. Convert the image from color to grayscale using the grayscale() function.
3. Apply a Gaussian filter for smoothing using the gaussian_blur() function.
4. Apply the Canny transform to detect edges (potential lane lines) using the canny() function.
5. Define a quadrilateral mask containing lane lines an using the region_of_interest() function.
6. Apply the Hough transform to detect points that construct lines of similar slope (potential lane lines) from the Canny transform output using the hough_lines() function.
7. Draw a single line on the left and right lanes using the draw_lines() function.

I made several additions to the draw_lines() function to draw single lines that map out the left and right lanes. First, I initialized variables that would contain (x,y) coordinate values for line endpoints detected from the Hough transform. I also initiated variables to hold the values of the running total of slopes for detected line segments and variables to count the number of line segment loop iterations. For each detected line segment, I calculated the slope using two given points to determine if the line corresponds with the left lane (negative slope) or the right lane (positive slope). To avoid incorporating any unwanted points within the quadrilateral region of interest that are not lane lines, I included logical conditions that positive slopes must be between 0.55 and 0.9 and that negative slopes must be between -0.9 and -0.55. I used nested if statements to store values of the endpoints for the detected lane lines. I also calculated the running total of slopes and incremented the counters for each iteration of the loop.

The average positive and negative slopes were calculated using the respective running total of slopes and the loop counters. Then the slope formula uses the average slopes and detected lane endpoints to calculate new extrapolated endpoints that contain the left and right lane lines. Finally, the new endpoints are drawn onto the original image. For imported videos, this is repeated for each image frame throughout the video.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming for this pipeline is the inability to detect curved lane lines. Another potential shortcoming would be the inability to distinguish lane lines from an image of driving over a hill with objects in the background. Edges detected from this image may not be filtered out in the calculation of slopes and endpoints. One other shortcoming is the inability to account for other vehicles or obstructions in the image directly in the vehicle path.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to include a better line extrapolation method that does not change as rapidly as the current method. For dashed and dotted lane lines, the detected endpoints are changing rapidly through a series of video frames, which results in a drawn line that is oscillating in place. Another possible improvement would be to project the drawn line up until the end of the visible straight lane lines rather than up to a predetermined drawn line length using the mask vertices.
