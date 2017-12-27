---
layout:     post
title:      Neural Color Transfer between Images
subtitle:   Neural Color Transfer
date:       2017-12-09
author:     Tab
catalog: true
tags:
    - color transfer
    - local affine transform
    - patchmatch
---

# Outline
![](https://i.imgur.com/zpjGGL3.png)
![](https://i.imgur.com/kRnDlmt.png)
![](https://i.imgur.com/0jvjvu7.png)

---

# Related Work
![](https://i.imgur.com/MHXgYc7.png)
![](https://i.imgur.com/3NIEN8J.png)

>- Here, we further discuss some details of the paper "Visual Attribute Transfer through Deep Image Analogy". Considering that this paper got some ideas from it, such as NNF in the feature space and the coarse-to-fine reconstruction method, this paper compares itself with the "Analogy" paper both in approaches and results.

![](https://i.imgur.com/HTQjFsE.png)
![](https://i.imgur.com/5lIMnXG.png)

>- In fact, this paper implemented a more reasonable method, instead of just simply upsampling the patchmatch result, when reconstructing the image. Besides, the local affine transform (locally linear model) also helps when getting better edges in the final images.

---

# Algorithm
![](https://i.imgur.com/I8pIQY6.png)
![](https://i.imgur.com/eEVflvz.png)

>- We can see from the figure: patchmatch results in deep feature space can better satisfy our needs. So it's a natural thought that we can reconstruct the image from coarse to fine.

![](https://i.imgur.com/MGphLs4.png)

>- This figure offers us an intuitive feeling that color transfer in the image domain can work much efficiently than in the feature domain.

![](https://i.imgur.com/e9TUnnF.png)
![](https://i.imgur.com/qoToceC.png)
![](https://i.imgur.com/ha5a6g2.png)
![](https://i.imgur.com/kwegkfJ.png)
![](https://i.imgur.com/qtaFK4P.png)
![](https://i.imgur.com/yfovuXx.png)
![](https://i.imgur.com/UozbzXf.png)
![](https://i.imgur.com/gmhBjz7.png)
![](https://i.imgur.com/tuOp85h.png)
![](https://i.imgur.com/A2X3mds.png)
![](https://i.imgur.com/77VSM5y.png)
>- How to merge: for every pixel in G, find a pixel in Gi so that it matches best. At last, we can merge all the guidance map and get a multi-reference guidance map. IL can be considered as one choice that how to merge the guidance map. Different colors in it represent different guidance map choices for pixels.

---

# Experiments
![](https://i.imgur.com/nrkSVHe.png)
![](https://i.imgur.com/wTnbPBP.png)
![](https://i.imgur.com/9UUYhfa.png)
![](https://i.imgur.com/j98WrEy.png)
![](https://i.imgur.com/b0FEwYQ.png)
![](https://i.imgur.com/0vQj1aA.png)
![](https://i.imgur.com/QB60QXG.png)
![](https://i.imgur.com/qCSIJtr.png)
![](https://i.imgur.com/QunXgb7.png)
![](https://i.imgur.com/3QWqsd6.png)

---
# Discussion
![](https://i.imgur.com/s906DcW.png)

---
# Reference
1. [Neural Color Transfer between Images](https://arxiv.org/pdf/1710.00756.pdf)
2. [Visual Attribute Transfer through Deep Image Analogy](https://arxiv.org/pdf/1705.01088.pdf)
3. [Deep Photo Style Transfer](https://arxiv.org/pdf/1703.07511.pdf)
