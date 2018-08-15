# **Finding Lane Lines on the Road**

[Jupyter Notebook (HTML)](P1.html)

[//]: # (Image References)

[image1]: ./res/1_Gauss.png "Grayscale"
[image2]: ./res/2_Canny.png "Canny"
[image3]: ./res/3_mask.png "Masked"
[image4]: ./res/4_Hough.png "Hough"
[image5]: ./res/5_filtered.png "Filtered"
[video1]: ./test_videos_output/solidWhiteRight.mp4 "vid1"
[video2]: ./test_videos_output/solidYellowLeft.mp4 "vid2"
[video3]: ./test_videos_output/challenge.mp4 "vid3"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied a gaussian blur as shown in the below image.  Here, I employed a `kernel_size` of `[5,5]`.  

![alt text][image1]

After Gaussian smoothing, I appled Canny Edge Detection in line with the recommended 3:1 ratio of threshold, employing a `low_threshold` of `50` and a `high_threshold` of `150`.  These are the same values used in the lecture notes.

![alt text][image2]

After Canny Edge Detection, I applied a mask which formed a simple parallelogram.

![alt text][image3]

From there, I was able to use the Hough Transform with the following arbitrary parameters:
* `rho` = 1
* `theta` = π/180
* `threshold` = 25
* `min_line_length` = 200
* `max_line_gap` = 100

![alt text][image4]

In order to draw a single line on the left and right lanes, I modified the `draw_lines( )` function by sorting lines by their length, longest to shortest.  These lines were then extended to the top and bottom of the masked image.  With a specified band where the endpoints should lie within, I removed any entries which fell outside this band.  I declared the longest line to be one of the two lane lines and then chose the other as the second longest whose slope was opposite in sign to that of the longest.  This was repeated with reduced Hough parameters until two lines were produced.

![alt text][image5]

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when the road curves.  There is no logic currently to frame and capture the curvature of the road.  The pipeline would break down further if there was additional signage in the road or more cars directly in front of it.

Another shortcoming could be that the masking references static values in the corners of the image frame.  This could become tedious to maintain if the camera were to be relocated.  In fact, even minor calibrations of the camera would require considerable upkeep on the software side.  Better logic can be used in the future.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to include the current steering angle into the Hough line calculation.  This would provide an estimate to the curvature of the road and lane lines.

Another potential improvement could be to interpolate using prior frames.  Currently, the pipeline fits one set of lines per image, but with a prior estimate of where lane markers were, estimate drift can be reduced.

# Videos

[video 1](test_videos_output/solidWhiteRight.mp4)

[video 2](test_videos_output/solidYellowLeft.mp4)

[video 3](test_videos_output/challenge.mp4)
