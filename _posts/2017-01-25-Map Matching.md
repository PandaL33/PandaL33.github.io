---
layout:     post
title:      "Map Matching"
subtitle:   " \"GIS\""
date:       2017-01-25 18:25:13
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - GIS
---
## 前言

&emsp;&emsp;在GPS定位时，常常会伴随着定位坐标的偏移。特别地，当汽车在道路上行驶时，需要将汽车GPS定位偏移的坐标点匹配到道路上。本文主要提供一种简便易行的匹配方案，用于将GPS偏移的坐标点匹配到道路上。

&emsp;&emsp;GPS定位坐标偏移现象：

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-1.png)


## 道路匹配

### 1、匹配算法
&emsp;&emsp;由于GPS的定位精度以及不同空间坐标系之间的差异，导致设备定位点出现坐标偏移（不在道路上）问题。系统采用最邻近算法解决设备定位点偏移问题，其算法流程如下：

1）获取设备定位点以及匹配道路数据（图中红色点为设备定位点，黄色线为匹配道路）；
 ![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-2.png)

2）设置匹配容差，以设备定位点为中心，匹配容差为半径绘制圆形缓冲区（图中黄色圆形区域）；
 ![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-3.png)

3）获取圆形缓冲区与匹配道路的交集（邻近道路集合），计算设备定位点到各个邻近道路集合的距离，得到距离设备定位点最近的最邻近道路，计算设备定位点到最邻近道路的投影点，该投影点为设备定位点的道路匹配点（图中匹配道路上的投影红点）。
 ![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-4.png)

##### 算法和流程逻辑 
 ![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-5.png)

### 2、算法实现
&emsp;&emsp;未完待续。。。

## 道路匹配校准数据获取
### 1、影像数据获取
&emsp;&emsp;前期准备：
- 安装google earth 4.3(一定要安装低版本。高版本可能会截取不了）
- 下载getScreen，设置GE4.3->工具－>选项－>3D视图－>图形模式->选择DirectX 和 使用安全模式 （重新启动）在GE中找到要截取影像数据的地图位置。

&emsp;&emsp;获取影像数据示例：

1）启动google earth并定位所需影像数据位置，运行GetScreen.exe工具。

  ![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-6.png)

2）点击“两点定位”按钮，设置影像数据范围。然后点击“图片计算”按钮，获取截屏数量。最后点击“开始截屏”按钮，并输入输出文件名（如UT.JPG）获取影像数据。

  ![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-7.png)

### 2、匹配道路绘制

1）影像数据配准
&emsp;&emsp;打开ArcMap并添加GetScreen截取的影像数据，打开影像数据配准工具Georeferencing（菜单Customize----->Toolbars------->Georeferencing）。

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-9.png)

&emsp;&emsp;点击Add Control Points工具，点击地图中步骤一设置的控制点，通过Input X and Y窗口，输入根据步骤一获取的控制点坐标（控制点一般取3~5个为宜）。
 
![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-10.png)

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-11.png)

&emsp;&emsp;添加完控制点坐标后，通过Georeferencing菜单的Rectify功能，输出匹配后的影像数据。
  
![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-12.png)

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-13.png)

2）道路匹配校准数据绘制
&emsp;&emsp;打开ArcCatlog，右键New----->Shapefile添加道路的shapefile文件，弹窗shapefile文件窗口。
 
![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-14.png)

&emsp;&emsp;输入道路图层名称，选择要素类型Feature Type为Polyline，点击“Edit”按钮设置图层的空间参考。选择Coordinate System------>World------>WGS1984 World Mercator坐标系。

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-15.png)

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-16.png)

&emsp;&emsp;使用ArcMap打开校准后的影像数据并添加新建的道路shapefile文件，右键道路图层---->Edit Features------>Start Editing启动图层编辑功能。

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-17.png)

&emsp;&emsp;在Create Features窗口的Construction Tools一栏中点击“Line”，根据影像数据中道路的轮廓绘制匹配的道路数据。编辑完成后保存即可完成匹配道路数据的绘制。
  
![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/map-matching/map-matching-19.png)

