---
layout: post
title:  "Attention is All You Need (Transformer)"
date:   2021-03-08 09:00:00 +0800
tags:
  - Article_Review
permalink: /0005
---

# 原标题：Attention is All You Need

### 1. 作者

Ashish Vaswani, Noam Shazeer, Niki Parmar等一众谷歌大佬

### 2. 引用

 Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N Gomez, Łukasz Kaiser, and Illia Polosukhin. Attention is all you need. Advances in neural information processing systems, 30, 2017.

https://proceedings.neurips.cc/paper/2017/hash/3f5ee243547dee91fbd053c1c4a845aa-Abstract.html

### 3. 摘要

2017年那个时候序列转录模型（序列→序列）通常基于复杂的循环或卷积神经网络来搭建Encoder-Decoder架构。**最好的模型**通常在Encoder-Decoder之间加入attention机制。**本文提出全新的简单的网络架构——Transformer**，其仅仅依赖注意力机制，完全摒弃了之前的循环与卷积层。然后在两个机器翻译的任务上超越了SOTA。

### 4. Review

由于NIPS会议本身单栏和8页纸张的限制，文章写的言简意赅。

#### 4.1 Introduction

几句话描述了RNN的优点和缺点，引出了Attention机制通常是与RNN结合起来从Encoder中抽取有效信息传给Decoder。简单高效的新模型**Transformer**精准打击了RNN的计算效率。

#### 4.2 Background

那时候有人想用CNN来代替RNN，缺点在于CNN对**长序列**需要叠加多层来达到相应的感受野。Transformer将这一目标简化成固定数量的操作，然后用**Multi-Head Attention**来模拟CNN的多通道。

#### 4.3 Model Architecture

<center class="half">
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/09c2ac52-0092-4a89-966b-564876d37dc6/2.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220307%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220307T065601Z&X-Amz-Expires=86400&X-Amz-Signature=f9863102e4d6b7eba5d8f869d964161fa1d8080388a41a9546a908607f744f2d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%222.jpg%22&x-id=GetObject" style="zoom: 20%;" />
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/473f8e65-9734-4d5d-98a7-e07e47bab52c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220307%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220307T065222Z&X-Amz-Expires=86400&X-Amz-Signature=7d2982c06fb6cca7d914b26bb5a39e6868898fdecb2d0c55ee272af0a910faca&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" style="zoom:40%;" />
</center>

##### 4.3.1 核心的block分为以下5个

1. **Input Embedding / Output Embedding**:把词汇变成$d_{model}=512$的向量，其中Input Embedding是一整个句子，Output Embedding是一个个词汇（第一次喂BOS，之后每次喂上一次Decoder输出的词汇）
2. **Multi-Head Attention**:
   - Encoder：从左到右输入的V,K,Q本质是同一个东西，在每个位置对整个序列的信息进行提取(**self-attention**)
   - Decoder：V,K是来自Encoder的信息，Q是来自Decoder的信息(**cross attention**)，<u>可以把前者看作读取输入语音序列的整体信息，后者看作是到目前为止翻译出来的信息，然后两者结合继续往后翻译</u>
3. **Masked Multi-Head Attention**:保证在解$t$时刻向量时候，看不到$t+1$时刻以及之后的信息
4. **Feed Forward**:将提取后的信息映射到更想要的语义向量空间，本质是MLP+Relu+MLP
5. **Position Encoding**:

##### 4.3.2 Encoder和Decoder对比

- Encoder是**self-attention+并行计算**
- Decoder是结合Encoder信息+当前时刻的所有翻译出来的信息**cross-attention+顺序计算**

<center class="half">
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/39d676af-e9c5-4be2-aa29-62d06d1058b6/3.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220307%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220307T080324Z&X-Amz-Expires=86400&X-Amz-Signature=440b86d4afb0b9072bb524bb18a94c0601bb5dd63097173e67851e4563a11d50&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%223.jpg%22&x-id=GetObject" style="zoom: 25%;" />
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e931a413-4255-4ea4-bc68-edfa0d3dee7f/4.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220307%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220307T080407Z&X-Amz-Expires=86400&X-Amz-Signature=855ecec49b1948cb9bafb867ac41da543c5fb523fcd53c74e38390bea1f1bb20&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%224.jpg%22&x-id=GetObject" style="zoom: 25%;" />
</center>

### 5. Rethink

1. Transformer的精髓在于并行计算带来的高效率，QKV的设计一定有更加深层次的原理。
2. 原论文中，Decoder在叠加多个block的时候，吃的都是Encoder最终输出的信息，其实你可以五花八门想一些奇怪的吃Encoder多层信息的方法。
3. Transformer是有监督的学习，Training的时候Decoder需要喂GT，但是Tesing的时候Decoder有可能会在某一步预测错误，导致后面连续出错，所以在Training中加入一些噪声（Scheduled Sampling）可以使得模型对错误更加鲁棒。
4. Transformer是一个Autoregressive (AT)，即顺序解码输出结果，而NAT是另一个大坑，可并行计算直接输出所有结果。