# Vehicle Detection
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

[//]: # (Image References)
[output_0]: ./output_images/output_0.png

The goal of this project is to write a software pipeline to detect vehicles in a video (start with the test_video.mp4 and later implement on full project_video.mp4) and to explain the details of that project in the README below.

# The Project
---

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* TODO: Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

## Training Model

### Colorspace
YUV colorspace was used because it allowed me to reduce the number of channels that I had to use. This helped speed up my model because I could throw away two-thirds of the data for the slowest step (color histogram) without suffering in accuracy.

### Classifier

A linear support vector machine was initially used with default values. This gave 98.5% accuracy which was good enough to work on the pipeline. 
Afterwards, I went back to improve the accuracy. I range `GridSearchCV()` over two kernels and C values. I found that the rbf kernel with C of 10.0 had a better f1 score than the linear svc kernel with C of 1.0. I used f1 so that I didn't miss false positives.
After testing in my pipeline, I found that using the rbf kernel was substantially slower in training and predicting. Checking the classifier took up 96% of the time of the pipeline whereas the linear kernel only took 6% of the pipelines time.
I grid searched with the linear kernel from 0.1 to 100 and found that C didn't significantly influence the score, but 0.1 was slightly better and it is less prone to overfitting.

# Files:
* [test images folder](./test_images): Some example images for testing the pipeline on single frames.  
* [output images folder](./output_images): Images from each stage of the pipeline to help the reviewer examine the pipeline
* [project_video.mp4](./project_video.mp4): The video that the pipeline should work well on.  

# Reference:
* [Labeled data for the vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/vehicles.zip) 
* [Labeled data for non-vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/non-vehicles.zip) 
* [GTI vehicle image database](http://www.gti.ssr.upm.es/data/Vehicle_database.html)
* [KITTI vision benchmark suite](http://www.cvlibs.net/datasets/kitti/)
* [Udacity labeled dataset](https://github.com/udacity/self-driving-car/tree/master/annotations)


