---
layout:     post
title:      Conditional Variational Encoder
subtitle:   CVAE
date:       2017-09-29
author:     Chaowei FANG
catalog:    true
tags:
    - CVAE
    - machine learning
    - generative model

---

# CVAE
>- Generative model with conditional information (labels or attributes)
- Training  
input: sample X and condition Y  
output: X  
<img src="https://ws4.sinaimg.cn/large/006tKfTcgy1fk9wvkt94sj30go0gsn1p.jpg" width="400">
- Testing  
input: condition Y and Gaussian noise vector z  
output: reconstructed result  
<img src="https://ws2.sinaimg.cn/large/006tKfTcgy1fk9wvlhwjfj30hc09yq4c.jpg" width="350">

# Examples
>- Attribute-conditioned image generation  
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fk9wvilcg5j310m0ioh61.jpg)
- Learning diverse image colorization  
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fk9wvhf87vj31140h8wrb.jpg)
- Forecasting from static images  
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fk9wvgjemsj30te0bi7gm.jpg)
- Facial expression editting  
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fk9wvjbj6tj30v20c6wkz.jpg)

# Reference
1. Yan X, Yang J, Sohn K, et al. Attribute2Image: Conditional Image Generation from Visual Attributes[J]. european conference on computer vision, 2015: 776-791.
2. Deshpande A, Lu J, Yeh M C, et al. Learning Diverse Image Colorization[J]. 2016.
3. Walker J, Doersch C, Gupta A, et al. An Uncertain Future: Forecasting from Static Images Using Variational Autoencoders[M]// Computer Vision – ECCV 2016. Springer International Publishing, 2016.
4. Yeh R, Liu Z, Goldman D B, et al. Semantic Facial Expression Editing using Autoencoded Flow[J]. 2016.
5. Doersch C. Tutorial on Variational Autoencoders[J]. 2016.
