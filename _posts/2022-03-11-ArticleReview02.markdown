---
layout: post
title:  "Vision Transformer (ViT)"
date:   2022-03-11 09:00:00 +0800
tags:
  - Article_Review
permalink: /0006

---

# 原标题：An Image is Worth 16×16 Words：Transformers for Image Recognition at Scale

### 1. 作者

Alexey Dosovitskiy, Lucas Beyer等谷歌大脑团队

### 2. 引用

[Dosovitskiy, Alexey, et al. "An image is worth 16x16 words: Transformers for image recognition at scale." *arXiv preprint arXiv:2010.11929* (2020).](https://arxiv.org/abs/2010.11929)

### 3. 摘要

2020年10月挂在arXiv上面的，当时attention要么是结合CNN一起用，要么去替代CNN架构中的部分操作。**本文**验证了**纯transformer架构也可以在图片分类上做的很好**。在超大型数据集上进行pre-train然后在中小型数据集上进行fine-tune可以得到与SOTA CNN方法一样的效果，同时需要更少的计算资源（😓属实有钱人炫富了，Google的计算资源超乎你想象）

### 4. Review

由于ICLR会议本身单栏和8页纸张的限制，文章写的言简意赅。

#### 4.1 Introduction

- self-attention在NLP里广泛应用，由于Transformer的计算高效性与可扩展性，现在能够train一个100Billion参数的大模型了。**虽然dataset与model都在变得超大，但是Transformer性能一直在往上涨，也就意味着它不容易过拟合。**
- 目前CV领域还是CNN主导，然后添加部分的self-attention或者直接替代卷积，虽然效果不错但是由于各自特定的注意力机制，没办法在现有的硬件加速器上进行高效拓展。
- 虽然NLP都是通过自监督训练，但是cv的baseline都是有监督的模型，为了公平对比**本文选择了有监督**。

#### 4.2 Conclusion

- Transformer在中小型数据集上训练，如果不添加强正则化约束，效果不如同等大小CNN架构的。**原因：**
  1. CNN具有图片性质的归纳偏置：**locality**局部性，**Translation Equivariance**平移等价性，这两者都是最底层卷积核所带来的性质。
  2. Transformer没有任何先验假设，所有性质都需要从数据中学习，所以需要大量数据
- 为了保证NLP里面原始Transformer的架构不加修改的迁移到CV领域，**本文**只在图片预处理的**patch extraction**步骤中加入了一些图片的归纳偏置，然后在大数据集上预训练，使用时微调可以得到非常好的效果。
- 未来的应用方向有[detection](Carion, Nicolas, et al. "End-to-end object detection with transformers." *European conference on computer vision*. Springer, Cham, 2020.)，[segmentation](https://openaccess.thecvf.com/content/CVPR2021/html/Zheng_Rethinking_Semantic_Segmentation_From_a_Sequence-to-Sequence_Perspective_With_Transformers_CVPR_2021_paper.html)等等，在本文之前已经有[DETR](https://arxiv.org/abs/2005.12872)了
- **ViT**自监督训练效果还没有达到有监督的水平，需要进一步探讨深挖

#### 4.3 Related Works

简要说了Transformer已经是NLP领域的SOTA方法了，比如[BERT](https://arxiv.org/abs/1810.04805)和[GPT](https://scholar.google.com.hk/scholar?q=Improving+Language+Understanding+By+Generative+Pre-Training&hl=zh-TW&as_sdt=0&as_vis=1&oi=scholart)。

- **Transformer用在CV领域的最大困难是复杂度过高**，e.g. 224×224的图片展平就是5w+的序列长度，而现在NLP顶多也就能做到1000+，所以根本现有硬件根本放不下。
- 有一篇文章跟本文做的很像，但是**本文验证了大数据上对原始Transformer的预训练可以得到与SOTA CNN相似的效果**（更多实验，更少对Transformer的魔改），本文的方法也适用于更大的图像（从2×2裁切到16×16裁切）

#### 4.3 Model Architecture

跟BERT一模一样，就是把图片裁切成多个16×16 patch作为序列输入，然后加上position encoding

<img src="https://raw.githubusercontent.com/Rashfu/Rashfu.github.io/master/assets/images/article/5.jpg" style="zoom: 70%;" />

### 5. Rethink

- **训练监督方式**：因为transformer在NLP是通过大规模的自监督训练起来的，所以CV目前也是类比NLP通过masked patch prediction来做，但是效果不如有监督。所以未来的发展方向之一是探讨transformer在CV领域的自监督目标函数。
- **模型架构改进**：你可以改tokenlization，可以改transformer的block。有的论文把self-attention换成mlp也可以做的很好，metaformer认为transformer有效是因为架构（换成不能学习的池化也可以做的很好）
- **对比学习**：2021年自监督领域最火的方法