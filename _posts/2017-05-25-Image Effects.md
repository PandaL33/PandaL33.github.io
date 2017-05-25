---
layout:     post
title:      "Image Effects"
subtitle:   " \"图像特效\""
date:       2017-05-25 10:04:13
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity3D
---
## 图像特效

Image Effects主要应用在Camera对象上，为画面添加视觉效果。Unity中所有的图像特效都编写在OnRenderImage函数中。 

#### Antialiasing(FullScreen)：抗锯齿（全屏）特效

该特效提供了平滑图像的功能，抗锯齿特效与算法的速度成反比，其属性为：

- Technique：抗锯齿技术，算法有7种可供选择
    1. FXAA2：快速近似抗锯齿算法
    2. FXAA3 Console：快速近似抗锯齿算法控制台，默认选项。
        * Edge Min Threshold：边缘最小阈值
        * Edge Threshold：边缘阈值
        * Edge Sharpness：边缘清晰度
    3. FXAA1 PresetA：预设快速近似抗锯齿算法A
    4. FXAA1 PresetB：预设快速近似抗锯齿算法B
    5. NFAA：仅模糊局部便捷的边缘模糊算法
        * Edge Detect Ofs：边缘检测
        * Blur Radius：模糊半径
        * Show Normals：选中此项则显示为法线效果
    6. SSAA：仅模糊局部边界的边缘模糊算法
    7. DLAA：自适应处理长边界抗锯齿算法
        * Sharp：清晰度

#### Bloom：泛光特效

泛光是一种增强版的辉光、眩光效果。泛光特效在增加光晕的同时自动添加高效能镜头眩光。其属性为：

- Quality：质量。
    1. Cheap：低等级质量，计算速度快
    2. High：高等级质量，默认选项
- Mode：模式
    1. Basic：基本模式
    2. Complex：复杂模式
- Blend：混合
    1. Screen：屏幕模式，该模式模拟两张图像同时投射到屏幕上。每个颜色通道被分开处理和渲染，能保留更多的细节
    2. Add：叠加模式，该模式下，R、G、B通道的各个颜色值累加，最大值为1，可以使亮度较低的像素变亮，当需要实现耀眼的白色光晕时使用该模式
- HDR：高动态光照渲染
    1. Auto：自动，默认选项。用于根据摄像头的HDR设置来控制高动态光照渲染效果的开光
    2. On：强制打开高动态光照渲染效果
    3. Off：强制关闭高动态光照渲染效果
- Intensity：强度，用于控制全局光照的强度，主要影响泛光和光晕
- Threshhold：阈值，用于控制泛光和光晕计算的阈值
- RGB Threshhold：RGB阈值，只在Mode设置为Complex模式下生效，可以为每个颜色通道分别设置阈值
- Blur Iterations：模糊迭代，重复应用多少次高斯模糊到图像上。跌打次数越多越平滑
- Sample Diatance：采样间距，用于控制最大模糊半径，对性能影响较小
- Lens Flares：镜头眩光，只在Mode设置为Complex模式下生效
    1. Ghosting：重影镜头眩光类型
    2. Anamorphic：变形镜头眩光类型
    3. Combined：组合类型
- Local Intensity：局部强度，只在模式设置为Complex下有效，仅用于镜头眩光，值为0表示镜头眩光无效果，非0值则有
    1. 1st-4th Color：颜色调整，仅当镜头眩光模式为重影或混合时有效
    2. Stretch width：拉伸宽度，用于控制镜头眩光的变形宽度
    3. Rotation：旋转，用于控制镜头眩光的变形方式
    4. Blur Iterations：模糊迭代，用于控制变形镜头眩光的模糊迭代次数，次数越高光晕越圆滑
    5. Saturation：饱和度，用于控制镜头眩光饱和度，值为0时，光晕的颜色将近似于Tint Color
    6. Tint Color：着色，用于调整变形镜头眩光的颜色
    7. Mask：光晕遮蔽图，指定一张作为遮蔽图，用于实现屏幕边缘的镜头眩光效果
    8. Threshold：局部阈值，定义哪部分图像将被用于产生镜头眩光


#### Bloom And Flares：泛光和镜头眩光特效

属性大部分与Bloom相似，特别的有：

- Cast lens flares：激活镜头眩光，该项用于开启或关闭镜头眩光效果
- Use alpha mask：使用Alpha通道作为遮蔽图，进而控制泛光效果

#### Blur：模糊特效

图像模糊特效可以实时将渲染出的画面进行模糊处理。其属性如下：

- Iterations：迭代次数，用于控制迭代次数，次数越多模糊的效果越好
- Blur Spread：模糊半径，更大的数值可以让模糊在同样的迭代次数中蔓延得更广。默认设置0.6
- Blur Shader：模糊Shader

#### Color Correction Curves：色彩校正（曲线）特效

该特效使用曲线调整每一个颜色通道，也可以根据每个像素的深度进行调整，其属性为：

- Saturation：饱和度，用于设置色彩饱和度级别，值越大色彩饱和度越高，0时图像为黑白
- Mode：模式，用于指定参数配置模式
    1. Simple：简单配置模式
    2. Advanced：高级配置模式
- Red：红色，用于调节红色通道曲线
- Green：绿色，用于调节绿色通道曲线
- Blue：蓝色。用于调节蓝色通道曲线
- Red(depth)：红(深度),用于基于深度调整红色通道曲线（只在Advanced模式下可用）
- Green(depth)：绿(深度),用于基于深度调整绿色通道曲线（只在Advanced模式下可用）
- Blue(depth)：蓝(深度),用于基于深度调整蓝色通道曲线（只在Advanced模式下可用）
- Blend Curve：混合曲线，用于控制前景和背景颜色校正之间的混合方式曲线（只在Advanced模式下可用）
- Selective：选择性颜色校正，用于将源图像中的某种颜色替换为另一种颜色
    1. Key：关键色，用于指定被替换的颜色
    2. Targe：目标色，用于指定替换后的颜色

#### Constrast Enhance：对比度增强特效

对比度增强特效可以增强游戏画面的对比度，原理是使用了图像处理领域中非锐度遮蔽图方式来达到增强对比度的效果，其属性为：

- Intensity：强度，用于设置对比度增强的强度，值越大图像对比度越高
- Threshhold：阈值，用于控制处理像素的阈值，对于阈值以下的像素不进行对比度增强处理
- Blur Spread：模糊半径，用于设置模糊计算的半径，在指定的半径值之内将进行对比度处理

#### Edge Detecion：几何边缘检测特效

边缘检测的图像特效根据场景中游戏对象的几何形状来绘制其轮廓线，边缘由颜色的差异、相邻像素所对应的法线朝向以及深度等因素来共同决定。其属性为：

- Mode：模式用于指定轮廓线绘制的模式，有5种选项
    1. Triangle Depth Normal：依据几何体的深度与法线检测边缘
    2. Roberts CrossDepth Normals：使用Roberts Cross滤镜检测边缘
    3. Sobel Depth：使用Sobel滤镜金策边缘
    4. Sobel DepthThin：使用薄Sobel滤镜检测边缘
    5. Triangle Luminance：使用三角亮度检测边缘

