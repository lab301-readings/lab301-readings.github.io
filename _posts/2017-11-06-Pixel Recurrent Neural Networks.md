---
layout:     post
title:      Pixel Recurrent Neural Networks
subtitle:   RNN
date:       2017-11-06
author:     Yan Zheyu
catalog: true
tags: 
    - Generative Model
    - RNN
    - Neural Networks

---
#Outline

[TOC]

---
# Introduction
This is a work of Google DeepMind. It introduced a way to model the distribution of natural images for generative models. Two methods based on **LSTM**, **Row-RNN** and **Diagonal BiLSTM** is the main method contribution of this paper and a structure similar to **ResNet** is used to improve perfomance.

---
# Abstract
>Modeling  the  distribution  of  natural  images  is a  landmark  problem  in  unsupervised  learning. This  task  requires  an  image  model  that is at once expressive,   tractable  and  scalable. We present a deep neural network that sequentially predicts  the  pixels  in  an  image  along  the  two spatial dimensions.  Our method models the discrete probability of the raw pixel values and encodes  the  complete  set  of  dependencies  in  the image. Architectural novelties include fast two-dimensional recurrent layers and an effective use of  residual connections  in  deep  recurrent  networks. We achieve log-likelihood scores on natural images that are considerably better than the previous state of the art.  Our main results also provide  benchmarks  on  the  diverse  ImageNet dataset.  Samples generated from the model appear crisp, varied and globally coherent.

---
# Related Work
>- **VAE:** arXiv preprint, 2013
- **Works on RNN:** Generating sequences with recurrent neural networks, 2013
- **LSTM:** Neural computation, 1997
- **Pixel RNN:** Generative image modeling using spatial LSTMs, In Advances in Neural Information Processing Systems, 2015


---
# Model
## Generating an Image Pixel by Pixel
The distribution of an input image **x** can be depicted by a multiplication of conditional distributions:
<center>$\displaystyle{\prod_{i=1}^{n^2}}p(x_i | x_1, ...,x_i-1)$</center>

## Pixels as Discrete Variables
We often takes values (intensity) of each pixel as continuous variables, thus requires computation of integrations, which is inaccurate and time-consuming when numerical methods are involved. The authors, howerver, model p(**x**) as a discrete distribution, with every conditional distribution being a multinomial that is modeled with a softmax layer.

# Pixel Recurrent Neural Networks
## Row LSTM
The Row LSTM is a unidirectional layer that processes the image row by row from top to bottom computing features for a whole row at once; the computation is performed with a one-dimensional convolution. The kernel of the one-dimensional convolution has size k × 1 where k ≥ 3; the larger the value of k the broader the context that is captured.
The perception domain of one pixel when k = 3 is depicted below.

|i-2|i-2|i-2|i-2|i-2|
|:---:|:---:|:---:|:---:|:---:|
|/|i-1|i-1|i-1|/|
|/|/|i|/|/|
|/|/|/|/|/|

It is of great importance that, **the weight sharing** in the convolution ensures translation invariance of the computed features along each row.
[LSTM][1] (if you are not familiar with LSTM, please click [this link][1]) has three gates and one latent value which can be depicted as $\boldsymbol o_i, \boldsymbol f_i, \boldsymbol i_i \boldsymbol g_i$, where $\boldsymbol i_i$ is the latent value and others are gates.
With two learnable Weights $\boldsymbol K^{ss}$ and $\boldsymbol K^{is}$, the four numbers mentioned above and the output of a **LSTM** cell, **hidden state** (which is also the output of the whole model) and **cell state**, can be depicted as:
<center>$[\boldsymbol o_i, \boldsymbol f_i, \boldsymbol i_i \boldsymbol g_i] = \sigma(\boldsymbol K^{ss}\circledast \boldsymbol h_i + \boldsymbol K^{is} \circledast \boldsymbol x_i)$</center>
<center>$\boldsymbol c_i = \boldsymbol f_i \odot \boldsymbol c_{i-1} + \boldsymbol i_i \odot \boldsymbol g_i$</center>
<center>$\boldsymbol h_i = \boldsymbol o_i \odot \tanh(\boldsymbol c_i)$</center>

where $\circledast $ is convolution and $\odot$ is elementwise multipilication.

## Diagonal BiLSTM
The calculation method of Diagonal BiLSTM is the same as Row LSTM, but the perception domain is different.
The perception domain of Diagonal LSTM is depicted as below and a BiLSTM takes the other side.

|i-4|i-3|i-2|/|/|
|:---:|:---:|:---:|:---:|:---:|
|i-3|i-2|i-1|/|/|
|i-2|i-1|i|/|/|
|/|/|/|/|/|

The author also provided ad trick for this method. By move each line one pixel step by step we can get:

|i-4|i-3|i-2|/|/|
|:---:|:---:|:---:|:---:|:---:|
|(i-4)|i-3|i-2|i-1|/|
|(i-4)|(i-3)|i-2|i-1|i|
|/|/|/|/|/|

then a convolution of kernel 2x1 can be applied to it.

## Other Methods
The authors also applied **ResNet** and **Masked Convoltion**. ResNet is popular and doesn't require introduction. The authors defined two masks for convolution, which are depicted below (where x is the perception domain):
<center>Mask A</center>

|x|x|x|x|x|
|:---:|:---:|:---:|:---:|:---:|
|x|x|x|x|x|x|
|x|x|pixel|/|/|
|/|/|/|/|/|/|
|/|/|/|/|/|

<center>Mask B</center>

|x|/|x|/|x|
|:---:|:---:|:---:|:---:|:---:|
|/|x|/|x|/|x|
|x|/|pixel|/|x|
|/|x|/|x|/|
|x|/|x|/|x|

# Specifications of Models
The first layer is a 7 × 7 convolution that uses the mask of type A. The two types of LSTM networks then use a variable number of recurrent layers. The input-to-state convolution in this layer uses a mask of type B, whereas the state-to-state convolution is not masked. The PixelCNN uses convolutions of size 3 × 3 with a mask of type B. The top feature map is then passed through a couple of layers consisting of a Rectified Linear Unit (ReLU) and a 1×1 convolution.

|Row LSTM| Diagonal BiSTM|
|:---------:|:----------:|

|7 x 7 conv mask A|
|:--:|
|Multiple  residual blocks|

|Row LSTM| Diagonal LSTM|
|:---------:|:---------:|
|i-s 1 x 3 mask B|i-s 2 x 1 mask B|
|s-s 1 x 3 no mask |s-s 2 x 1 mask B|

|ReLU followed by 1 × 1 conv, mask B (2 layers)|
|:-:|
|256-way Softmax for each RGB color (Natural images) or Sigmoid (MNIST)|

# Conclusion
>In this paper we significantly improve and build upon deep recurrent neural networks as generative models for natural images. We have described novel two-dimensional LSTM layers: the Row LSTM and the Diagonal BiLSTM, that scale more easily to larger datasets. The models were trained to model the raw RGB pixel values. We treated the pixel values as discrete random variables by using a soft-max layer in the conditional distributions. We employed masked convolutions to allow PixelRNNs to model full dependencies between the color channels. We proposed and evaluated architectural improvements in these models resulting in PixelRNNs with up to 12 LSTM layers.
We have shown that the PixelRNNs significantly improve the state of the art on the MNIST and CIFAR-10 datasets. We also provide new benchmarks for generative image modeling on the ImageNet dataset. Based on the samples and completions drawn from the models we can conclude that the PixelRNNs are able to model both spatially local and long-range correlations and are able to produce images that are sharp and coherent. Given that these models improve as we make them larger and that there is practically unlimited data available to train on, more computation and larger models are likely to further improve the results.

# Comments
This comment is done by our counterparts in [this link][2].
He thinks that the contribution of this paper is:
>1. They propose Diagonal BiLSTM units for the PixelRNN, which are efficient (thanks to the use of convolutions) while making it possible to, in effect, condition a pixel's distribution on all the pixels above it (see Figure 2 for an illustration).
2. They demonstrate that the use of residual connections (a form of skip connections, from hidden layer i-1 to layer i+1) are very effective at learning very deep distribution estimators (they go as deep as 12 layers).
3. They show that it is possible to successfully model the distribution over the pixel intensities (effectively an integer between 0 and 255) using a softmax of 256 units.
4. They propose a multi-scale extension of their model, that they apply to larger 64x64 images.

# Code
This project has its source on github, the code link is as follows:
https://github.com/igul222/pixel_rnn
and some major codes are here:
```python
# inputs.shape: (batch size, height, width, channels)
inputs = T.tensor4('inputs')

output = Conv2D('InputConv', N_CHANNELS, DIM, 7, inputs, mask_type='a')

if MODEL=='pixel_rnn':

    output = DiagonalBiLSTM('LSTM1', DIM, output)
    output = DiagonalBiLSTM('LSTM2', DIM, output)

elif MODEL=='pixel_cnn':
    # The paper doesn't specify how many convs to use, so I picked 4 pretty
    # arbitrarily.
    for i in xrange(4):
        output = Conv2D('PixelCNNConv'+str(i), DIM, DIM, 3, output, mask_type='b', he_init=True)
        output = relu(output)

output = Conv2D('OutputConv1', DIM, DIM, 1, output, mask_type='b', he_init=True)
output = relu(output)

output = Conv2D('OutputConv2', DIM, DIM, 1, output, mask_type='b', he_init=True)
output = relu(output)

# TODO: for color images, implement a 256-way softmax for each RGB channel here
output = Conv2D('OutputConv3', DIM, 1, 1, output, mask_type='b')
output = T.nnet.sigmoid(output)
```


---
# Reference
1. [Pixel Recurrent Neural Networks](https://arxiv.org/abs/1601.06759)
2. [[译] 理解 LSTM 网络](http://www.jianshu.com/p/9dc9f41f0b29)
3. [Notes on Pixel Recurrent Neural Networks](https://www.evernote.com/shard/s189/sh/fdf61a28-f4b6-491b-bef1-f3e148185b18/aba21367d1b3730d9334ed91d3250848)


  [1]: http://www.jianshu.com/p/9dc9f41f0b29
  [2]: https://www.evernote.com/shard/s189/sh/fdf61a28-f4b6-491b-bef1-f3e148185b18/aba21367d1b3730d9334ed91d3250848
