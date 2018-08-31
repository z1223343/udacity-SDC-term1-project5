# Vehicle Detection Project

### The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

---

[//]: # (Image References)
[image1]: ./output_images/raw_image.png
[image2]: ./output_images/hog_image.png
[image3]: ./output_images/vehicle_detect.png
[image4]: ./output_images/reduced_image.png
[image5]: ./output_images/vehicle_detect2.png
[image6]: ./output_images/normaliztion.png

[video1]: ./project_video.mp4

## Rubric Points
Here I will consider the rubric points individually and describe how I addressed each point in my implementation.

### Writeup/README
You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain my way to extracted HOG features from the training images.
The code for this step is contained in the 5th code cell of the `vehicle_detection.ipynb`

Before the `get_features` function, I read in all `vechile` and `non-vehicle` images. Here is an example of the `vehicle` and `non-vehicle` classes:

![][image1]
![][image4]

I then use gray scale image as an example to show the output of `skimage.hog()` function. Here parameters are `orient = 11`, `pix_per_cell = 16`, `cell_per_block = 2`. In the following pipeline, I will use 'YCrCb' colorspace to extract hog features in image.

![][image2]

#### 2. Explain how you settled on your final choice of HOG parmeters.

I tried various combinations of parameters and I finally detemined the parameters as mentioned above.

#### 3.  Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I decided to use `LinearSVC` as the SVM classifier because I found a non-linear kernel will lead to much more training time and might cause somewhat overfitting. Before training step, I use `sklearn.preprocessing.StandardScaler` to create `X_scaler` based on the training dataset, and normalize both the training dataset and test dataset.

![][image6]

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search. How did you decide what scales to search and how much to overlap windows?

To reduce false positive detection, I set sliding window area. For y axle, it starts searching at `ystart` and ends at `ystop`; For x axle, it starts searching at `xstart`. There are 8 scales of window, which can lead to a relatively better results after trying various combination of parameters. This part of code can be found in 8th cell of `vechile_detection.ipynb`.

#### Show some examples of test images to demonstrate how your pipeline is working. What did you do to optimize the performance of your classifier?

As mentioned above, I extract HOG features by using 3-channel YCrCb colorspace without spatially binned color and histograms of color. There are example output:

![][image3]
![][image5]

### Video Implementation

#### 1.Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

Here is a [link to my video result](./output_video.mp4)

#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

To make the results stable and to reduce false positive, I set a threshold in heatmap working as a filter which can get over wrong detection by accident. I then used scipy.ndimage.measurements.label() to identify individual blobs in the heatmap. I then assumed each blob corresponded to a vehicle. I constructed bounding boxes to cover the area of each blob detected.

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?

In this project, my vehicle detection is not very stable and cannot draw box out vechiles completely (for example it cannot always detect the white car's rear part). These weak points can be improved by choosing a better SVM classifier or apply deep learning approach, and video pipeline can be added filters like Kalman filter to make it more stable. Actually I am working on a more stable vision of vechile detector and use 3D bounding box to mark vehicles in the images. I was inspired by another student's work, where vehicle detector was done in the 'bird view' image. I will immediately update this repository after finishing it.
