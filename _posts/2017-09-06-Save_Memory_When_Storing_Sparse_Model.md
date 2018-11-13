---
title: "Save Memory When Saving Sparse Model"
layout: post
category: 论文精粹
tags: [ML]
excerpt: "将稀疏参数的模型存储到内存中，google的论文介绍了他们如何在这个过程中节省内存空间。"
---

Google的这篇论文讲的是在做ctr预估的时候他们用的几个技巧，其中之一就是如何节省内存空间。

我们知道如果用比如LR model训练的时候，得到的参数很可能会很多，因为features就很多。这时，参数多数都不为零，(In fact, simply adding a subgradi- ent of the L 1 penalty to the gradient of the loss will essentially never produce coeﬃcients that are exactly zero.)但是当存储模型的时候，参数sparse的话才是能节省空间的，为此google用“Follow The (Prox- imally) Regularized Leader” algorithm, or FTRL-Proximal算法来实现参数的稀疏。这个算法实际上就是SGD＋regularization。
(感觉跟加L1/L2实现参数的稀疏好像是一个方法，有时间应该验证一下。)

论文: [Ad Click Prediction: a View from the Trenches]

[Ad Click Prediction: a View from the Trenches]:https://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/41159.pdf
