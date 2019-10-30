# SDND-Advanced-Lane-Lines-Finding

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

[image1]: ./examples/image1.png
[image2]: ./examples/image2.png
[image3]: ./examples/image3.png
[image4]: ./examples/image4.png
[image5]: ./examples/image5.png
[image6]: ./examples/image6.png
[video1]: ./project_video_result_final.mp4 


### To finish this project, files that i have created are listed:

1. 'for test images.ipynb'.    for test images
2. 'P2_task.ipynb' .     for video testing
3. 'project_decription.ipynb' .    as writeup file.
4. result video is /project_video_result_final.mp4
5. extra task for challenge video, result is /project_video_challenge_final.mp4

---

### Camera Calibration

#### 1. compute the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function. I applied this distortion correction to the test image 'camera_cal/calibration1.jpg' using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

the parameter 'mtx', 'dist' are saved in the file 'camera_cal/dist_picklecol.p', which can be reused for the following tasks

### Pipeline (test images)

I use '`for test images.ipynb`' to do the test.

#### 1. Provide the distortion-corrected images and perform a perspective transform to use on these images.

The code for distortion corrected and perspective transform using on test images is included in function called `warp()`. The `warp()` function takes test image as input. For the internal of the function, `cv2.undistort()` will be used to correct the distortion, source points `src` and destination points `dst` for the perspective transform are chosen as follows: 

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 250, 0        | 
| 200, 720      | 250, 720      |
| 1096, 720     | 960, 720      |
| 700, 460      | 960, 0        |

I verified that my perspective transform was working realtively well, the sided black filled area are quite small. Here are the original test images and the corresponding processed images `transform_undst_img`
![alt text][image2]
![alt text][image3]
The distortion-corrected original test images are saved as `undist_img`

#### 2. transform images into binary images and apply different types of filters to sift the points belonging most likely to the lane lines.

LAB filters are defined as functions:

`color_wy` : LAB Color space filter, i use that with the corresponding parameters to identify white and yellow colors.

Threshold of L,A,B for both white and yellow colors are adjusted and then combined as filter `(yellow == 1) | (white == 1)`. When only this filter is applied, the effect seems quite well.
![alt text][image4]

Whatsmore, i also define several other filters to improve the stability of the indentification in different outer environments (hope they are useful 😏):

`filter_sobelx`: Sobel Filter in x direction
`filter_mag`: Magnitude of the gradient
`filter_dir`: Direction of the gradient
`filter_hls`: HLS filter for the channel S
`filter_hls1`: HLS filter for the channel L

the logic of combination of these filers are:
`[Sobel_x | LAB |(Magnitude & Directon) | HLS] = 1`. Effect as follows:
![alt text][image5]

I use this combined filter to handle the follwing processing. 


#### 3. Edit a function `hist()` to integrate three functions:

1. identify the points of the left and right lanes using sliding windows and fit lanes with polynomials.

2. calculate the radius of curvature of the lanes.

3. calculate the position of the vehicle with respect to center.


#### 6. Retransform and draw/text the result on `undist_img` 

Here is my result on test images:
![alt text][image6]

---

### Pipeline (video)

I use '`P2_task.ipynb`' to do the video test. The main part of the program starts form the cell 'video loading and processing'. 

Here's a [link to my video result](./project_video_result_final.mp4)

---

### Discussion and Questions

#### 1. Briefly discuss problems / issues i faced in my implementation of this project.

1. To make it robust, each filter itself and the combination of each filter should be adjusted.

2. in the project_video_result_final.mp4, at about 24. Second, Lane detection is quite bad. I have tried to use LAB filter as mentioned above alone, afterwards, this bad fluctuation do not happen and the total video result runs even better, but when i use LAB filter alone for challenge video, it can not run and hints fault.

3. The effect on challenge video seems very bad, i would like to ask some recommendations about tuning and choosing the image filter and also other approaches to improve that.
Here's a [link to my challenge video result](./project_video_challenge_final.mp4)






```python

```
