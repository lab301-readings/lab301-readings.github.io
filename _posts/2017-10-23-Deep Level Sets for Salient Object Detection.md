---

layout:     post
title:      Deep Level Sets for Salient Object Detection
subtitle:   Salient Object Detection
date:       2017-10-19
author:     Ping Hu, Bing Shuai, Jun Liu, Gang Wang
catalog: true
tags:
    - Machine Learning
    - computer vision
    - object detection

---

# Introduction
This paper is written by Ping Hu, Bing Shuai, Jun Liu, Gang Wang and this report is done by Yan Zheyu from Liao's Group in Zhejiang University. This paper uses **Level Set** method and intorduced **Guided Image Filtering** with refined VGG16 network to form a method to separate salient objects from the background under the framework of **Deep Learning**. It is weird because I didn't really recognize that the last but most important author of this paper, Gang Wang, is Prof. Wang sitting in our meeting room. It was a shame that he had left before my report started, and we really do have some questions about details and coding to ask.

---
# Abstract
Deep learning has been applied to saliency detection in recent years. The superior performance has proved that deep networks can model the semantic properties of salient objects. Yet it is difficult for a deep network to discriminate pixels belonging to similar receptive fields around the ob-
ject boundaries, thus deep networks may output maps with blurred saliency and inaccurate boundaries. To tackle such an issue, in this work, we propose a deep Level Set net- work to produce compact and uniform saliency maps. Our method drives the network to learn a Level Set function for salient objects so it can output more accurate boundaries and compact saliency. Besides, to propagate saliency in- formation among pixels and recover full resolution saliency map, we extend a superpixel-based guided filter to be a layer in the network. The proposed network has a simple structure and is trained end-to-end. During testing, the network can produce saliency maps by efficiently feedforwarding testing images at a speed over 12FPS on GPUs. Evaluations on benchmark datasets show that the proposed method achieves state-of-the-art performance.

---
# Related Work
>- **Level Set segmentation** 1988, Journal of Computational Physics
- Multiple **Traditional Methods** with contrast, color, structure, viewpoint, and central bias
- Various **Deep Saliency Networks** using CNN


---
# Pipeline of the Method  
&emsp;
![pipline of the Method][1]  
&emsp;

This method can be used and trained end to end. A full-scale input image is transformed into a smaller scale mask dividing the salient object from the background. The mask is then upsampled to meet the original scale. A Guided Superpixel Filter is then applied to it and the superpixels the method needs are pre-calculated. Finally a Heaviside Function is applied as a trick for training.

# Introduction to the Methods
## Level Set Segmentation
![Level Set Segmentation][2]

The main idea of this method is to perform segmentation by mapping the input into a space of higher dimention and do segmentation in this space.
This paper made a 2D to 3D mapping using a **Lipschitz function**[^lip] $\phi(x,y)$ define a pixel is inside the curve as $\phi(x,y)>0$ and a pixel is outside the curve as $\phi(x,y)<0$. The Lipschitz condition restricts the function to be continuous thus **the edge is more certain** and **outliers are better controlled**.
While, in the paper, this method is only applied to the model by linearly transforming the output of CNN to range (-0.5, 0.5), which seems to have no great use.

## CNN
This Convolutional Nerual Network is designed to get a salient segmentation of the input image **in a lower resolution**. The network is an extension of VGG16, which changes all pooling layers other than the first two into **dilated convolution** and preserves the size of all latter feature maps to be the same as the output.
It is quite obvious that, in this method, **paddings** are used intensively to preserve the size of feature maps. Will it cause more unwanted attention to the edge of inputs?

## Guided Superpixel Filtering
Guided Image Filtering uses one image as a filter to transform a input picture. When the input picture itself is used as the filter, the edge is better preserved.
This method uses superpixel as a filter. Superpixel itself **better preserves the edge** and guided image filtering also performs well in edge preserving, so the incorporation of the two methods tends to predict a more precise edge.  

Realization of this method seems to be tricky but a famous approach is used, which is illustrated below
<center>![G_S_F][3]</center>

A graph is created for superpixels. Nodes are the superpixels and two nodes share an edge if the corresponding superpixels share same pixels. *n-Ring* is defined as a set of superpixels whose distance in the graph to the target superpixel is n. Covolutions are done ring by ring.
*Lechao Cheng* is interested in this method and thinks superpixels have various applications. However, this paper doesn't describe how this convolution is done in coding, which is a great challenge for us. We intend to turn to Prof. Wang for help.

## Loss Function
The loss Function is defined as follows:
$L=\alpha \displaystyle\int_\Omega |H(\phi(x,y))-gt(x,y)|^2dxdy+\gamma Length(C)$
$\ \ \ +[\displaystyle\int_\Omega|\phi(x,y)-c_1|^2 H(\phi(x,y))dxdy]$
$\ \ \ +[\displaystyle\int_\Omega|\phi(x,y)-c_2|^2 (1-H(\phi(x,y)))dxdy]$
where $gt$ is ground truth, $\alpha$ and $\gamma$ are factors,
$H(x)$ is a step function which is 1 when $x\geq 0$ and is 0 otherwise,
$Length(C) = \displaystyle\int_\Omega \delta(\phi(x,y))|\nabla \phi(x,y) |dxdy$
$c_1 = \frac{\int_\Omega H(\phi(x,y))H(\phi(x,y))dxdy}{\int_\Omega H(\phi(x,y))dxdy}$
$c_2 = \frac{\int_\Omega H(\phi(x,y))(1-H(\phi(x,y)))dxdy}{\int_\Omega (1-H(\phi(x,y)))dxdy}$

The first term is a BCE Loss and in the second term, $\gamma = 0$, which maximises the length of edge.
The latter 2 terms constrains the variance of $\phi(x,y)$ for pixels in the salient object and the background.

## Heaviside Function
The task of minimizing this loss function is non-convex, so the Approximated Heaviside Function is introduced as a trick for better training, which:
$H_\epsilon(z)=\frac{1}{2}(1+\frac{2}{\pi}arctan(\frac{z}{\epsilon}))$
where $\epsilon$ is a factor.

# Training
The final network is first trained with Binary Cross Entropy(BCE) loss for 15 epochs, then fine-tuned with the proposed level set method for 15 epochs, and finally the Guided Superpixel Filtering layer is added and finetuned. They use Adam with an initial learning rate of 1e-4 to update the weights. The learning rate is reduced when validation performance stops improving. They implement the network with Torch framework.

# Conclusion
In this paper, an end-to-end deep level set network have been proposed to detect salient objects. Trained to learn a level set function instead of binary groundtruth directly, the network can deal with object boundaries more accurately. 
Furthermore, the proposed method extends the guided image filter to deal with superpixels so that saliency can be further propagated between pixels and the saliency map can be recovered to full resolution. Experiments on benchmark datasets demonstrate that the proposed deep level set network can detect salient objects effectively and efficiently.

However, from my point of view, two main questions should be asked:
1. Since Level Set Function is the method proposed in the topic, why the major property of Level Set Functions, **Lipschitz Condition** is not used.
2. In preserving the size of feature maps, padding is used, will this padding affect the edge of side and corners of this immage?
 

---
# Reference
1. [Deep Level Sets for Salient Object Detection](http://openaccess.thecvf.com/content_cvpr_2017/papers/Hu_Deep_Level_Sets_CVPR_2017_paper.pdf)


[^lip]: functions that $|f(x_1) - f(x_2)| \leq ||x_1 - x_2||$


  [1]: https://www.z4a.net/images/2017/10/19/pipeline.png
  [2]: https://www.z4a.net/images/2017/10/19/Level_Set.png
  [3]: https://www.z4a.net/images/2017/10/19/G_S-F.png