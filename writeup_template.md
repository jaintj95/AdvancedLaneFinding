## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./camera_cal_output/distortion_corrected/calibration1.jpg "Calibrated Image"
[image2]: ./output_images/distortion_corrected/straight_lines1.jpg "Undistorted Example Image"
[image3]: ./output_images/binary_image/straight_lines1.jpg "Binary Image"
[image4]: ./output_images/perspective_transformed/straight_lines1.jpg "Perspective Transform"
[image5]: ./output_images/polynomial_fit/straight_lines1.jpg "Polynomial Fit"
[image6]: ./output_images/final_image/straight_lines1.jpg "Final Output"
[video1]: ./project_video_output.mp4 "Video"


## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the seoncd code cell of the IPython notebook located in "./project.ipynb".

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![Calibrated Image][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![Undistorted Image][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at cell 6).  Here's an example of my output for this step.

![Binary Example][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `perspective_transform()`, which appears in cell 7 in the IPython notebook).  The `perspective_transform()` function takes as inputs an image (`img`). I chose to hardcode the source and destination points in the following manner:

```python
    src = np.float32([[568, 468], [715, 468], [1040, 680], [270, 680]])
    dest = np.float32([[200, 0], [1000, 0], [1000, 680], [200, 680]])
```

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![Perspective Transform][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I used the methods taught in the course such as sliding windows for this part. The code for polynomial fitting is in the 7th and 8th cell of the notebook.

![Polynomial Fit][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

Using the concepts introduced in the course I conquered this part of the challenge. The code for this is in 9th and 10th cell of the notebook.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

The code for this in the 11th cell of the notebook.  Here is an example of my result on a test image:

![Final Output][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The current implement works fine with shadows and glares on roads but it will likely fail when the road has steep curves and is not flat.
