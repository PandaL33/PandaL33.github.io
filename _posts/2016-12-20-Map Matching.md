---
layout:     post
title:      "Map Matching"
subtitle:   " \"GIS\""
date:       2016-12-20 18:25:13
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - GIS
---
## 前言

在GPS定位时，常常会伴随着定位坐标的偏移。特别地，当汽车在道路上行驶时，需要将汽车GPS定位偏移的坐标点匹配到道路上。本文主要提供一种简便易行的匹配方案，用于将GPS偏移的坐标点匹配到道路上。
GPS定位坐标偏移现象：

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-1.png)


## 道路匹配

### 1、匹配算法
由于GPS的定位精度以及不同空间坐标系之间的差异，导致设备定位点出现坐标偏移（不在道路上）问题。系统采用最邻近算法解决设备定位点偏移问题，其算法流程如下：
1）获取设备定位点以及匹配道路数据（图中红色点为设备定位点，黄色线为匹配道路）；
 ![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-2.png)

2）设置匹配容差，以设备定位点为中心，匹配容差为半径绘制圆形缓冲区（图中黄色圆形区域）；
 ![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-3.png)

3）获取圆形缓冲区与匹配道路的交集（邻近道路集合），计算设备定位点到各个邻近道路集合的距离，得到距离设备定位点最近的最邻近道路，计算设备定位点到最邻近道路的投影点，该投影点为设备定位点的道路匹配点（图中匹配道路上的投影红点）。
 ![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-4.png)

	算法和流程逻辑 
 ![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-5.png)

### 2、算法实现
未完待续。。。