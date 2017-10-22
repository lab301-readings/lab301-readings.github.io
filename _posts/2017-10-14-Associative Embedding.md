---
layout:     post
title:      Associative Embedding
subtitle:   joint detection and grouping
date:       2017-10-18
author:     Stephen Zhou
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - computer vision
    - Instance Segmentation
    - Multiperson pose estimation

---

# Related work
**Vector embeddings**
**Perceptual organization**
>- group pixels into parts
- detecting basic visual units first and grouping them second. our approach performs detection and grouping in one stage

**Multiperson pose estimation**
>- top-down
>- bottom-up

**Instance Segmentation**
>- do detection followed by segmentation
- Two recent works , DeepMask, Instance-Sensitive FCN

![](https://i.imgur.com/zyToSS9.png)
**Figure 1:** DeepMask network

![](https://i.imgur.com/xUofSIn.png)
**Figure 2:** Instance-Sensitive FCN network

![](https://i.imgur.com/qRwPSQU.png)
**Figure 3:** Instance-Sensitive FCN 

# Stacked Hourglass Architecture 
>- combine associative embedding with the stacked hourglass architecture
- repeated bottom-up and top- down
- consolidate global and local features

![](https://i.imgur.com/k3kg73x.png)
**Figure 4:** Stacked Hourglass Architecture 

![](https://i.imgur.com/JZ9cu0K.png)
**Figure 5:** Stacked Hourglass Architecture 

![](https://i.imgur.com/VfjxmUg.png)
**Figure 6:** Stacked Hourglass Architecture 

# Multiperson Pose Estimation
>- m detection  heatmap and m tag heatmap
- Detection loss : MSE
- Grouping loss:
![](https://i.imgur.com/1Saog1B.png)
![](https://i.imgur.com/VvWS2FP.png)

![](https://i.imgur.com/jQC4zEI.png)
**Figure 7:** An overview of our approach for producing multi-person pose estimates

# Experiments of Multiperson Pose Estimation
Dataset: MS-COCO  and MPII Human Pose
![](https://i.imgur.com/389nKD3.jpg)
**Figure 8:** visualize the associative embedding channels for different joints

![](https://i.imgur.com/s3TIKUM.png)
**Figure 9:** Results (AP) on MPII Multi-Person

![](https://i.imgur.com/GNnbRu7.png)
**Figure 10:** Results on MS-COCO test-std, excluding systems trained with external data

![](https://i.imgur.com/rN9fNeI.png)
**Figure 11:** Results on MS-COCO test-dev, excluding systems trained with external data

![](https://i.imgur.com/b6yXsoM.jpg)
**Figure 12:** Qualitative pose estimation results on MSCOCO validation images

# Instance Segmentation
**detection loss:** MSE between the predicted heatmap and the ground truth heatmap (the union of all instance masks)
**grouping loss:** 
![](https://i.imgur.com/9KXgfwO.png)

![](https://i.imgur.com/jNhdqBH.png)
**Figure 13:** instance segmentations' work

# Experiment of instance segmentation
Dataset: val split of PASCAL VOC 2012
Pretrained on MS COCO
![](https://i.imgur.com/9jNIz6d.png)
**Figure 14:** Example instance predictions produced by our system on the PASCAL VOC 2012 validation set

![](https://i.imgur.com/x2tWfTb.png)
**Figure 15:** Semantic instance segmentation results (mAP) on PASCAL VOC 2012 validation images

# conclusion
>-

 - 

列表项
---

 introduce associative embedding, a new method for single- stage, end-to-end joint detection and grouping
- associative embedding can be easily integrated with other state-of- the-art architectures that produces pixelwise predictions
- apply associative embedding to multiperson pose estimation and achieve state of the art results on two standard benchmarks



