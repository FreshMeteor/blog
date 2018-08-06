---
title: facenet论文学习
date: 2018-07-09 19:33:39
tags: computer vision
mathjax: true
---

# FaceNet: A Unified Embedding for Face Recognition and Clustering
## Abstract
FaceNet通过欧式空间的距离来直接映射图像相似度。两张图片通过FaceNet提取特征点之间的欧式距离，得出图片之间的差异。
欧式距离：欧几里得n维空间中两点的距离

$$\large{d(x,y)=\sqrt{\sum_{i=1}^{n}(x_i-y_i)^2}}$$

## Introduction
作者提出了一个能完成人脸核实，分辨和聚类任务的统一系统:FaceNet。方法是基于对每一个图像采用**深度卷积网络**学习它的embedding。