# Introduction

According to a report by the National Highway Traffic Safety Administration, about 100,000 road accidents involve driving under drowsiness/intoxicaition. In order to avert the same, a drowsiness detection system has been implemented that alerts the driver if he/she sleeps.

## Working Details: 

The basic thing about drowsiness detection is pretty simple. We first detect a face using dlib's frontal face detector. Once the face is detected , we try to detect the facial landmarks in the face using the dlib's landmark predictor. The landmark predictor returns 68 (x, y) coordinates representing different regions of the face, namely - mouth, left eyebrow, right eyebrow, right eye, left eye, nose and jaw. Ofcourse, we don't need all the landmarks, here we need to extract only the eye and the mouth region. 

Now, after extraxting the landmarks we calculate the <b>Eye Aspect Ratio (EAR)</b> as: 

```python 
def eye_aspect_ratio(eye):
	# Vertical eye landmarks
	A = dist.euclidean(eye[1], eye[5])
	B = dist.euclidean(eye[2], eye[4])
	# Horizontal eye landmarks 
	C = dist.euclidean(eye[0], eye[3])

	# The EAR Equation 
	EAR = (A + B) / (2.0 * C)
	return EAR
```
The eye region is marked by 6 coordinates. These coordinates can be used to find whether the eye is open or closed if the value of EAR is checked with a certain threshold value.

In the same way I have calculated the aspect ratio for the mouth to detect if a person is yawning. Although, there is no specific metric for calculating this, so I have taken for points, 2 each from the upper and lower lip and calculated the mean distance between them as: 
```python 
def mouth_aspect_ratio(mouth): 
	A = dist.euclidean(mouth[13], mouth[19])
	B = dist.euclidean(mouth[14], mouth[18])
	C = dist.euclidean(mouth[15], mouth[17])

	MAR = (A + B + C) / 3.0
	return MAR
```
