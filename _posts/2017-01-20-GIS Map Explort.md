---
layout:     post
title:      "Export GIS Map"
subtitle:   " \"GIS\""
date:       2017-01-20 20:00:00
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - GIS
---
## 前言

在GIS制图完成后，需要将地图输出成不同的格式，如.emf、.pdf、*.jpg等格式，输出的文件可以方便在计算机上进行浏览查看。
地图输出可以分为两大类：栅格数据和矢量数据，输出栅格数据的格式有BMP、JPG等，而输出矢量数据的格式有PDF、SVG等。
IExport接口作为地图输出的主要接口，其数据成员为：

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/export-map/export-map-1.jpeg)

IExport接口被10种输出格式的类实现，如下图：

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/export-map/export-map-2.jpeg)

这10个类对应了ArcGIS所支持的地图输出格式，同时这10个类也可以划分为两大类，即矢量格式和栅格格式。Window平台的分辨率一般为96dpi，而这个也是ArcGIS栅格数据输出的默认分辨率，而对于像PDF这样的分辨率，默认为300dpi。

## GIS地图

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/export-map/export-map-3.jpeg)

## 地图输出实现

### 1、EMF输出
```
/// <summary>
/// 以EMF格式输出地图
/// </summary>
/// <param name="pActiveView">输出视图</param>
/// <param name="pExportFileName">输出路径</param>
publicvoidExportEMF(IActiveView pActiveView,string pExportFileName)
{
    IExport pExport;
    pExport =newExportEMF()asIExport;
    pExport.ExportFileName= pExportFileName;
    pExport.Resolution=300;
    tagRECT exportRECT;
    exportRECT = pActiveView.ExportFrame;
    IEnvelope pPixelBoundsEnv =newEnvelopeClass();
    PixelBoundsEnv.PutCoords(exportRECT.left,exportRECT.top,exportRECT.right,exportRECT.bottom);
    pExport.PixelBounds= pPixelBoundsEnv;
    int hDC;
    hDC = pExport.StartExporting();
    pActiveView.Output(hDC,(int)pExport.Resolution,ref  exportRECT,null,null);
    pExport.FinishExporting();
    pExport.Cleanup();
}
```

输出结果：

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/export-map/export-map-4.jpeg)

###  2、PDF输出
```
/// <summary>
/// 以PDF格式输出地图
/// </summary>
/// <param name="pActiveView">输出视图</param>
/// <param name="pExportFileName">输出路径</param>
publicvoidExportPDF(IActiveView pActiveView,string pExportFileName)
{
IEnvelope pEnv = pActiveView.Extent;
IExport pExport=newExportPDFClass();
    pExport.ExportFileName= pExportFileName;
    pExport.Resolution=300;
    tagRECT exportRECT;
    exportRECT.top =0;
    exportRECT.left =0;
    exportRECT.right = pActiveView.ExportFrame.right;
    exportRECT.bottom = pActiveView.ExportFrame.bottom;
IEnvelope pPixelBoundsEnv =newEnvelopeClass();
    pPixelBoundsEnv.PutCoords(exportRECT.left, exportRECT.top,
    exportRECT.right, exportRECT.bottom);
    pExport.PixelBounds= pPixelBoundsEnv;
int hDC= pExport.StartExporting();
    pActiveView.Output(hDC,(int)pExport.Resolution,ref  exportRECT,
null,null);
    pExport.FinishExporting();
    pExport.Cleanup();
}
```
输出结果：

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/export-map/export-map-5.jpeg)

###  3、JPG输出
```
/// <summary>
/// 以JPG格式输出地图
/// </summary>
/// <param name="pActiveView">输出视图</param>
/// <param name="pFileName">输出路径</param>
/// <param name="pScreenResolution">屏幕分辨率</param>
/// <param name="pOutputResolution">输出分辨率</param>
publicvoidExportJPG(IActiveView pActiveView,String pExportFileName,Int32 pScreenResolution,Int32 pOutputResolution)
{
IExport pExport =newExportJPEGClass();
    pExport.ExportFileName= pExportFileName;
    pExport.Resolution= pOutputResolution;
    tagRECT pExportRECT;
    pExportRECT.left =0;
    pExportRECT.top =0;
    pExportRECT.right = pActiveView.ExportFrame.right *
(pOutputResolution /pScreenResolution);
    pExportRECT.bottom = pActiveView.ExportFrame.bottom *
(pOutputResolution /pScreenResolution);
IEnvelope pEnvelope =newEnvelopeClass();
    pEnvelope.PutCoords(pExportRECT.left, pExportRECT.top,
    pExportRECT.right, pExportRECT.bottom);
    pExport.PixelBounds= pEnvelope;
System.Int32 hDC = pExport.StartExporting();
    pActiveView.Output(hDC,(System.Int16)pExport.Resolution,ref  pExportRECT,
null,null);
    pExport.FinishExporting();
    pExport.Cleanup();
}
```
输出结果：

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/export-map/export-map-6.jpeg)
