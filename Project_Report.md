# **Finding Lane Lines on the Road** 

## Overview 

This short report describes the used pipeline to find lane lanes on the road. The results of the images that were used to test the pipeline will be shown here. Since it's not possible to stream videos on a markdown file I invite you to check the HTML file, jupyter notebook of the project or the "test_videos_output" folder to see the annotated videos. A discussion of the sortcomings and possible improvement will also be discussed. Enjoy reading :)




[//]: # (Image References)
[image1]: ./test_images_output/grayscale.jpg "Grayscale"
[image2]: ./test_images_output/blur.jpg "Gaussian Blur"
[image3]: ./test_images_output/canny.jpg "Canny edges"
[image4]: ./test_images_output/roi.jpg "Region of interest"
[image5]: ./test_images_output/hough.jpg "Hough lines"
[image6]: ./test_images_output/whiteCarLaneSwitch.jpg "Final result"


### 1. Describing The Pipeline

The pipeline consists of 6 steps. First, the images are converted to grayscale as a preparation for the following steps.
![alt text][image1]
The second step is the use of guassian blur to smooth the images in order to make the edge detection more efficient. 
![alt text][image2]
Next we apply the Canny edges detection which is a function that uses the gradient to detect the swift changes in the brightness and we end up with a black image with the detected edges in white.
![alt text][image3]
In order to distinguish the lines from other shapes of edges we use the hough function. This function is based on the hough space where the lines are represented by points and points by lines. The groups of lines in hough space that intersect in one point represent the pixels (points) of the image that constitute a line. The line detection is only applied on a region of interest using an image mask. Since the camera is supposed to be fixed on the car, we can predetermine the region of the image where the lane lines are supposed to be. 
![alt text][image4]
![alt text][image5]


The lane lines now are detected, the last step is to draw them on the raw images. To do that, I used the signs of the slope of hough lines to separate them in two groups : right lane line and left lane line. After that, I calculated the average of slopes and intercepts for each group and draw solid lines using these average values. The result is shown in the next image (more images in the HTML and test_image_output folder) 
![alt text][image6]
The next step was to test the pipeline on videos, this is done by applying the same pipeline on each frame of the video.

### 2. Potential shortcomings the current pipeline

The current pipeline has many shortcomings, one of them is the lack of car detection. This makes it vulnerable to other cars entering the region of interest especially if the color of the car is bright. 

Another shortcoming would be the hard turns, as we can see in the video of the optional challenge, the lane lines are out of the predefined region of interest which makes our pipeline non efficient.

In the second 4-5 of the challenge video, part of the road was a bit brighter that the rest of the road. In this part the canny edges algorithm has a harder time detecting edges since it's based on brightness. 

Another minor problem is the vibration of the drawn lines, this is caused by the *mean* function. A more "smooth" method should solve this problem.

### 3. Possible improvements to the pipeline

Based on the mentioned shortcomings, a potential improvement would be the replacement (or the combination ) of Canny edge detection with a colored images edge detection. This will allow for a better perception of the environement. Another useful addition would be car tracking, so the edge detection doesn't get confused by any cars entering the region of interest. 

Another potential improvement would be the implementation of an active region of interest. This region changes during hard turns based on the traffic signs (or other indicators). This will allow the other algorithms to work properly.

 
