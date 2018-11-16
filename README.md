# Danny's Udacity Term1 Project:  Finding Lane Lines on the Road

## Background / Scope of project
The course has videos and exercises that walk through examples of various computer vision functions that are needed to draw lines that represent the left and right lane markings.  Most of the code (but not all) for the project is already provided but you have to "tune" some of the parameters to make it produce the right results.



[//]: # (Image References)

[imageA]: ./images_in_writeup/DWB-writeup1.jpg "Grayscale"
[imageB]: ./images_in_writeup/Canny-fig.jpg "Grayscale"
[imageC]: ./images_in_writeup/ROIandRawLines.jpg
[imageD]: ./images_in_writeup/ImproveDrawLines.jpg


---

### Reflection

### 1. The project pipeline involves the following steps

_Step 0: Activate Environment and Notebook via Anaconda prompt_
Basic commands entered are as follows:
```
cd ~\Documents\Udacity\Term1>cd DWB-T1-P1
conda env list
activate carnd-term1
Jupyter Notebook
```

_Step 1: Import and Export Images_

(1a) Read in the images from the local PC hard drive so that the Python notebook can operate on them.  This is accomplished with the matplotlib.image package and the os package.
(1b) Create directory to save files and save images to this new directory after operating on them.  Same packages as '1a'.  A Couple of code examples are:
```
os.chdir('C:/Users/bynum/documents/udacity/term1/DWB-T1-P1')
WhtCurv = mpimg.imread('test_images/solidWhiteCurve.jpg')
```

_Step 2: Image Operations to hide everything except edges_

(2a) Convert image to grayscale (since computing a gradient, want all pixels to be on single "intensity" scale).

(2b) Apply a low pass filter to the image using the "Gaussian Blur" operation - this is a spatial resolution filter that will reduce the single-pixel noise in the image and also make the image look "softer" overall.  The operation uses a "kernel size" to determine how much to blur the image - during the course/quizes I used a value of '3' so that's what I started with in this project - in order to illustrate this operation I have posted some pictures below.

![Image of Gaussian Blur][imageA]

(2c) Apply the Canny Edge Detection function (built into openCV) which looks for gradients (large changes in intensity from pixel to pixel) across the image.  The output of this function looks like the picture below.  The parameters (and short description) I used are below as well
```
Image_Op3 = cv2.Canny(Image_Op2,Can_L_Trshld,Can_H_Trshld)
Can_L_Trshld = 40  #gradients less than this not considered
Can_H_Trshld = 220 #gradients above this considered, as well as gradients down to low_threshold if connected to a pure high gradient
```

![Image of Canny Function Ouput][imageB]

_Step 3: Find pixels which can be classified as lines/line-segments_

(3a) Create a region of interest "mask" which only looks at the portion of the image which is likely to contain lane lines.  This helps filter out possible line-segments that are not lane lines and the region I chose to use for this project is shown in blue below.  The code to accomplish this is a bit-wise and as follows:
```
mask = np.zeros_like(Image_Op3)   
ignore_mask_color = 255 
vertices= np.array([[(x1,y1),(x2,y2),(x3,y3),(x4,y4),(x1,y1)]],dtype=np.int32)
cv2.fillPoly(mask, vertices, ignore_mask_color)
masked_edges = cv2.bitwise_and(Image_Op3, mask)
```

(3b) Find line segments by transforming X-Y space into "Hough Space" using the Hough Lines Function that is built-in to openCV.  The tuning of the parameters to optimize selection of lane lines is pretty intuative using the descriptions given for each parameter in the function so I will paste in those notes below:
```
# Define the Hough transform parameters
rho = 2+0 # distance resolution in pixels of the Hough grid
theta = np.pi/(360*1) # angular resolution in radians of the Hough grid
threshold = 18-0   # minimum number of votes (intersections in Hough grid cell)
min_line_length =17-10 #minimum number of pixels making up a line
max_line_gap = 2+5    # +40 maximum gap in pixels between connectable line segments

# Output "lines" is an array containing endpoints of detected line segments
lines = cv2.HoughLinesP(masked_edges, rho, theta, threshold, np.array([]),
                            min_line_length, max_line_gap)
```

The image below shows the output of step 3 - which is raw line segments drawn within the defined ROI.

![Image ROI and Raw Lines][imageC]

_Step 4: Draw two single continuous lines to represent the left and right lane lines_
NOTE:  This is my description of how I improved the "draw lines" function, but I didn't put it inside a function.

The applies some simple logic as it steps through each line defined by the Hough function:

(a) It calculates a current slope and current y-intersept for the selected/given line segment definition.

(b)  It performs two checks to try and group each line segment as a "left line group" element or "right line group" element.  It does this by checking for "reasonable" slopes and also checking which half of the image the line segment starts on. Note that any line segment that has a slope that matches neither left or right is rejected.

(c)  The the slopes and y-intercepts are averaged for left and right groups respectively

(d) The left group line is then drawn and the right group line is drawn and overlaid on the image.  I used a fixed verticle position near the bottom of the image for "y1", but for the y2 endpoint I used the "tallest" y value that is in the appropriate group of line segments.

The output of this is an image with a single overlay line representing both the left and right lane lines.  The following image shows the line segments that were averaged into a single fine line with this function
![Image of Improved Draw Lines Function][imageD]

_Step 5: Perform the draw lines on each successive image in a video_
The pipeline was written to work on single images (one at a time), but then I put all of that into a single funciton that can be called multiple times to operate on a video.  Once the single image pipeline is put into a function it is suprisingly short set of code to perform the operation on a video stream and save the output - here is the code:
```
# Import everything needed to edit/save/watch video clips
import pylab
from moviepy.editor import VideoFileClip
from IPython.display import HTML

def process_image(Current_Image):
#DWB Pipeline OMITTED in writeup - see files
    return Return_Image
#only takes 4 lines of code to read in, process and save video! Cool!
white_output = 'test_videos_output/solidWhiteRightDanny2.mp4'
clip1 = VideoFileClip("test_videos/solidWhiteRight.mp4")
white_clip = clip1.fl_image(process_image) #NOTE: this function expects color images!!
%time white_clip.write_videofile(white_output, audio=False)
```

### 2. Identify potential shortcomings with your current pipeline

The ROI is fixed and it is pretty restricted for this camera and camera mount location so if the camera is changed this may have to be redone completely.

Sometimes the slope of the calculated lane line is a little bit off from the actual lane line and this could probably be made more accurate.


### 3. Suggest possible improvements to your pipeline

Make the overall result much less dependent on the ROI since this could change for different mount locations.

The code needs to be refactored and cleaned up if it were to be used anywhere else.

I noticed that my pipeline does not work on the challenge video because it got a divide by zero error.  For cases where I am calculating slopes I need to check that we will not be dividing by zero as we step through a list.

Right now the way it draws lines the end point sometimes does not extend all the way to the end of the lane line that your eye can see so this could be improved.
