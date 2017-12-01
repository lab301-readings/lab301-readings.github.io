---
layout:     post
title:      Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network 
subtitle:   Super-Resolution 
date:       2017-11-26
author:     Daxin
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - deep learning
    - Super-resolution
    - GAN
---

# Introduction
- **Super-Resolution** is a task of estimating a high-resolution image from its low-resolution counterpart, which is a ill-posed problem. Many supervised SR algorithms recently proposed use the minimization of the mean squared error between the recovered HR image and the ground truth as their optimization target. Although it can maximizes the peak signal-to-noise ratio(PSNR), the results always lack of high texture detail, which means high PSNR does not necessarily reflect the perceptually better SR result. 
- This paper proposes a new state of the art for image SR with high upscaling factors(4×). It proposes SRGAN optimized for a new perceptual loss.

The result of this work is as follows. 

![](https://ws3.sinaimg.cn/large/006tKfTcgy1flzeg0ee59j30k009ttb6.jpg)

---
# Method

## Ultimate goal
The ultimate goal is to train a generating function `G` that estimates for a given LR input image its corresponding HR counterpart. To achieve this, a generator network is trained as a feed-forward CNN `GθG` parametrized by `θG`.

![](https://ws4.sinaimg.cn/large/006tKfTcgy1flzehab64yj30bm02aglk.jpg)

Here `θG` = {`W1:L` ; `b1:L`} denotes the weights and biases of a L-layer deep network and is obtained by optimizing a SR-specific loss function `lSR`. `ILR` is the LR image and `IHR` is the HR image.

## Adversarial network architecture 
The architecture network is clear and easy to understand.

![](https://ws1.sinaimg.cn/large/006tKfTcgy1flzenycdifj30k00b4mzd.jpg)

## Perceptual loss
The definition of the perceptual loss function `lSR` is critical for the performance of the generator network. It contains two parts. One is called *content loss* and the other is called *adversarial loss*.

![](https://ws4.sinaimg.cn/large/006tKfTcgy1flzepra6fij30fy04laab.jpg)
### Content loss
MSE is the most widely used optimization target for image SR on which many state-of-the-art approaches.

![](https://ws2.sinaimg.cn/large/006tKfTcgy1flzerplp6mj30ba02at8o.jpg)

But solutions of MSE optimization problems often lack high-frequency content. Here define the VGG loss as the euclidean distance between the feature representations of a reconstructed image `GθG(ILR)` and the reference image `IHR`.

![](https://ws2.sinaimg.cn/large/006tKfTcgy1flzeuwpmbuj30c103pdfw.jpg)

`φi,j` indicates the feature map obtained by the j-th convolution (after activation) before the i-th maxpooling layer within the VGG19 network. `Wi,j` and `Hi,j` describe the dimensions of the respective feature maps within the VGG network.

### Adversarial loss
This encourages our network to favor solutions that reside on the manifold of natural images, by trying to fool the discriminator network. The generative loss is defined based on the probabilities of the discriminator `DθD(GθG (ILR))` over all training samples as:

![](https://ws1.sinaimg.cn/large/006tKfTcgy1flzexmdoy0j30aa02n748.jpg)

---
# Experiment
## Mean opinion score (MOS) testing 
Mean opinion score (MOS) testing is a test for human to verify which image is more perceptual. The testers were asked to assign an integral score from 1 (bad quality) to 5 (excellent quality) to the super-resolved images. 
Different algorithms' result is as follows.

![](https://ws4.sinaimg.cn/large/006tKfTcgy1flzf1t4b7dj30e009hjsd.jpg)

The result form SRGAN is definitely best of all.

## Different loss function

![](https://ws3.sinaimg.cn/large/006tKfTcgy1flzf3vi40xj30bo08smyd.jpg)

## Different methods

![](https://ws3.sinaimg.cn/large/006tKfTcgy1flzf4j372pj30k007fgn5.jpg)

## Visual Results

![](https://ws3.sinaimg.cn/large/006tKfTcgy1flzf5qw4scj30jk09fdig.jpg)

![](https://ws4.sinaimg.cn/large/006tKfTcgy1flzf7bqjvsj30jz0ao3zw.jpg)

![](https://ws3.sinaimg.cn/large/006tKfTcgy1flzf7m9s4gj30jb0bgq4g.jpg)

# Conclusion and Discussion
## Contribution

- a new state of the art for image SR with high upscaling factors (4×) 
- propose SRGAN optimized for a new perceptual loss.

## Discussion	
- computational efficiency 
- deeper networks (B > 16) increase the performance of SRResNet 
-  SRGAN variants of deeper networks are increasingly difficult to train due to the appearance of high-frequency artifacts. 


# Reference
1. [Ledig C, Theis L, Huszar F, et al. Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network[J]. 2016.](https://arxiv.org/abs/1609.04802)

