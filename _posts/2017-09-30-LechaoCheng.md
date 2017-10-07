---
layout:     post
title:      Deep Bilateral Learning for real-time image enhancement(Siggraph 2017)
date:       2017-09-30
author:     lechao cheng
catalog: true
tags:
    - Bilateral Grid
    - Local Affine Transformation
    - Deep Learning
    
---

# Outline

>- **Bilateral Grid**
- **Local Affine Transformation**
- **Network FrameWork**


 
---
# Bilateral Grid

![Definition](https://i.imgur.com/dSCsu7k.png)

---
# Bilateral Grid
![](https://i.imgur.com/Oax3nPu.png)

---
# Bilateral Grid
![](https://i.imgur.com/u1bXI6E.png)

---
# Bilateral Grid
![](https://i.imgur.com/8RiVg4d.png)

---
# Bilateral Grid
![](https://i.imgur.com/lIVRC6e.png)

---
---
# Bilateral Grid
![](https://i.imgur.com/JhZbMLJ.png)

---
# Bilateral Grid
![](https://i.imgur.com/KXzvShP.png)
---

# Bilateral Grid
![](https://i.imgur.com/EDKMcnM.png)

---
# Bilateral Grid
![](https://i.imgur.com/nHuwQlA.png)

---
# Bilateral Grid
![](https://i.imgur.com/RlQ57jo.png)

---
# Bilateral Grid
![](https://i.imgur.com/LHOAzDs.png)

---
# Bilateral Grid
![](https://i.imgur.com/mKZhUUY.png)
---
# Local Affine Transformation
![](https://i.imgur.com/glbKmXD.png)
![](https://i.imgur.com/hFNZZIP.png)

---
#Network FrameWork
![](https://i.imgur.com/pW0gdqv.png)
**Fusion and Linear Prediction**:
 ![](https://i.imgur.com/OAa4VwU.png)
 ![](https://i.imgur.com/qTThKeG.png)
---
#Network FrameWork
![](https://i.imgur.com/pW0gdqv.png)
**Upsampling with a trainable slicing layer**:
![](https://i.imgur.com/3z33UUf.png)
slicing(tri-linearly interpolating):
(16x16x8)x12 ----> 256x256x12(input size: 256x256x3)

---
#Network FrameWork
![](https://i.imgur.com/pW0gdqv.png)
**Local Affine Transformation**:
![](https://i.imgur.com/4hE5Yz4.png)

---
---
#Results
![](https://i.imgur.com/B2GyTzb.png)
---
