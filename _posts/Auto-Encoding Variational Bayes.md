---
layout:     post
title:      Auto-Encoding Variational Bayes
subtitle:   VAE
date:       2017-10-03
author:     Yan Zheyu
catalog: true
tags:
    - Mathematical derivation
    - VAE

---
#Outline

[TOC]

---
# Introduction
This paper is written by Diederik P. Kingma, Max Welling and this report is done by Yan Zheyu from Liao's Group in Zhejiang University. Because the writer of paper *Tutorial on Variational Autoencoder, C. Doersch, 2016* has a deep insight of **VAE**, this report will not focus on how to understand **VAE**, but will put more efforts on the mathematical part. 

---
# Abstract
How can we perform efficient inference and learning in directed probabilistic models, in the presence of continuous latent variables with intractable posterior distributions, and large datasets? We introduce a stochastic variational inference and learning algorithm that scales to large datasets and, under some mild differentiability conditions, even works in the intractable case. Our contributions is two-fold. First, we show that a reparameterization of the variational lower bound yields a lower bound estimator that can be straightforwardly optimized using standard stochastic gradient methods. Second, we show that for i.i.d. datasets with continuous latent variables per datapoint, posterior inference can be made especially efficient by fitting an approximate inference model (also called a recognition model) to the intractable posterior using the proposed lower bound estimator. Theoretical advantages are reflected in experimental results.

---
# Related Work
>- **Wake-sleep Algorithm:** 1995, Science
- **Stochastic Variational Inference:**  2013
- **Directed Probabilistic Models:** Advances in neural information processing systems
- **DARN:** Deep autoregressive networks

---
# Flowchart of VAE  
&emsp;
![Flowchart of VAE][1]
<center>**Fiure1 Flowchart of VAE**[^Flowchart]</center>
## Description
During the training part, we start with a input vector **x**. After the encoder, we get the mean of **z** (μ), and var of **z** in the log form. We use the **reparameterize trick** intorduced in the next section to reparmaeterize a **randomly sampled** **z** from a certain distribution, and put the reparameterized **z** into the Decoderto get the output. We hope the output to be similar to the input.
In the test part, the Encoder is neglected and a  random input is given to the decoder. [^MoreDes] 

---
# Mathematical Derivation of the Method
Let $p_α(\boldsymbol\theta)$ be some  hyperprior for the parameters introduced above, parameterized by α. The
marginal likelihood can be written as:  
<center>$
\log p_α(\boldsymbol{X}) = D_{KL}(q_Φ(\boldsymbol{\theta}) ||p_α(\boldsymbol{\theta|X})) + L(Φ;\boldsymbol{X})\ \ \ \ (1)
$</center>

where the first RHS term denotes a KL divergence of the approximate from the true posterior, and
where $L(\phi;\boldsymbol X)$ denotes the variational lower bound to the marginal likelihood:
<center>
$L(\phi;\boldsymbol X)=\int q_\phi (\boldsymbol \theta)(\log p_\boldsymbol \theta (\boldsymbol X) + \log p_\alpha(\boldsymbol \theta) - \log q_\phi(\boldsymbol \theta)\ d\boldsymbol \theta\ \ \ \ (2)$
</center>

Note that this is a lower bound since the KL divergence is non-negative; the bound equals the true
marginal when the approximate and true posteriors match exactly. The term $\log p_\theta(\boldsymbol X)$ is composed of a sum over the marginal likelihoods of  individual datapoints $\log p_\theta(\boldsymbol X) = \sum_{i=1}^N \log p_\theta(\boldsymbol x^{(i)})$, which can each be rewritten as:
<center>
$\log p_\boldsymbol \theta(\boldsymbol x^{(i)}) = D_{KL}(q_\phi(\boldsymbol{z}\ |\ \boldsymbol x ^{i})||p_\boldsymbol \theta(\boldsymbol z\ |\ \boldsymbol x^{(i)}))\ +\ L(\boldsymbol \theta, \phi,\boldsymbol x^{i})\ \ \ \ (3)$
</center>

where again the first RHS term is the KL divergence of the approximate from the true posterior, and
$L(\boldsymbol \theta; \phi; \boldsymbol x)$ is the variational lower bound of the marginal likelihood of datapoint i:
<center>$
L(\boldsymbol \theta,\phi,\boldsymbol x^{(i)})\ =\ \int q_\phi(\boldsymbol z|\boldsymbol x)(\log p_\boldsymbol \theta(\boldsymbol x^{(i)}|\boldsymbol z)+\log p_\boldsymbol \theta(\boldsymbol z)-\log q_\phi(\boldsymbol z|\boldsymbol x)) d\boldsymbol z \ \ \ \ (4)
$</center>

The expectations on the RHS of eqs (2) and (4) can obviously be written as a sum of three separate expectations, of which the second and third component can sometimes be analytically solved, e.g. when both $p_\boldsymbol \theta(\boldsymbol x)$ and $q_\phi(\boldsymbol z|\boldsymbol x)$ are Gaussian.
For generality we will here assume that each of these
expectations is intractable. Under certain mild conditions outlined in section (see paper) for chosen approximate posteriors $q_\phi(\boldsymbol \theta)$ and $q_\phi(\boldsymbol z|\boldsymbol x)$ we can reparameterize conditional samples $\tilde{z} \sim q_\phi(\boldsymbol z|\boldsymbol x)$ as
<center>$\tilde z = g_\phi(\boldsymbol\epsilon,\boldsymbol x) with \boldsymbol\epsilon \sim p(\boldsymbol\epsilon)\ \ \ \  (5)$</center>

where we choose a prior $p(\boldsymbol\epsilon)$ and a function $g_\phi(\boldsymbol\epsilon, \boldsymbol x)$ such that the following holds:
<center>$\displaystyle{L(\boldsymbol
\theta, \phi, \boldsymbol x^{(i)}) = \int q_\phi(\boldsymbol z|\boldsymbol x)\left(\log p_\boldsymbol\theta(\boldsymbol x^{(i)}|\boldsymbol z) + \log p_\boldsymbol\theta(z) − \log q_\phi(\boldsymbol z|\boldsymbol x)\right) d\boldsymbol z}$</center>

<center>
$\displaystyle{= \int p(\boldsymbol\epsilon) \left(\log p_\boldsymbol\theta(\boldsymbol x^{(i)}|\boldsymbol z) + \log p_\boldsymbol \theta(\boldsymbol z) − \log q_\phi(\boldsymbol z|\boldsymbol x)\right)\bigg | _{\boldsymbol z=g_\phi(\boldsymbol \epsilon,\boldsymbol x^{(i)})} d\boldsymbol\epsilon} \ \ \ \ (6)$
</center>

The same can be done for the approximate posterior $q_\phi(\boldsymbol\theta)$:
<center>$\tilde\theta = h_\phi(\boldsymbol\zeta)\ $ with $\ \boldsymbol\zeta \sim p(\boldsymbol\zeta)\ \ \ \  (7)$</center>

where we, similarly as above, choose a prior $p(\boldsymbol\zeta)$ and a function $h_\phi(\boldsymbol\zeta)$ such that the following holds:
<center>$\displaystyle{L(\phi,\boldsymbol X) = \int q_\phi(\boldsymbol\theta) \left(\log p_\boldsymbol\theta(\boldsymbol X) + \log p_\alpha(\boldsymbol \theta) − \log q_\phi(\boldsymbol\theta)\right) d\boldsymbol\theta}$</center>
<center>$\displaystyle{
= \int p(\boldsymbol\zeta) \left(\log p_\boldsymbol\theta(\boldsymbol X) + \log p_\alpha(\boldsymbol\theta) − \log q_\phi(\boldsymbol\theta)\right) \bigg| _{\boldsymbol\theta=h_\phi(\boldsymbol\zeta)} d\boldsymbol\zeta (8) }$
</center>

For notational conciseness we introduce a shorthand notation $f_\phi(\boldsymbol x,\boldsymbol z,\boldsymbol \theta)$:
<center>$f_\phi(\boldsymbol x,\boldsymbol z,\boldsymbol \theta) = N · (\log p_\boldsymbol\theta(\boldsymbol x|\boldsymbol z) + \log p_\boldsymbol\theta(\boldsymbol z) − \log q_\phi(\boldsymbol z|\boldsymbol x)) + \log p_\alpha(\boldsymbol\theta) − \log q_\phi(\boldsymbol\theta) (9)$</center>

Using equations (8) and (6), the Monte Carlo estimate of the variational lower bound, given datapoint $\boldsymbol x^{(i)}$, is:

<center>
$\displaystyle{L(\phi,\boldsymbol X) \approx \cfrac{1}{L}
\sum^L_{l=1}f_\phi\left(x^{(l)}, g_phi(\boldsymbol\epsilon^{(l)},\boldsymbol x^{(l)}), h_\phi(\boldsymbol\zeta^{(l)})\right)\ \ \ \  (10)}$</center>

where $\boldsymbol\epsilon^{(l)} \sim p(\boldsymbol\epsilon)$ and $\boldsymbol\zeta^{(l)} \sim p(\boldsymbol\zeta)$. The estimator only depends on samples from $p(\boldsymbol\epsilon)$ and $p(\boldsymbol\zeta)$ which are obviously not influenced by $\phi$, therefore the estimator can be differentiated w.r.t. $\phi$.
The resulting stochastic gradients can be used in conjunction with stochastic optimization methods such as **SGD** or **Adagrad** [^DHS10]. See algorithm 1 for a basic approach to computing stochastic gradients.

---
# Reference
1. [Auto-Encoding Variational Bayes](https://arxiv.org/abs/1312.6114)
2. [aotoencorder理解(5):VAE（Variational Auto-Encoder，变分自编码器）](http://blog.csdn.net/u011534057/article/details/55045470)


  [1]: https://i.imgur.com/qyAIDec.png
  [^Flowchart]: http://blog.csdn.net/u011534057/article/details/55045470
  
  [^MoreDes]: You can get more in *Tutorial on Variational Autoencoder*, C. Doersch, 2016.
  
  [^DHS10]: John Duchi, Elad Hazan, and Yoram Singer. Adaptive subgradient methods for online learning and stochastic optimization. Journal of Machine Learning Research, 12:2121–2159, 2010.