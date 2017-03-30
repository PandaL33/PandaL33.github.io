---
layout:     post
title:      "Unity3D Light"
subtitle:   " \"GIS\""
date:       2017-03-29 21:00:00
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity3D
---
## 前言

在GIS制图完成后，需要将地图输出成不同的格式，如.emf、.pdf、*.jpg等格式，输出的文件可以方便在计算机上进行浏览查看。
地图输出可以分为两大类：栅格数据和矢量数据，输出栅格数据的格式有BMP、JPG等，而输出矢量数据的格式有PDF、SVG等。
IExport接口作为地图输出的主要接口，其数据成员为：

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/export-map/export-map-1.jpeg)

IExport接口被10种输出格式的类实现，如下图：

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/export-map/export-map-2.jpeg)

这10个类对应了ArcGIS所支持的地图输出格式，同时这10个类也可以划分为两大类，即矢量格式和栅格格式。Window平台的分辨率一般为96dpi，而这个也是ArcGIS栅格数据输出的默认分辨率，而对于像PDF这样的分辨率，默认为300dpi。

