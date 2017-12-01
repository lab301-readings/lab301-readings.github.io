layout:     post
title:      Adversarial Image Perturbation for Privacy Protection, A Game Theory Perspective
subtitle:   AIP
date:       2017-11-27
author:     Yan Zheyu
catalog: true
tags:
    - Adversarial Image Perturbatio
    - Game Theory
    - Neural Networks

---

#Outline

[TOC]

---

# Introduction
This is a work of Max Planck Institute for Informatics, Saarland Informatics Campus, Saarbrücken, Germany. It introduced a game between **users** who use Adversarial Image Perturbation to protect their personal identity and **recognizers** which take counter measures to get better recognition results. The authors test all combination of methods of the users' and the recognizers' and analysis the results.

---

# Abstract
>Users like sharing personal photos with others through social media. At the same time, they might want to make automatic identification in such photos difficult or even impossible. Classic obfuscation methods such as blurring are not only unpleasant but also not as effective as one would expect [28, 37, 18]. Recent studies on adversarial image perturbations (AIP) suggest that it is possible to confuse recognition systems effectively without unpleasant artifacts. However, in the presence ofcounter measures against AIPs [7], it is unclear how effective AIP would be in particular when the choice ofcounter measure is unknown. Game theory provides tools for studying the interaction between agents with uncertainties in the strategies. We introduce a general game theoretical framework for the user-recogniser dynamics, and present a case study that involves current state of the art AIP and person recognition techniques. We derive the optimal strategy for the user that assures an upper bound on the recognition rate independent of the recogniser’s counter measure. Code is available at https://goo.gl/hgvbNK.

---

# Related Work
>- **Privacy and computer vision:** traditional methods including *blur, darkening, camouflage glasses* are not useful when recognizers deploy CNN
- **AIP:** Intriguing properties of neural networks. In ICLR, 2014.
- **Robust classification against AIPs - pre-convnets:** A robust minimax approach to classiﬁcation. JLMR, 2003.
- **Robust classification against AIPs - simple transfer:**  Assessingthreat of adversarial examples on deep neural networks. In ICMLA, 2016. 
- **Robust AIPs against classifiers:** Real and stealthy attacks on state-of-the-art face recognition. In SIGSAC, 2016. 
- **Person recognition models:** Person recognition in personal photo collections. In ICCV, 2015. 


---

# User-Recogniser Game
## Two-Person Constant-Sum Games
The game can be seen as a Two-Person Constant-Sum Game, which means there are two players, and the sum payoffs of the players is a constant value.
We take this game as this: 

* There are two players, **user** and **recognizer**
* The **payoff** of recognizers is its accuracy (True Samples / All Samples) and **payoff** of users is 1 - accuracy. It is obvious that this game is constant-sum.

We have a theorem proposed by von Neumann, 1928:
$\displaystyle{p(\Theta ^{u\star},\Theta^{r})\leq p(\Theta ^{u\star},\Theta^{r\star})\leq p(\Theta ^{u},\Theta^{r\star}) \ \ \ \forall \Theta ^{u},\Theta^{r}}$

In this case p is **payoff** of the recognizer, $\Theta^u$ is the **strategy space** of user and $\Theta^r$ is the **strategy space** of recognizer. The followed $\star$ denotes the best **strategy space** for the players.

## Game Features
* **Fixed Model.** We assume that U and R use a fixed model $\displaystyle{f}$. 
* **Known model**  Each player is aware that the opponent uses $\displaystyle{f}$. *It is reasonable because users and recognizers often use know models and all models based on convnet are similar.*
* **Payoff** R's payoff is the recognition rate on the test set:
$\displaystyle{p_{ij} = \mathop{\mathbb P} \limits_{(\hat x,\hat y)\thicksim D}\Big[\mathop{\arg \max}\limits_y f^y(n_j(r_i(\hat x)))=\hat y \Big ]}$
* **User's strategy space $\Theta^u$.** We consider additive perturbations such that for an input x
$\displaystyle{r_i(x)=x+t(x), \ \ \ \ ||t(x)||_2\leq \epsilon}$
These perturbations are frequently referred to as adversarial image perturbations (AIPs).
* **Recognizer's strategy space $\Theta^r$.** We use translation (T), Gaussian additive noise (N), blurring (B), and cropping & re-sizing (C).

# Adversarial Image Perturbation Strategies
All Adversarial Image Perturbation Strategies can be seen as:
$\displaystyle{\mathop{\max}\limits_t \mathcal L(f(x+t),y) \ \ \ \ \ \ s.t ||t||_2\leq \epsilon}$

where x is the input image and y is the ground truth label; the loss function $\mathcal{L}$ is to be specified.
## Existing AIP methods
* **Fast Gradient Vector:**  $\mathcal{L} = -\log \hat f^y$, $t^\star = -\gamma\nabla\mathcal{L}$
* **Fast Gradient Sign:**  $\mathcal{L} = -\log \hat f^y$, $t^\star = -\gamma\  sign (\nabla\mathcal{L})$
* **Gradient Ascent:** starts from $t^{(0)}=0$ and perform $K$ times for $t^{(m+1)} = t^{(m)}- \gamma \nabla \mathcal{L} (x+t^{(m)})$ for $m = 0, ... ,K$
* **Basic Iterative**  BI is identical to GA, except that $\nabla\mathcal{L}$ is replaced with $sign (\nabla\mathcal{L})$.
* **DeepFool** DF algorithm solves the objective:
$\displaystyle{\min\limits_t ||t||_2\ \ \ \ \ s.t. \mathop{\arg \max}\limits_y f^y(x+t)\neq y}$

## AIP methods proposed by the author
### Gradient Ascent – Maximal Among Non-GT 
Approximate $c\thickapprox y^\star:=\mathop{\arg \min}\limits_{y'\neq y}f^{y'}$. The authors set the loss function as $\mathcal{L}= f^{y^\star}-f^y$, and perform gradient ascent with a fixed step size γ for K iterations. A $y^\star$ can be found.

### Vaccination against image processing
Since several image processing method are used as counter strategies in recognition, vaccination is needed.
The method is simple but useful. For countering an AIP-neutralising image processing technique $n_j$, the authors consider including the image processing step in the loss function: $\mathcal L(n_j(x + t))$.

# Image Processing Used in Recognizers
* **Proc:**  Even before R’s image processing strategies take place, the perturbed image needs to be (1) re-sized to the original image (from the network input sizes) and (2) quantised to integer values (e.g. 24-bit true color).
The authors denote the above two basic processing steps as Proc.
* **Translation:** translation by a random offset within 10% of the image side lengths.
* **Noise:** adds iid Gaussian noise with variance $σ^2$ = $10^2$. 
* **Blur:** B blurs with Gaussian kernel of width chosen from {1, 3, 5, 7, 9} uniformly at random.
* **Crop:** crops with a random offset within 10% of the image side lengths and re-sizes back to the original.

# Results of the Experiment
This chart shows the accuracy (%) of recognizers in different image processing methods Vs vaccinations. The accuracy varies in different combination of strategies, so analysis based on game theory is needed.
|User $\Theta^u$|Proc|T|N|B|C|TBNC|
|:-----:|:----:|:----:|:----:|:----:|:----:|:----:|
|GAMAN|4.0|6.6|15.0|22.2|16.7|9.9|
|/T|2.5|2.3|11.6|18.5|7.2|4.9|
|/N|5.8|7.6|4.6|23.6|16.6|9.1|
|/B|0.4|0.8|8.6|5.8|3.1|1.4|
|/C|2.6|2.2|11.8|18.1|3.4|4.3|
|/TNBC|0.7|0.9|5.2|9.5|3.2|2.0|

Situations followed shoulde be considered:
* Optimal deterministic strategy.
* Optimal random strategy.
* Knowledge on R’s strategy. 
* Limited knowledge on $\Theta ^r$.

# Conclusion
> In this work, we have constructed a game theoretical framework to study a system with two players, user U and recogniser R, with antagonistic goals (dis-/enable recognition). We have examined existing and new adversarial image perturbation (AIP) techniques for U. As a case study of the framework, we have presented a game theoretical analysis of the privacy guarantees for a social media user, assuming strategy spaces that include the state of the art AIPs and person recognition techniques.

---
# Reference
1. [Adversarial Image Perturbation for Privacy Protection -- A Game Theory Perspective](https://arxiv.org/pdf/1703.09471)
