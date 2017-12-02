# Vehicle Detection
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


The goal of this project is to write a software pipeline to detect vehicles in a video (start with the test_video.mp4 and later implement on full project_video.mp4) and to explain the details of that project in the README below.

The Project
---

The goals / steps of this project are the following:

* TODO: Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* TODO: Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* TODO: Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* TODO: Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* TODO: Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* TODO: Estimate a bounding box for vehicles detected.

Reference:
* [Labeled data for the vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/vehicles.zip) 
* [Labeled data for non-vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/non-vehicles.zip) 
* [GTI vehicle image database](http://www.gti.ssr.upm.es/data/Vehicle_database.html)
* [KITTI vision benchmark suite](http://www.cvlibs.net/datasets/kitti/)
* [Udacity labeled dataset](https://github.com/udacity/self-driving-car/tree/master/annotations)

Some example images for testing your pipeline on single frames are located in the `test_images` folder.  To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `ouput_images`, and include them in your writeup for the project by describing what each image shows.    The video called `project_video.mp4` is the video your pipeline should work well on.  

**As an optional challenge** Once you have a working pipeline for vehicle detection, add in your lane-finding algorithm from the last project to do simultaneous lane-finding and vehicle detection!

**If you're feeling ambitious** (also totally optional though), don't stop there!  We encourage you to go out and take video of your own, and show us how you would implement this project on a new video!
