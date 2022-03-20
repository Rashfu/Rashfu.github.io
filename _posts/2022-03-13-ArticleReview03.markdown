---
layout: post
title:  "STTR体内双目有监督"
date:   2022-03-13 14:00:00 +0800
tags:
  - Article_Review
permalink: /0007

---

# 原标题：Revisiting Stereo Depth Estimation From a Sequence-to-Sequence Perspective With Transformers

### 1. 作者

Zhaoshuo Li, Xingtong Liu, Mathias Unberath 约翰霍普金斯大学cs专业的博士，直觉公司机器学习工程师（卖达芬奇机器人的），导师

Nathan Drenkow, Andy Ding, Francis X. Creighton, Russell H. Taylor, 一众医生

### 2. 引用

[Li, Zhaoshuo, et al. "Revisiting stereo depth estimation from a sequence-to-sequence perspective with transformers." *Proceedings of the IEEE/CVF International Conference on Computer Vision*. 2021.](https://openaccess.thecvf.com/content/ICCV2021/html/Li_Revisiting_Stereo_Depth_Estimation_From_a_Sequence-to-Sequence_Perspective_With_Transformers_ICCV_2021_paper.html)

### 3. 摘要

双目深度估计原理：优化左右目图片对极线上像素匹配

利用Transformer做：

1. <font color=##FF0000>从直接输出视差→输出的是attention矩阵</font>，所以不再有fixed视差范围
1. 可以识别遮挡区域并给出置信度map。因为用了occlusion map作为GT监督
1. <font color=##FF0000>optimal Transport</font>保证了匹配的唯一性，更加符合常理

最后验证泛化能力非常优秀

### 4. Review

#### 4.1 Introduction

回顾了双目深度估计的挑战：

- 预先手动设定的视差范围，没有办法泛化
- 缺乏遮挡区域的识别
- 没有匹配唯一性约束

<font color=#FF0000>**阐述了自己的整个工作：**</font>

- 整体：Transformer架构+OT
- 细节：采用了Relative Position Encoding + 注意力机制
- 实验

#### 4.2 Conclusion

集合了CNN与Transformer架构的优点，在仿真数据集上训练完，不需要fine-tune在测试集上也能SOTA，效果很好

#### 4.3 Model Architecture

1. Backbone 采用CVPR2018PSMNet的SPPbackbone

<img src="https://raw.githubusercontent.com/Rashfu/Rashfu.github.io/master/assets/images/article/6.jpg" style="zoom: 70%;" />

### 5. Rethink
