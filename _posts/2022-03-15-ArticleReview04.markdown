---
layout: post
title:  "MVSNet"
date:   2022-03-15 14:00:00 +0800
tags:
  - Article_Review
permalink: /0008

---

# 原标题：MVSNet：Depth Inference for Unstructured Multi-view Stereo

### 1. 作者

香港科技大学教授，ICCV 2011主席，IEEE Fellow权龙团队（Yao Yao, Zixin Luo, Shiwei Li前三作学生都去了Apple公司），然后四作是自己创建了一家Altizure公司专门做三维重建

[<font color=#0099ff>一些拓展阅读，部分语句还是很有启发性</font>](https://zhuanlan.zhihu.com/p/39433640)

### 2. 引用

[<font color=#0099ff>Yao, Yao, et al. "Mvsnet: Depth inference for unstructured multi-view stereo." Proceedings of the European Conference on Computer Vision (ECCV). 2018.</font>](https://openaccess.thecvf.com/content_ECCV_2018/html/Yao_Yao_MVSNet_Depth_Inference_ECCV_2018_paper.html)

### 3. 摘要

- 文章提出了端到端多视图深度估计方法(<font color=#FF0000> 输入知道是任意视角下的多个图片，输出一个视角的深度图</font>)

- 方法总共分为四个步骤：
  1. CNN提取特征；
  2. 用<font color=#FF0000>可微齐次变换</font>构建3D代价体；
  3. 3D卷积出初始深度图；
  4. 细化出最终深度图
- 室内室外都验证了算法的高精度与速度快

### 4. Review

#### 4.1 Introduction

- 传统方法无法处理低纹理和反光，即便在兰伯特表面效果不错，但是整体重建完整度不行
- CNN-based方法理论上可以提取全局语义（包括了反光等先验知识），从而提升了匹配的鲁棒性

​	CNN用在双目匹配的效果不错，因为图片都是矫正过的，无需考虑相机参数，问题简化为水平像素级别的视差估计。但是多视图(MVS)不能简单看成多个双目的拼接，这样会无法充分利用信息。**现有工作处理速度又都比较慢而且吃内存**。所以关键问题：<font color=#FF0000>如何处理任意多视角图片？如何加速？如何减少内存消耗</font>。

​	有重点地介绍了proposed MVSNet <font color=#0099FF>(整体思路将多视图重建分解成每个视图的深度估计)</font>

- 输入一张reference image + 多张source image
- 利用**<font color=#FF0000>differentiable homography warping</font>**将<u>多视图的2D特征</u>与<u>相机几何</u>隐含的融入3D cost volume（在相机圆锥体中构建，而不是在普通的欧式空间）
- 利用**<font color=#FF0000>variance-based metric</font>**将多张图的特征能够融入一个cost volume
- 多尺度3D卷积出初始视差图
- 用reference image优化视差图

#### 4.2 Conclusion



#### 4.3 Related Works



#### 4.3 Model Architecture



### 5. Rethink
