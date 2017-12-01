---
layout:     post
title:      Adversarial Examples Detection in Deep Networks with Convolutional Filter Statistics
subtitle:   AdvDetect
date:       2017-11-26
author:     zcy
catalog: true
tags:
    - Neural Network
    - Adversarial Examples
    - Detection
---
# Summary

+ Two Types of Adversarial Example
    - LBFGS-adversarials
    - EA-adversarials

+ A Hypothesis Explanation
    - Test-set should be Interpolation of Train-set
    - Adversarial Example are usually Extrapolation

- Some About NN
    - NN is good at performing Interpolation
    - It is difficult to perform Extrapolation

- Detect Adversarial Examples
- Convert to detect Interpolation or Extrapolation
and only predict in Interpolation area
    - But, it is hard to detect it in high-dimension like image

- Overview Previous Approach
    - Treat NN like black-box, only use final output Approach in This paper
    - Try to use some intermediate information like Neural Style Transfer or Feature Loss
- A Statistical Approach
    - Analysis the distribution of output of one layer using PCA to emphasis important component
    * ![Stat1](https://imgur.com/7jEje9v.png)
    - Apparently, There are some statistical feature Compute Mean & Std
    * ![Stat2](https://imgur.com/2Us719C.png)
    - Strong Regularization Effect Using Threshold to filter Samples

- Refinement Cascade Classifier
    - Using A series of SVM when the result of previous one is not confident enough    

- Some Similar Work 
    - SafetyNet by Jiajun Lu et al. ICCV17
        -   Using VGG + RBF-SVM
        -   Complex & Better Performance
    - Jan et al. ICLR17
        - Using subnetwork attached to original NN More similar to this work

- Some Related Work 
    - DeepFool by Seyed-Mohsen et al. CVPR 16
     - A way for generating adversarial samples 
    - Jiajun Lu et al. CVPR workshop & arxiv
        - Perturbations in Physical World is not severe 
        - For different distance & angle