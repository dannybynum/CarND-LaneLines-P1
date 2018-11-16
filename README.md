# Danny's Udacity Term1 Project:  Finding Lane Lines on the Road

## Background / Scope of project
The course has videos and exercises that walk through examples of various computer vision functions that are needed to draw lines that represent the left and right lane markings.  Most of the code (but not all) for the project is already provided but you have to "tune" some of the parameters to make it produce the right results.



[//]: # (Image References)

[imageA]: ./images_in_writeup/DWB-writeup1.jpg "Grayscale"
[imageB]: ./images_in_writeup/Canny-fig.jpg "Grayscale"

---

### Reflection

### 1. The project pipeline involves the following steps

***Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.***

_Step 1: Import and Export Images_
(1a) Read in the images from the local PC hard drive so that the Python notebook can operate on them.  This is accomplished with the matplotlib.image package and the os package.
(1b) Create directory to save files and save images to this new directory after operating on them.  Same packages as '1a'.  A Couple of code examples are:
'''
os.chdir('C:/Users/bynum/documents/udacity/term1/DWB-T1-P1')
WhtCurv = mpimg.imread('test_images/solidWhiteCurve.jpg')
'''

_Step 2: Image Operations to hide everything except edges_
(2a) Convert image to grayscale (since computing a gradient, want all pixels to be on single "intensity" scale).

(2b) Apply a low pass filter to the image using the "Gaussian Blur" operation - this is a spatial resolution filter that will reduce the single-pixel noise in the image and also make the image look "softer" overall.  The operation uses a "kernel size" to determine how much to blur the image - during the course/quizes I used a value of '3' so that's what I started with in this project - in order to illustrate this operation I have posted some pictures below.

![Blured][imageA]

(2c) Apply the Canny Edge Detection function (built into openCV) which looks for gradients (large changes in intensity from pixel to pixel) across the image.  The output of this function looks like the picture below.  The parameters (and short description) I used are below as well
'''
Image_Op3 = cv2.Canny(Image_Op2,Can_L_Trshld,Can_H_Trshld)
Can_L_Trshld = 40  #gradients less than this not considered
Can_H_Trshld = 220 #gradients above this considered, as well as gradients down to low_threshold if connected to a pure high gradient

'''

![Blured][imageB]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
