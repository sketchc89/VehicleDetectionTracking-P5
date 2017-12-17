# Vehicle Detection
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

[//]: # (Image References)
[output_0]: ./output_images/output_0.png
[ref_image]: ./doc_images/ref_image.png "Image to demonstrate features"
[small_windows]: ./doc_images/small_window.png "Subset of small windows used for detection"
[smed_windows]: ./doc_images/smed_window.png "Subset of small-medium windows used for detection"
[med_windows]: ./doc_images/medium_window.png "Subset of medium windows used for detection"
[large_windows]: ./doc_images/large_window.png "Subset of large windows used for detection"
[no_edges_large]: ./doc_images/no_edges.png "Sliding window with no edges"
[edges_large]: ./doc_images/large_edges.png "Sliding window with edges large"
[edges_border]: ./doc_images/border_edges.png "Image with a border around the edge"
[edges_med]: ./doc_images/med_edges.png "Sliding window with edges medium"
[edges_small]: ./doc_images/small_edges.png "Sliding window with edges small"
[result_1]: ./doc_images/result_1.png "Test image 1 detection"
[result_2]: ./doc_images/result_2.png "Test image 2 detection"
[result_3]: ./doc_images/result_3.png "Test image 3 detection"
[result_4]: ./doc_images/result_4.png "Test image 4 detection"
[result_5]: ./doc_images/result_5.png "Test image 5 detection"
[color_hist]: ./doc_images/hist_feat.png "First channel color_histogram"
[spatial_feat_0]: ./doc_images/spatial_feat_0.png "First channel spatial feature"
[spatial_feat_1]: ./doc_images/spatial_feat_1.png "Second channel spatial feature"
[spatial_feat_2]: ./doc_images/spatial_feat_2.png "Third channel spatial feature"
[hist_feat_0]: ./doc_images/hist_feat_0.png "First channel hist feature"
[hog_feat_0]: ./doc_images/hog_feat_0.png "First channel HOG feature" 
[hog_feat_1]: ./doc_images/hog_feat_1.png "Second channel HOG feature"
[hog_feat_2]: ./doc_images/hog_feat_2.png "Third channel HOG feature"
[heatmap_1]: ./doc_images/heatmap_1.png "Heatmap from result 1"
[heatmap_2]: ./doc_images/heatmap_2.png "Heatmap from result 2"
[heatmap_3]: ./doc_images/heatmap_3.png "Heatmap from result 1"
[heatmap_4]: ./doc_images/heatmap_4.png "Heatmap from result 2"
[heatmap_5]: ./doc_images/heatmap_5.png "Heatmap from result 1"

The goal of this project is to write a software pipeline to detect vehicles in a video (start with the test_video.mp4 and later implement on full project_video.mp4) and to explain the details of that project in the README below.

# The Project
---

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

## Training Model

### Colorspace

I tested all of the colorspaces against a test set to see which colorspace worked the best. YUV was slightly better than HLS/HSV. YUV colorspace also allowed me to reduce the number of channels that I had to use in some tests. The Y channel represents the luminance and is the most useful for human perception, though not necessarily computers. I tested using only using the first channel of YUV to trade some speed for accuracy but eventually used all 3.

### Classifier

A linear support vector machine was initially used with default values. This gave 98.5% accuracy which was good enough to work on the pipeline. 
Afterwards, I went back to improve the accuracy. I range `GridSearchCV()` over two kernels and C values. I found that the rbf kernel with C of 10.0 had a better f1 score than the linear svc kernel with C of 1.0. I used f1 so that I didn't miss false positives.
After testing in my pipeline, I found that using the rbf kernel was substantially slower in training and predicting. Checking the classifier took up 96% of the time of the pipeline whereas the linear kernel only took 6% of the pipelines time.
I grid searched with the linear kernel from 0.1 to 100 and found that C didn't significantly influence the score, but 0.1 was slightly better and it is less prone to overfitting.

## Pipeline

### Vehicle Detector Class

A vehicle detector class was used to keep track of heatmaps between frames and keep useful variables used between functions like threshold, classifier, scaler, etc.

### Features

![Reference image][ref_image]

#### Histogram of Gradients - Sliding window to subsampling and back

I first used a sliding window with skimage hog. This was very slow, so I moved to HOG subsampling which was better. I also read that OpenCV's implementation of HOG was different than skimage. I tried it using a sliding window and this turned out to be even faster than HOG subsampling. It also significantly reduced the complexity of my code. Sub-sampling requires you to keep track of the position in blocks, cells, and pixels, while sliding window only requires pixels. I used a window of 64x64 to match the training images. I used 32 pixels per block, 16 pixels per cell and 4 pixels per step to increase speed. The number of orientation bins set to 9 based on this [paper](https://courses.engr.illinois.edu/ece420/fa2017/hog_for_human_detection.pdf) which showed statistically significant increases in performance up to 9 bins.

![Hog features][hog_feat_0]
![Hog features][hog_feat_1]
![Hog features][hog_feat_2]

#### Color Histogram

I was suprised by how slow the histogram operation was. After moving to HOG sub-sampling, the color histogram function `color_hist()` was 2-2.5x slower than HOG. In order to reduce the time spent on this operation, I rewrote the function so that I could extract data from just one channel. Since I was using YUV, I could eliminate the second and third channels without losing much accuracy since the first channel contains the most important visual information. I eventually removed the feature all together to increase speed with less than a 1% loss in accuracy.

![Color Histogram][color_hist]

#### Spatial binning

This operation was fast enough that I didn't worry about whether or not it was adding any value.

![Spatial features][spatial_feat_0]
![Spatial features][spatial_feat_1]
![Spatial features][spatial_feat_2]

### Windows

Since I switched to using OpenCV's HOG implementation that uses windows rather than sub-sampling, my window logic was much simpler. I used 4 scales of windows (128px-0.5, 107px-0.6, 91px-0.7, 80px-0.8). I restricted the small windows to the top, center region of the image, the medium windows all but the top, and large windows to the lower portion. I used an overlap of 75-80% which led to my system running slower. Below is a subset of the windows used in the small, medium, and large scales.

![Small sliding windows][small_windows]
![Small sliding windows][smed_windows]
![Medium sliding windows][med_windows]
![Large sliding windows][large_windows]


#### Edges

The normal method of implementing sliding windows shifts windows over until the right/bottom side of the window fall off the image. With larger step sizes, this causes the right and bottom sides of the image to be missed. I added in a check to force the windows so that the right side of the window aligned with the right side of the image and the bottom side of the window aligned with the bottom side of the image for each loop. This still didn't produce the results I wanted so I added a border around the width of the window after scaling so that the same amount of overlap would occur at the edges as at the center. Note the sides are pulled out on the third image.

![Edges of sliding windows missing][no_edges_large]
![Edges of sliding windows added][edges_large]
![Edges with border][edges_border]

### Boxes, Heatmaps, & Thresholding

Windows found to contain cars were passed to a heatmap. Pixels within the window were given points. The current heatmap was added to a deque of length 30 used to keep track of heatmaps between frames. The mean score of the previous 30 frames were added to the current heatmap to produce a more stable heatmap. All pixels that were found to contain cars below the set threshold were ignored because these could be due to poor detection. Blobs of pixels were labeled and a box was drawn around them.

Below are heatmaps from the test images with a threshold of 5. Top-left image, top-right, current heatmap, bottom left threshold, bottom right final image.

---
#### Test image 1

![Group Heatmap][heatmap_1]

---
#### Test image 2

![Group Heatmap][heatmap_2]

---
#### Test image 3

![Group Heatmap][heatmap_3]

---
#### Test image 4

![Group Heatmap][heatmap_4]

---
#### Test image 5

![Group Heatmap][heatmap_5]

### Video 
# TODO Make sure this link works
The [linked video](./result_vid.mp4) shows the vehicle pipeline working on [the given project_video](./project_video.mp4).

## Areas for Improvement

### Speed

My pipeline ran at 3-4 iterations per second. The tracking pipeline I used was fairly quick on each individual window (the HOG step was not the weak link), but the number of windows I chose to use to get good detection on the edges was probably too high. After writing up the areas to improve, I implemented improving edges and throwing away the color histogram.

Future areas to improve:
* Reduce overlap for small windows that take up a large percent of the time. I could reduce overlap if detection at the edges were better.
* Understand how OpenCV HoG works better. It is possible to run on a full image and parse the windows from the resulting vector. This could be faster than looping in python with vectorization.

## Files:
* [test images folder](./test_images): Some example images for testing the pipeline on single frames.  
* [output images folder](./output_images): Images from each stage of the pipeline to help the reviewer examine the pipeline
* [project_video.mp4](./project_video.mp4): The video that the pipeline should work well on.  
* [documentation text folder](./doc_text): Text files showing the speed of functions as improvements were made and classifier parameter search.
* [documenation images folder](./doc_images): Images used for documentation purposes

## Reference:
* [Labeled data for the vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/vehicles.zip) 
* [Labeled data for non-vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/non-vehicles.zip) 
* [GTI vehicle image database](http://www.gti.ssr.upm.es/data/Vehicle_database.html)
* [KITTI vision benchmark suite](http://www.cvlibs.net/datasets/kitti/)