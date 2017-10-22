---
layout:     post
title:      Feature Pyramid Networks for Object Detection
subtitle: 
date:       2017-10-11
author:     zcy
catalog:    true
tags:
	- computer vision
	- pyramid network
	- object detection

---

# Abstract

This paper deals with the issue of Object Detection, and
the author provide a new algorithm call "Feature Pyramid Networks"
, which is inspired by "Image Pyramid" in traditional object
detection approaches. Compared with R-CNN and its variant, the 
FPN approach greately improve the speed of predition, and achieves state-of-the-art single-model results on the COCO detection benchmark.

# Background

This paper involes lots of related-work, especially the contents
about the Object Detection.

## Traditional Approaches
- SIFT (Scale-invariant feature transform)
    - Based on Interest Points 
- HOG (Histogram of Oriented Gradient)
    - Statistical feature of gradient histogram

## Deep Learning Approach 

- R-CNN (Regions with CNN)
    - Selective Search
    - Warped Image Region
    - CNN Classify
    - Border Box Regression
- Fast R-CNN
    - Shared Conv. Layer
    - 25x faster
- Faster R-CNN
    - Using RPN ( Region Proposal Network ) to select area.
    - 250x faster

# Feature Pyramid Network

## Main Idea

FPN can be used with any normal conv architecture, it can be used for classification. In such an architecture all layers have progressively decreasing spatial resolutions.

FPN would now take C5 and convolve with 1x1 kernel to reduce filters to give P5. Next, P5 is upsampled and merged it to C4 (C4 is convolved with 1x1 kernel to decrease filter size in order to match that of upsampled P5) by adding element wise to produce P4.

Similarly P4 is upsampled and merged with C3(in a similar way) to give P3 and so on. The final set of feature maps, in this case {P2 .. P5} are used as feature pyramids.

we provide a picture to illustrate the architecture of FPN.

![Network](https://i.imgur.com/oHFmpww.png)

## Applications

- Feature Pyramid Networks for RPN
    - RPN Anchor ( Scale, Ratio )
    - Keep Single Scale and variant Ratio.
- Feature Pyramid Networks for Fast R-CNN
    - ROI Pooling

## Discussion

This method is a generic pyramid representation and can be used in applications other than object detection. We can use FPNs to generate segmentation proposals, following the DeepMask/SharpMask framework.

According to the Test, the results demonstrate that FPN is a generic feature extractor and can replace image pyramids for other multi-scale detection problems.

# Experiments

![Experiments](https://i.imgur.com/NT70FZz.jpg)


# Reference

1.https://arxiv.org/pdf/1612.03144.pdf
2.http://blog.csdn.net/jesse_mx/article/details/54588085
3.http://www.cnblogs.com/skyfsm/p/6806246.html
4.http://www.360doc.com/content/17/0303/14/10408243_633634497.shtml