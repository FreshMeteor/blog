---
title: 'Face Recognition: Primates in the Wild'
date: 2018-06-27 11:40:20
tags: Computer Vision
mathjax: true
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

$$\LARGE{L_i=[\widetilde{x}_{i1},\widetilde{y}_{i1},\widetilde{x}_{i2},\widetilde{y}_{i2},\widetilde{x}_{i3},\widetilde{y}_{i3}]}$$  

并且用$\LARGE{\widetilde{x}_{ij}=(x_{ij}-\frac{1}{3}\sum_{k=1}^3x_{ik})}$来将坐标矫正，y同理。而对于N张图片的数据集，采用如下公式模板计算:  

$$\LARGE{t=\frac{1}{N}\sum_{i=1}^N\frac{L_i}{||L_i||_2^2}}$$  

然后利用如下近似转换，将图片角度调整到合适的位置：  
![image2](image2.png)  
其中s，θ，m分别代表比例系数，偏转角度，平移参数。为了得到参数，我们将以上等式重写成一个线性方程组  

$$\LARGE{Ax=b}$$  
然后我们通过 $\LARGE{x=(A^TA)^{-1}A^{T}b}$ 获得最小二乘估计，  
其中$\LARGE{x=[s*cos(\theta),s*sin(theta),m_x,m_y]^T}$  
如图1所示，用户需要手动标注三个点，才能给primnet进行身份识别。  

## PrimNet  
对于人脸，已经有足够大的数据集，但对于其他灵长类动物特别是濒危动物，脸部数据集的可用性是有限的。作者发现**SphereNet-4**，一个较小的人脸识别网络，在灵长类动物数据集上进行训练时会出现过度拟合，所以在设计PrimNet时对SphereNet-4架构进行了两项修改：
* 通过每一层group convolution策略使网络变得稀疏，减少了参数数量。  
* 通过增加通道数量来扩大网络，从而增强隐藏层的判别力。  

在传统CNN结构中，每个卷积滤波器作用于输入feature map的每一个通道，但是在group convolution中，每个卷积滤波器只作用于输入通道的一个子集，从而减少了参数数量。但是需要注意，如果所有layer都采用group convolution的话，每一个group之间的信息是隔离且无法交换的。  

通过4个卷积层的grouping和shuffling，PrimNet变成了一个比较稀疏的网络，总共只有 $\large9.92\times10^5$ 个参数，而与之相较，Sphere-4有 $\large1.26\times10^7$ 个参数，ShuffleNet 有大约 $\large1.4\times10^8$ 个参数。通过减少滤波器数量限制了中间layer的维度，并且这种增加稀疏度的办法没有妨碍它们的表达能力。  

PrimNet是通过*AM-Softmax function*训练的，这个函数已经被证明可以有效学习人脸特征。  

### PrimNet架构
![image3](image3.png)

## Experiments  
PrimNet主要完成三项任务：  
1. 验证
2. 封闭式识别
3. 开方式识别  
对每个实验，使用5-foldcross-validation来评估这个个体化模型的表现。