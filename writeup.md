# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

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


My pipeline consisted of 8 main steps:

 1. Obtaining an image with only white and yellow segments:
     * White color detection was done on an RGB version of the image where all channels must be larger than a threshold = 200
     * Yellow color detection was done on an HSV version of the image where only pixels that satisfy the Hue, Saturation and Value conditions were considered.
     * This was implemented in a functions named color_range()
        
 2. Obtaining a grayscale image from the white+yellow image.
 
 3. Applying Gaussian Blur filter to the grayscale image.
   
 4. Applying Canny Edge Detection:
     * The thresholds utilized were: low_threshold = 75, high_threshold = 150.
        
 5. Obtaining an image with only the region of interest of the canny image:
     * The region of interest in this case was simply a trapezoid which vertices were tuned to produce best image over multiple images and videos. The main requirement of this region is to detect the right and left lane lines. 
        
 6. Apply Hough line detection on the region of interest image:
     * The hough transform was used to detect yellow and white lane lines in the region of interest image.

The remaining two steps are how the lines were processed and drawn on an annotated version of the original image:

 7. Processing and averaging the lines:
     * A line processing function was applied to filter line into left and right lane lines. First, the slope of all detected line was calculated and based on the slope as well as the position of the lines, they were divided into right and left lane lines. After that, each of the right and left lane lines were averaged with previous values in order to better reject noise and provide better line transitions in an annotated video. This was implemented in a new function named process_lines()
        
 8. Drawing the lines:
     * A function was designed to draw the obtained lines on the original image. First, the y positions of the ends of right and left lines are pre-determined. For each line the first y-axis end would be in the bottom of the image while the other is curently set at approximately one third of the image's height. Using the y positions and the right and left line parameters obtained from step 7 (slope and intercept), the equivalent x position of the line ends are calculated and one right and one left line are created and added to the original image. 

The below pictures are examples on the perfomance of the developed code: 

![Alt text](./test_images_output/solidWhiteRight.jpg?raw=true "solidWhiteRight")

![Alt text](./test_images_output/solidYellowCurve.jpg?raw=true "SolidYellowCurve")


### 2. Identify potential shortcomings with your current pipeline

One of the shortcomings of my current pipeline is that the y values of the ends of the extrapolated right and left lines are kept constant and rather the x parameters of the ends are changed. It would be better if both x and y parameters changed dynamically. Also, a lot of fixed parameters were used in steps like: canny edge detection, hough line detection and region masking. It would be better if these parameters changed dynamically to ensure better performance and lane detection quality.

Another shortcoming is that the current pipeline only works with straight lines and the results under curved lane lines and other shapes are not guaranteed.

### 3. Suggest possible improvements to your pipeline

As stated above, in order to address the shortcomings of the current pipeline:
* Dynamic and adaptive tuning of the parameters used in: canny, hough, region masking and line drawing segments of the pipeline.
* Finding and applying methods of detecting curved lines and other geometric shapes that can be of benefit to the system.

