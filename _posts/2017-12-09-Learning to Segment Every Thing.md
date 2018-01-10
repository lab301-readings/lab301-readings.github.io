layout:     post
title:      Learning to Segment Every Thing
subtitle:   weight transfer
date:       2017-12-09
author:     stephen_zhou
catalog:    true
tags:
    - computer vision
    - deep learning
    - weight transfer
---


![result.png](http://upload-images.jianshu.io/upload_images/9376546-7b341fdb897378ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# mask rcnn
![mask_rcnn.png](http://upload-images.jianshu.io/upload_images/9376546-2dc1b01a4ca57c3c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/9376546-99532fddc5d7edd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# architecture
![architecture.png](http://upload-images.jianshu.io/upload_images/9376546-483cd79cf8324c1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# training
**Stage-wise training:** 
First stage: train a Faster R-CNN 
Second stage: train mask head and  weight transfer with box head fixed 

**End-to-end joint training:**
Jointly train the bounding box head and the mask head
But stop the gradient with the respect to $ W^c_{det} $

# Ablation Experiment 
Train on COCO by partitioning the 80 classes into sets A and B,
20/60 split , 20 contained in PASCAL VOC  and the 60 that are not 

![image.png](http://upload-images.jianshu.io/upload_images/9376546-91cc7fa6c6708676.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/9376546-b3629b9f858f430f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/9376546-136717785b5b6293.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/9376546-74aa823cf3b163ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# Large-Scale Instance Segmentation 
A from the COCO dataset, B from the Visual Genome (VG) dataset
VG: 108077 images , over 7000 category synsets annotated with object bounding boxes 
![image.png](http://upload-images.jianshu.io/upload_images/9376546-317a278663618252.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# Conclusion:
> - partially supervised
> - Weight transfer
> - Multi-task 







