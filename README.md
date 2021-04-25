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
(https://github.com/ruyi-tsai/CarND-Advanced-Lane-Lines/blob/master/output_images/undistortion.png)
