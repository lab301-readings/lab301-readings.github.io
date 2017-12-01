---
layout:     post
title:      Globally and Locally Consistent Image Completion
subtitle:   inpainting
date:       2017-10-07
author:     stephen_zhou
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - computer vision
    - deep learning
    - inpainting
---


# Related work 
>- diffusion-based image synthesis: histograms of local features
- patch-based approaches: PatchMatch
- CNN-based: limited to very small and thin masks
- context encoder-based approach: based on GAN
![](https://i.imgur.com/jIzptHN.png)
**Figure 1:** network of context encoder-based approach

# Convolutional neural networks
- A  completion network

- Two additional networks: the global and the local context discriminator networks

- dilated convolution: 99x99 to 307x307
![](https://i.imgur.com/OcADlDi.png)
**Figure 2:** network of this paper
![](https://i.imgur.com/gkkTtiN.png)

![](https://i.imgur.com/lDo7KwN.png)

![](https://i.imgur.com/IC2Wr8E.png)

![](https://i.imgur.com/eQFEtA2.png)


# Loss function
- MSE loss:
![](https://i.imgur.com/aj6tvnY.png)
- GAN loss:
![](https://i.imgur.com/K1HyGmR.png)
- Total loss: 
![](https://i.imgur.com/0zwIDTR.png)

# Trianing algorithm
![](https://i.imgur.com/nYbpXVW.png)

# Experiment
## Comparison with ExistingWork
### Arbitrary Region Completion
![](https://i.imgur.com/Cx9uQHx.png)

### Center Region Completion 
![](https://i.imgur.com/0RrbogC.png)

## Global and Local Consistency
![](https://i.imgur.com/qLQqt7c.png)

## Object Removal
![](https://i.imgur.com/RqQAHlu.png)

## Faces and Facades
**dataset:** 

- CelebFaces Attributes Dataset: 202, 599
- the CMP Facade dataset: 606

![](https://i.imgur.com/4WvBWhe.jpg)

# Limitations and Discussion
- square masks, especially when inpainting mask is at the border of the image
![](https://i.imgur.com/XvBFI9K.png)
![](https://i.imgur.com/qeMa04l.png)

- heavily structured object, is partially masked
![](https://i.imgur.com/rxeWHXl.png)
** Figure 3:** User study on the [Hays and Efros 2007] dataset. We compare Ground Truth (GT) images, [Hays and Efros 2007] (Hays), CE [Pathak et al. 2016], and our approach


