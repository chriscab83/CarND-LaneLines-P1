# **Finding Lane Lines on the Road** 

## Writeup

### Christopher Cabrera - 06/03/2017

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.  I began by converting the image to grayscale. I then took the grayscaled image and added a gaussian blur to help in edge detection. After adding the blur, I put the image through OpenCV's Canny edge detection which returned an image of highlighted edges. I then took the image of edges and masked it with a four-sided polygonal area of interest in the area lane lines would likely be found so the resulting image would only contain the edges in that area. With the masked Canny edge image, I applied OpenCV's Hough Transform to get line segments as an array of coordinates indicating pixel coordinates to the begginning and end of each line detected.  From there, I extrapolated those line segments into a single line for the left and a single line for the right lane lines that spanned the entirety of my region of interest.  Lastly, I drew the weighted single left and right lane lines on top of the original image to show the results.

To extrapolate the single lane lines from the line segments, I first seperated the individual line segments into left and right lane lines using the slope of the found line.  Once I categorized each line, I used numpys polyfit method to extract the slope and y-intercept from the known points. Then, with simple algebra, I found the missing x-coordinates for each line to properly plot my lines. To help slightly reduce some noise, I added a complimentary filter to the slope and y-intercept to use the sum of 15% of the last frames values and 85% of the new calculations. 

Using this pipeline and strategies, I found that I was able to draw a relatively smooth line for all of the test images as well as the two test videos in the project.


### 2. Identify potential shortcomings with your current pipeline


Although my pipeline worked well for relatively straight lines, this method would definitely not work on winding roads, or even roads with a large turn.  The algorithm would break down in the use of linear regression as you would only be getting straight line results. Also, my algorithm would have a difficult time distinguishing between the left and right lane lines once the road started to turn as it is relying entirely on the slope of the lines to categorize them into their proper side. As that slope changes past expected values for a straight road the pipeline would no long know which side of the lane the line belongs to. Lastly, the static variables used for the various transforms, filters, and masks were highly tweaked for the specific situations found in the test images and test videos. These static variables would likely not work in many other situations, road conditions, lighting, etc.

### 3. Suggest possible improvements to your pipeline

To improve my pipeline, I could possibly use a higher degree of polynomial regression to find a curved line to fit the found points for each lane line. Also, it would be beneficial to build a smarter pipeline that could change its variables for the transforms, filters, and region of interest mask based on the current road conditions. Dynamic variables for these things would improve the pipelines performance by keeping it from being pigeonhold based on just the examples we optimized the variables for. Lastly, we would need to find a new way to categorize the lines into right and left lane lines because the slope method would not work in most real world conditions.
