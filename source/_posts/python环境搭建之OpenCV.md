---
title: python环境搭建之OpenCV
abbrlink: 61656
date: 2018-11-08 10:36:53
categories: 后端
tags:
  - python
  - CV
---

### openCV介绍

Open Source Computer Vision Library.OpenCV于1999年由Intel建立，如今由Willow Garage提供支持。OpenCV是一个基于BSD许可（开源）发行的跨平台计算机视觉库，可以运行在Linux、Windows、MacOS操作系统上。它轻量级而且高效——由一系列 C 函数和少量C++类构成，同时提供了Python、Ruby、MATLAB等语言的接口，实现了图像处理和计算机视觉方面的很多通用算法。最新版本是3.3 ，2017年8月3日发布。

**优势--为什么有OpenCV**

计算机视觉市场巨大而且持续增长，且这方面没有标准API，如今的计算机视觉软件大概有以下三种：
1. 研究代码（慢，不稳定，独立并与其他库不兼容）
2. 耗费很高的商业化工具（比如Halcon, MATLAB+Simulink）
3. 依赖硬件的一些特别的解决方案（比如视频监控，制造控制系统，医疗设备）这是如今的现状，而标准的API将简化计算机视觉程序和解决方案的开发，OpenCV致力于成为这样的标准API。

OpenCV致力于真实世界的实时应用，通过优化的C代码的编写对其执行速度带来了可观的提升，并且可以通过购买Intel的IPP高性能多媒体函数库（Integrated Performance Primitives）得到更快的处理速度。右图为OpenCV与当前其他主流视觉函数库的性能比较。

__应用领域编辑__
* 人机互动
* 物体识别
* 图像分割
* 人脸识别
* 动作识别
* 运动跟踪
* 机器人
* 运动分析
* 机器视觉
* 结构分析
* 汽车安全驾驶

简言之，通过openCV可实现计算机图像、视频的编辑。广泛应用于图像识别、运动跟踪、机器视觉等领域。

### 安装

一切就绪以后以管理员身份运行cmd或PowerShell。依次输入以下命令：

```bash
pip install --upgrade setuptools

pip install numpy Matplotlib

pip install opencv-python
```

opencv环境已经整好，就是这么简单。只需要numpy、Matplotlib、opencv-python三个包，都不大很快就可以下好，如果下载中间出现error或wrong，重新输入命令即可。

> 如果多次下载失败，可以从 [http://www.lfd.uci.edu/~gohlke/pythonlibs/](http://www.lfd.uci.edu/~gohlke/pythonlibs/) 直接下载whl包安装，安装whl包依然使用pip

```
pip install 包的位置(如：E:\download\xxx.whl)
```

### 测试

写.py脚本:

```python
#导入cv模块
import cv2 as cv
#读取图像，支持 bmp、jpg、png、tiff 等常用格式
img = cv.imread(r"E:\python\test.jpg")
#创建窗口并显示图像
cv.namedWindow("Image")
cv.imshow("Image",img)
cv.waitKey(0)
#释放窗口
cv2.destroyAllWindows() 
```

运行以上脚本，如果可以显示出测试的图像，则环境搭建成功

opencv的学习，推荐网站[www.opencv.org.cn](www.opencv.org.cn)，是中文的教程哦！