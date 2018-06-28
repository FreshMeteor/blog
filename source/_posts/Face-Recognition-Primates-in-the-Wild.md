---
title: 'Face Recognition: Primates in the Wild'
date: 2018-06-27 11:40:20
tags: Computer Vision
---

# 论文学习：Face Recognition: Primates int the Wild  
## 摘要  
为了保护濒危灵长类动物，捕捉动物再手动标签的造价高昂，于是本文作者提出了一个非侵入式的灵长类个体身份识别方法。  
* 一个狐猴身份识别系统，在129个狐猴，3000张照片的数据集中的rank-1中达到了92.45%的识别效果。 
* 提出一个新的CNN结构：PrimNet 应用于移动电话的小型数据集，在rank-1中达到了93.75%的狐猴个体识别准确性。
* PrimNet对其他灵长类动物有普遍性
* 开发了一个Android程序，用于研究人员跟踪和识别灵长类动物。  
* 计划将金丝猴面部和狐猴面部数据集开源。  

## 面部校准  
使用三个校准点：左眼，右眼，嘴中心。  
![image1](image1.png)  
用$\LARGE{[x_{ij},y_{ij}]^T}$表示第i张图片标记点的坐标，左眼、右眼、嘴中心分别表示为$\LARGE{(x_{i1},y_{i1}),(x_{i2},y_{i2}),(x_{i3},y_{i3})}$  
然后生成一个六元向量:  

$\LARGE{L_i=[\widetilde{x}_{i1},\widetilde{y}_{i1},\widetilde{x}_{i2},\widetilde{y}_{i2},\widetilde{x}_{i3},\widetilde{y}_{i3}]}$  

并且用$\LARGE{\widetilde{x}_{ij}=(x_{ij}-\frac{1}{3}\sum_{k=1}^3x_{ik})}$来将坐标矫正，y同理。而对于N张图片的数据集，采用如下公式计算:  

$\LARGE{t=\frac{1}{N}\sum_{i=1}^N\frac{L_i}{||L_i||_2^2}}$  
