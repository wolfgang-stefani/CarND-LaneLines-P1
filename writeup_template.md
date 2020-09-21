# **Finding Lane Lines on the Road** 

## Writeup


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "gray"
[image2]: ./examples/blur_gray.jpg "blur_gray"
[image1]: ./examples/grayscale.jpg "Grayscale"
[image1]: ./examples/grayscale.jpg "Grayscale"
[image1]: ./examples/grayscale.jpg "Grayscale"
[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of 5 steps. 

1) First, I converted the initial images ("image") to grayscale >>"gray":

![alt text][image1]

2) Second, I blurred the images with Gaussian blurring >> "blur_gray".

![alt text][image2]

3) Third, I canny-edged the images. This algorithm detects pixels whose adjacent pixels have quite different values. The result is images where you can see only silhouettes ("masked_edges"). 
4) The canny-edged images can now be used for hough transformation. This algorithm is able to detect lines in images. Every pixel can be represented as a sine-curve in rho-theta-parameter space the so-called hough space. Intersections of sine-curves in hough spaces represent detected lines. cv2HoughLinesP writes the endpoints (x1, y1, x2, y2) of the detected lines in to a simple array. 
5) Next the detected lines are drawn on a blank copy (same dimensions with values set to 0) of the initial image or canny-edged image (no matter because they have the same dimensions). NOTE: The output of the function hough_lines() includes hough transformation (described in step 4), line detection (dito) and drawing the detected lines on a blank copy. Drawing is realized by the function draw_lines(). The result is "line_image".
6) In the next step, the initial image is combined with the line_image using the function weighted_image(). The resulting image "combo" is computed as follows: initial_img * α (=0.8) + line_img * β (=1. + γ (=0)
7) Last, only the region of interest is shown using the function region_of_interes() and previously defined vertices.

Additional info to step 5): In order to draw a single line - and not just a lot of line segments - on the left and right lanes, I modified the draw_lines() function as followed:

1) All the detected lines have an individual slope m and intercept b. For each line these both parameters are being computed (m = (y2-y1)/(x2-x1) and b = y1 - m*x1) and written into a dedicated array to use them later. For example all slopes m from line segments belonging to the left lane are written to the array lm = np.array(()). How to know that line segments belong to the left lane in this example? The left lane has negative slope (attention: y-axis goes down!). So only if the condition that slope < 0 is fulfilled, the slope of the analysed line segment is being written into the lm-array.
2) Now we have a long list with many values for slopes and intercept. To find one corresponding slope respectively intercept the array is used to compute the average, e.g. the values in lm are used to compute the average slope of the left lane "lm_avg". Same happens to the intercept of the left lane, slope and intercept of the right lane.
3) For each lane, two points can be computed. One point is where y = 540 in order to start at the bottom and meet the requirements. The second point is at the height of the apex from the region of interest which is y = 310. 

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
