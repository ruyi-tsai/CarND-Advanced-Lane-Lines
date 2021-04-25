## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)
![Lanes Image](./examples/example_output.jpg)



Overview
---
This project I will use advence method to find the line on the road. However, I try overcome curve line of the road. In the below ,I will detail discuss pipeline .

Pipeline
---

* Cailbration your Camera
* Perspective Transform
* Color and Gradient
* Histogram
* Silding window
* polynomial curve fit

Cailbration your Camera
---
Since the camera is  distortion. This occurs when a camera’s lens is not aligned perfectly parallel to the imaging plane, where the camera film or sensor is. This makes an image look tilted so that some objects appear farther away or closer than they actually are.So we need cailbration camera to display the real world.
* Converting an image, imported by cv2 or the glob API, to grayscale:
```python
cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)
```
* Finding chessboard distort corr 
```python
cal_undistort(img, objpoints, imgpoints)
```
chessboard distortion result
![chessboard distortion](https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/undistortion.png)

Color and Gradient
---
Here we'll look specifically at HLS. When we separate the H, L, and S channels we get the following result:
```python
hls = cv2.cvtColor(image, cv2.COLOR_RGB2HLS)
```
![HSL_image](https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/HSL_image.png)
second I use gradient sobel method to find line (enhance edge) combine S channel image to show line: 
```python
sobelx = cv2.Sobel(gray, cv2.CV_64F, 1, 0) # Take the derivative in x
abs_sobelx = np.absolute(sobelx) # Absolute x derivative to accentuate lines away from horizontal
scaled_sobel = np.uint8(255*abs_sobelx/np.max(abs_sobelx))
s_binary[(s_channel >= s_thresh_min) & (s_channel <= s_thresh_max)] = 1
```
![combine_binary](https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/combine_binary.png)

 Perspective Transform
 ---
 Let's see perspective transform can help to see line.
 ```python
M = cv2.getPerspectiveTransform(src, dst)
Minv = cv2.getPerspectiveTransform(dst, src)
warped = cv2.warpPerspective(combined_binary, M, img_size, flags=cv2.INTER_LINEAR)
```
 ![perspective_image](https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/perspective_image.png)
 
 Histogram
 ---
 I take a histogram along all the columns in the lower half of the image like this
 ```python
  histogram = np.sum(bottom_half, axis=0)
  ```
 ![histgram](https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/histgram.png)
 
Silding window
---
As shown in the previous animation, we can use the two highest peaks from our histogram as a starting point for determining where the lane lines are, and then use sliding windows moving upward in the image (further along the road) to determine where the lane lines go.
```python
find_lane_pixels(binary_warped)
```
![find_lane_pix](https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/find_lane_pix.png)

polynomial curve fit
---
We can't use window to slow curve line ,it is so slow. We change another polynomial curve fit to track the lanes through sharp curves and tricky conditions

```python
search_around_poly(binary_warped)
```

![find_lane_fit_line](https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/find_lane_fit_line.png)

Result 
---
all result place in folder output_images
![test1](https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/test1.jpg)
![test2](https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/test2.jpg)
![test3](https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/test3.jpg)

Suggest Possible improvement to your pipeline
---
In this section , we only idenify line but have many sign to be line. So in the  further ,we want to exactly what is line or sign.

