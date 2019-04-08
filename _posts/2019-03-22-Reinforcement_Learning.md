---
title: "Reinforcement Learning"
layout: post
category: [机器学习]
tags: [rl]
excerpt: "台湾李宏毅教授的Reinforcement Learning课程笔记"
---

# 李宏毅教授课程笔记

### 入门

- 所有的state和action都是连续的。比如当前state是有一杯水，action是打翻了水，那么下一个state就是打翻了水的状态。注意，有时候environment的变化跟action没关系(比如游戏环境的变化)。
- 从开始到结束的过程，叫episode。
- AlphaGo的情况，state就是棋盘，action就是自己落子的位置，environment就是对手。采取了action之后，对手也会下一步棋，改变state。在围棋里面，reward一直是0，因为什么都没有发生，知道最后赢了(reward=1)输了(reward=-1)。
- 模型最后才得到reward会产生这样一个问题：模型可能走了几百步然后赢了，但是它不知道这几百步里哪些走法是好的，哪些走法是差的，它需要自己去学。AlphaGo一般要学习3000W盘之后才能学到，方法是学两个machine让它们互相下。实际上AlphaGo是先用supervise learning学，让它下的不错了之后，再用reinforcement learning。
- 如果下围棋是用supervise learning的方法，那么可能学出的model不是最优的。原因在于，supervise learning在训练的时候，需要观察当前状态，然后人告诉模型在当前状态下走哪一步是最好的，但是通常情况下人也不知道哪一步是最好的，笔者认为这也是AlphaGo比人类强的原因，它能找到更好的解。

### Policy-based

- $Actor/Policy = \pi(Oberservision)$

- 如果actor采用NN，那么就是deep reinforcement learning。NN的输出就是可能采取的action，有几个action就有几个维度的输出。NN的好处是比较generalize，即使没有遇到的情况，也会输出一个结果，有可能会得到一个合理的结果。

- 衡量goodness of actor：在一个episode里面，最大化获得的总的reward的期望。

  - 这里有两个地方要注意：
    1. reward是一个总和，不是每一步的reward。
    2. 最大化的是期望。因为采取完全相同的actions，每一次获得的reward也有可能不一样，原因有两个，一个是actor是stochastic的，采取行动有可能不是最大概率的那个，二个是环境也是随机的，采取相同的行动，环境给的reward可能也不一样。

  - 一个episode可以用一个trajectory(轨道)$\tau$表示：

    $$\tau=\{s_1, a_1, r_1, s_2, a_2, r_2, … , s_T, a_T, r_T\}$$

    总reward:

    $$R(\tau) = \sum^T_{n=1}r_n$$

    一个$\tau$代表了一个过程，过程有千万种可能，但是当选择了某一个actor去做action的时候，有可能只能出现某一些过程，另外一些过程比较难出现。用$P(\tau|\theta)$表示当actor的参数是$\theta$的时候$\tau$这个过程出现的几率。

    用$\bar R_\theta$表示一个过程获得总reward的期望：

    $\bar R_\theta = \sum_\tau R(\tau) P(\tau|\theta)​$

    但是所有的$\tau​$不太可能全部列出，只能用actor($\pi_\theta​$)去玩N次游戏，获得$\{\tau^1, \tau^2, … , \tau^N\}​$。这样做，就好像是从$P(\tau|\theta)​$里面做N次sample一样。

    所以：

    $\bar R_\theta = \sum_\tau R(\tau) P(\tau|\theta) \approx \frac{1}{N} \sum_{n=1}^N R(\tau^n)$

  - 用Gradient ascent方法最大化${\theta}$：

    ${\theta^*} = argmax \bar R_\theta$

    Start with $\theta^0$

    $\theta^1 \leftarrow \theta^0 + \eta \nabla \bar R_{\theta^0}$

    ...

    $\theta = \{w_1, w_2, … , b_1, ...\}$

    $\nabla \bar R_\theta = \begin{bmatrix} \partial \bar R_\theta/\partial w_1 \\ \partial \bar R_\theta/\partial w_2 \\ .\\.\\.\\ \partial \bar R_\theta/\partial b_1 \\.\\.\\. \end{bmatrix}$

    具体计算：

    $\bar R_\theta = \sum_\tau R(\tau) P(\tau|\theta)$

    $\nabla \bar R_\theta = \sum_\tau R(\tau) \nabla P(\tau | \theta) = \sum_\tau R(\tau) P(\tau | \theta) \frac{\nabla P(\tau | \theta)}{P(\tau | \theta)} = \sum_\tau R(\tau) P(\tau | \theta) \nabla log P(\tau|\theta) \approx \frac{1}{N} \sum_{n=1}^{N} R(\tau^n)\nabla log P(\tau^n|\theta)​$

    $P(\tau|\theta) = p(s_1)p(a_1|s_1,\theta)p(r_1, s_2|s_1, a_1)p(a_2|s_2, \theta)p(r_2, s_3|s_2, a_2)… = p(s_1)\prod_{t=1}^{T}p(a_t|s_t, \theta)p(r_t, s_{t+1}|s_t, a_t)$

    $log P(\tau|\theta) = log p(s_1) + \sum_{t=1}^{T}log p(a_t|s_t, \theta) + log p(r_t, s_{t+1}|s_t, a_t)$

    $\nabla log P(\tau|\theta) = \sum_{t=1}^{T} \nabla log p(a_t|s_t, \theta)$

    $\nabla \bar R_\theta \approx \frac{1}{N} \sum_{n=1}^{N} R(\tau^n) \nabla log P(\tau^n|\theta) = \frac{1}{N} \sum_{n=1}^{N} R(\tau^n) \sum_{t=1}^{T_n} \nabla log p(a_t^n|s_t^n, \theta) = \frac{1}{N} \sum_{n=1}^{N} \sum_{t=1}^{T_n} R(\tau^n) \nabla log p(a_t^n|s_t^n, \theta)$

    注意：这里$R(\tau^n)$是第n个trajectory的*总的*reward，而不是某一步的reward，这也表示我们要优化的是总的收益，而不局限于单步。

### A3C

- Policy-based: Learning an actor

  Value-based: Learning a critic

  AlphaGo是融合了多种方法：policy-based + value-based + model-based





Reference:

1. [李宏毅youtube](https://www.youtube.com/watch?v=W8XF3ME8G2I)

