---
layout:     post
title:      Characterizing and Improving Stability in Neural Style Transfer
subtitle:   Stable Video Color Transfer
date:       2017-12-25
author:     Tab
catalog: true
tags:
    - Style transfer
    - Stability
    - Optical Flow
---

# Outline

[![10ccb9c4b200b339a.png](https://www.z4a.net/images/2017/12/27/10ccb9c4b200b339a.png)](https://www.z4a.net/image/rTXDG)
[![2be85fec6ab4d7706.png](https://www.z4a.net/images/2017/12/27/2be85fec6ab4d7706.png)](https://www.z4a.net/image/rTs2I)
[![36873134ae5d631f5.png](https://www.z4a.net/images/2017/12/27/36873134ae5d631f5.png)](https://www.z4a.net/image/rTB6E)

>- This is a work of L.Fei-fei's group, which was admitted in the ICCV17. It extends some ideas of perceptual loss and style transfer based on feed-forward network to video style transfer.
- Its stability analysis is a beautiful theory, which can provide us an insight of the reason consecutive frames have some unstable appearance.
- The recurrent convolutional network is also a point which deserves us to discuss.


---

# Related Work
[![4298f87615d14792c.png](https://www.z4a.net/images/2017/12/27/4298f87615d14792c.png)](https://www.z4a.net/image/rT0MU)
[![5.png](https://www.z4a.net/images/2017/12/27/5.png)](https://www.z4a.net/image/rT4of)

> Here, we conclude some popular work and methods used in style transfer. To compare these methods, I list a table, which illustrates the relations of these papers. At last, I show a figure to help you recall the network of the group's previous work.

---

# Analysis
[![6.png](https://www.z4a.net/images/2017/12/27/6.png)](https://www.z4a.net/image/rTJwp)
[![7.png](https://www.z4a.net/images/2017/12/27/7.png)](https://www.z4a.net/image/rT1ci)

> At the beginning, let's recall the loss in image style transfer. Note the gram matrix representation. It is closely related to our discussion later.
 
[![8.png](https://www.z4a.net/images/2017/12/27/8.png)](https://www.z4a.net/image/rTLrA)

As the gram matrix and objective function shows, one of our goals is to minimize the style loss. Here we ignore the index j for convenience. Then we can first consider the simplest situation, let C=W=H=1, then the solution must be $\Phi_p=\pm\Phi_s$ as the first picture in Figure2 shows. Next, let H=2, then we generalize it to the 2D case. Also we can do the same thing in the N-D space. It's easy to understand the relation by the Figure2 and the following Theory. Then we will prove the theory.

[![9.png](https://www.z4a.net/images/2017/12/27/9.png)](https://www.z4a.net/image/rT84J)
[![10.png](https://www.z4a.net/images/2017/12/27/10.png)](https://www.z4a.net/image/rTC5O)

> According to Figure 3, the theory is reasonable in the author's experiment.

---

# Algorithm
[![11ed45b058e1f8fb77.png](https://www.z4a.net/images/2017/12/27/11ed45b058e1f8fb77.png)](https://www.z4a.net/image/rTf2K)

> First, let's see the network in details. Note that the previous frame output will be concatenated with the next frame content. And there is a new loss named FLOW LOSS, which measures the temporary consistency. 

[![12.png](https://www.z4a.net/images/2017/12/27/12.png)](https://www.z4a.net/image/rTiOr)
[![13.png](https://www.z4a.net/images/2017/12/27/13.png)](https://www.z4a.net/image/rTG6a)

> To be more specific about flow loss, here is an illustration. It is based on the brightness constancy constraint. As for the mask, it is used to avoid artifacts at motion boundaries.


---

# Experiments
[![14.png](https://www.z4a.net/images/2017/12/27/14.png)](https://www.z4a.net/image/rToM0)
> Here the optim method refers to the paper 'Artistic Style Transfer for Videos'.
[![15.png](https://www.z4a.net/images/2017/12/27/15.png)](https://www.z4a.net/image/rTvvj)
[![16.png](https://www.z4a.net/images/2017/12/27/16.png)](https://www.z4a.net/image/rTaiT)
[![17.png](https://www.z4a.net/images/2017/12/27/17.png)](https://www.z4a.net/image/rTDrv)
[![18.png](https://www.z4a.net/images/2017/12/27/18.png)](https://www.z4a.net/image/rTZxP)
[![19.png](https://www.z4a.net/images/2017/12/27/19.png)](https://www.z4a.net/image/rTFQn)
[![20.png](https://www.z4a.net/images/2017/12/27/20.png)](https://www.z4a.net/image/rTR5C)
[![21.png](https://www.z4a.net/images/2017/12/27/21.png)](https://www.z4a.net/image/rTrMN)
> Because we do not need optical flow information when testing (only used in training instead), the result of the network is not likely to be influenced by the accuracy of optical flow results, whereas the optim method can't work without it.
[![22.png](https://www.z4a.net/images/2017/12/27/22.png)](https://www.z4a.net/image/rTlVw)

---
# Discussion

[![23.png](https://www.z4a.net/images/2017/12/27/23.png)](https://www.z4a.net/image/rTK96)

---
# Reference
1. [Characterizing and Improving Stability in Neural Style Transfer](https://arxiv.org/pdf/1705.02092.pdf)
2. [Perceptual Losses for Real-Time Style Transfer and Super-Resolution](https://arxiv.org/pdf/1603.08155.pdf)
3. [Artistic style transfer for videos](https://arxiv.org/pdf/1604.08610.pdf)
