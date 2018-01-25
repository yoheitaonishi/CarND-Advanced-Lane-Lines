# Advanced Lane Finding Project

[//]: # (Image References)

[image1]: ./output_images/image01.png "Undistorted"
[image2]: ./output_images/image03.png "Road Transformed"
[image3]: ./output_images/image04.png "Binary Example"
[image4]: ./output_images/image05.png "Warp Example"
[image5]: ./output_images/image07.png "Fit Visual"
[image6]: ./output_images/image08.png "Output"

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in first, second and third cell of the IPython notebook located in "./project.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Distortion

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Color transforms

I used a combination of color and gradient thresholds to generate a binary image.  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![alt text][image3]

#### 3. Perspective transform

The code for my perspective transform is in the 8th code cell of the IPython notebook).  it takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32([
                  (580, 464),
                  (735, 464), 
                  (275, 682), 
                  (1110, 682)]
    )
dst = np.float32([
                  (offset,0),
                  (width-offset,0),
                  (offset,height),
                  (width-offset,height)]
    )
```

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Identified lane-line pixels and fit their positions with a polynomial

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in 18th, 19th, 20th and 21th code cell in `project.ipynb`

#### 6. Plotted back down onto the road.

I implemented this step in 20th code cell.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Final video output.  

Here's a [link to my video result](https://youtu.be/rlpJdBNwUV4)

---

### Discussion

* Sometimes, my video can't detect line smothly. I think Line class can be improved. Now, I use last n lane curverad if line isn't detectd but it doesn't work well when line isn't detected in a continuous. I should calculate line curverad insted it. It would be good if Line class has some calculated value like array of previous curverad. Then I can calulate curverad with some weight
* In challenge video, right half of road that car is running and left half one are different color. In this case, my code might detect border of different color as line. It should be set limit of line width.
