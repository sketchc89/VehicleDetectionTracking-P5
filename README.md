# Vehicle Detection
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

[//]: # (Image References)
[output_0]: ./output_images/output_0.png

The goal of this project is to write a software pipeline to detect vehicles in a video (start with the test_video.mp4 and later implement on full project_video.mp4) and to explain the details of that project in the README below.

# The Project
---

The goals / steps of this project are the following:

* TODO: Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* TODO: Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* TODO: Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* TODO: Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* TODO: Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* TODO: Estimate a bounding box for vehicles detected.

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


