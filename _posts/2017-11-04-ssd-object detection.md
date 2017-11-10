---
layout:     post
title:      Single Shot MultiBox Detector
subtitle:   SSD
date:       2017-11-04
author:     stephen zhou
catalog: true
tags:
    - computer vision
    - deep learning
    - object detection
---

# related work
![](https://i.imgur.com/91Xrk1v.png)
**figure 1:** classic network of object detection

# model
![](https://i.imgur.com/va1IK8u.png)
**figure 2:** the confrontation between YOLO and SSD

# details
**Multi-scale feature maps for detection**

**Convolutional predictors for detection:** 3x3 kernel   default boxes

**Default boxes and aspect ratios:**
scale: ![](https://i.imgur.com/MVbIfLL.png)
aspect ratios: 4 or 6
mxn—>(c+4)kmn
![](https://i.imgur.com/kRD4QY1.png)
**figure 3:** SSD working framework

# loss
**Matching strategy:** every ground truth match with IOU higher than a threshold(0.5)

**Loss function:**
![](https://i.imgur.com/AQhiMxj.png)
L(conf): softmax
L(loc): Smooth L1 Loss

# trick
**Negative mining:** ratio between the negatives and positives is at most 3:1

**Data Augmentation:**
For every image: 

>- Use the entire original input image.
>- Sample a patch so that the minimum IOU overlap with the objects is 0.1, 0.3, 0.5, 0.7, or 0.9.
>- Randomly sample a patch

Then for every patch , horizontally flipped with probability of 0.5
mAP update from 65.4% to 74.3%

# experiment result

![](https://i.imgur.com/G6SpcRB.png)
**figure 4:** PASCAL VOC2007 test detection results

![](https://i.imgur.com/U5qrhwf.png)
**figure 5:** PASCAL VOC2007 test detection results of different models

![](https://i.imgur.com/c1kbGJK.png)
**figure 6:** Sensitivity and impact of different object characteristics on VOC2007 test set using

![](https://i.imgur.com/DEJkKPu.jpg)
**figure 7:** Detection examples on COCO test-dev with SSD512 model

# model analysis
>- Data augmentation is crucial
>- More default box shapes is better
>- Atrous is faster
>- Multiple output layers at different resolutions is better

![](https://i.imgur.com/BfQjB17.png)
**figure 8:** Effects of various design choices and components on SSD performance

![](https://i.imgur.com/ZqUBNtC.png)
**figure 9:** Effects of using multiple output layers

# conclusion
>- The core of SSD :predicting category scores and box offsets for a fixed set of default bounding boxes in multiscale
>- Faster and better

