# Danny's Udacity Term1 Project:  Finding Lane Lines on the Road

## Background / Scope of project
After watching the course videos and working through the examples a project is completed.  Although most of the code that performs image operations is already provided, you get the
benefit of repetition by having to review it and rewrite somem of it all in one location.





[//]: # (Image References)

[imageA]: C:\Users\Bynum\Documents\Udacity\Term1\DWB-T1-P1\test-images-output\PipeLine_Writeup_103.103.jpg "Grayscale"


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Prior to starting the actual function/project pipeline the following setup is performed:
* importing packages that are utilized: cv2, etc

The project pipeline involves the following steps - descriptions of notable aspects of each step are also included.
Step 1: Import and Export Images
(1a) Read in the images from the local PC hard drive so that the Python notebook can operate on them.  This is accomplished with the matplotlib.image package and the os package.
(1b) Create directory to save files and save images to this new directory after operating on them.  Same packages as '1a'.

Step 2: Image Operations: Create Suitable Input to Algorithms
(2a) Convert image to grayscale (since computing a gradient, want all pixels to be on single "intensity" scale.
(2b) Apply a low pass filter to the image using the "Gaussian Blur" operation - this is a spatial resolution filter that will reduce the single-pixel noise in the image and also make the image look "softer" overall.  The operation uses a "kernel size" to determine how much to blur the image - during the course/quizes I used a value of '3' so that's what I started with in this project - in order to illustrate this operation I have posted some pictures below.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![Blured][imageA]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
