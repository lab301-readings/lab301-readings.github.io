---
layout:     post
title:      Generative Adversarial Networks
subtitle:   GAN
date:       2017-09-29
author:     Tab
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - computer vision
    - deep learning
    - GAN
---

# Outline
**Generative Model**
>- **Density estimation**
- **Sample generation**

---
# Motivation
>- **High-dimensional** probability distribution
- incorporated into **reinforcement learning**
- predict missing data **(semi-supervised learning)**
- **multi-model output** 
- **realistic generation of samples** 

![multi-model output](https://i.imgur.com/n1M2eUd.png)

**Figure 1:** In this example, a model is trained to predict the next frame in a video sequence. Here the 'multi-model' means that the possible result is not a single answer. You can see the second image uses MSE to form an answer with an average over many slightly different images. However, with an additional GAN loss, the third image can understand that there are many possible outputs and each of them is sharp, recognizable and realistic.

---
# Related Work
>- **FVBN:** In parallel or not?
- **ICA:** Generator functions has few restrictions?
- **Boltzmann machine:** Markov chains needed? 
- **VAE:** Variational bound needed? 
- Quality of samples?

![Taxonomy of Generative Models](https://i.imgur.com/5r0WeiB.png)

**Figure 2:** An illustration about alternative methods of generative models

---
**FVBN**
![FVBN](https://i.imgur.com/1dRxmmS.png)
**Figure 3:** Fully Visible Belief Nets with great generation cost

---
**ICA**

![ICA](https://i.imgur.com/DriRz5C.png)
**Figure 4:** ICA puts too much restrictions to generator functions

---
**VAE**

![VAE](https://i.imgur.com/rIvXNR1.png)
**Figure 5:** VAE is not asymptotically consistent unless q is perfect

---
# Algorithm

## The GAN framework
![GAN framework](https://i.imgur.com/i6A5MaU.png)
**Figure 6:** Generater vs. discriminator

---
## Cost function

### The discriminator's cost
$J^{(D)}((\theta)^{(D)},(\theta)^{(G)})=-\cfrac{1}{2}E_{x\sim p_{data}}logD(x)-\cfrac{1}{2}E_{z}logD(G(z))$

### The generator's cost
>- **Minimax (JS divergence)**
- **Maximum likelihood game (KL divergence)**
- **Heuristic, non-saturating game**

**Minimax (JS divergence)**
![Minimax](https://i.imgur.com/cBIOPpo.png)
**Figure 7:** Generator's minimax cost: $J^{(G)}=-J^{(D)}$

**Maximum likelihood game (KL Divergence)**

$J^{(G)}=-\cfrac{1}{2}E_ze^{\sigma^{-1}(D(G(z)))}$

**Heuristic, non-saturating game**

$J^{(G)}=-\cfrac{1}{2}E_zlogD(G(z))$

### A significant problem

**Non-convergence**

This problem usually happen when the ability of discriminator is great. (So the gradient of V is always vanishing) It is obvious that when D(G(z)) is near zero, the gradient is also near zero.
![convergence problem](https://i.imgur.com/26HACno.png)
**Figure 8:** Convergence performance of different methods
![convexity of different methods](https://i.imgur.com/KkEHJRW.png)
**Figure 9:** Convexity of different methods

**Mode collapse**
![mode collapse 1](https://i.imgur.com/5nAo9Xz.png)
**Figure 10:** Example of mode collapse, which can be avoided by unrolled GAN
![mode collapse 2](https://i.imgur.com/ehIgrTh.png)
**Figure 11:** Different results with different orders of p and q in KL divergence

---
# Experiment
**Evaluation of generative models**
![evaluation of generative models](https://i.imgur.com/mKuZyzj.png)
**Figure 12:** Evaluation of generative models. The two figures correspond with MLP and DCGAN, which shows that the cost is not pretty accurate according to the quality of pictures.

**Dicrete outputs**
>**Frontier**
- Semi-supervised learning
- Using this code

---
# Discussion
## Tips and Tricks
---
### Train with labels
- Generating condition on labels: P(y\|x)
- Training with labels: P(x, y)

---
### One-sided label smoothing
**(to reduce vulnerability to adversarial examples)**
- Default discriminator cost
```
cross_entropy(1., discriminator(data))+cross_entropy(0.,discriminator(samples))
```
- One-sided label smooth cost
```
cross_entropy(.9, discriminator(data))+cross_entropy(0.,discriminator(samples))
```
- Do not smooth negative labels

$D^*(x)=\cfrac{(1-\alpha)p_{data}(x)+\beta p_{model}(x)}{p_{data}(x)+p_{model}(x)}$

According to the discriminator's loss, the best discriminator should implement the function $D^*(x)$. So if we only decrease the coefficient of $p_{data}$, it'll just have the confidence of D decline. However, if we get a positive $\beta$, it is likely that there exists a peak (local optimum value)  which leads to a trend of generating wrong samples.

---

### Virtual Batch Normalization

![Virtual Batch Normalization](https://i.imgur.com/YEZJoPQ.jpg)
**Figure13:** correlation of samples in a batch caused by batch normalization

>- Batch Norm
(cause correlation of pictures in the single batch)
- Reference Norm
(Fix a reference batch R for normalization)
- Virtual Batch Norm
(avoid overfit in Reference Norm)

![steps of VBN](https://i.imgur.com/kiTzaWH.png)
**Figure14:** Steps of Virtual Batch Normalization


## Balance G and D?
>- D always performs better(in accordance with theory)
- The network of D is bigger and deeper
- Do not limit D when it is too accurate


# Reference
1. [Ian Goodfellow(2016). GAN Tutorial. In NIPS 2016](https://arxiv.org/pdf/1701.00160.pdf)
2. [Slide for the tutorial.](http://www.iangoodfellow.com/slides/2016-12-04-NIPS.pdf)
