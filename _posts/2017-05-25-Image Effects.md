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
- Edges Exponent：边缘指数，该项在Mode指定为SobelDepth、SobelDepthThin类型情况下有效
- Sampling Distance：采样距离，值越大创建的轮廓线会越宽，容易产生环状失真
- Edges Only：仅边缘，用于控制背景与Background中指定的原色进行混合的程度
- Color：背景色，配合Edges Only项使用，当Edges Only大于0时该项所设置的颜色与背景进行混合

#### Depth Of Field Deprecated：景深特效

景深特效用来模拟摄像机聚焦效果的图像特效，其属性为：

- Resolution：分辨率，用于指定分辨率模式，有3种模式
    1. High：高分辨率
    2. Medium：中等分辨率
    3. Low：低分辨率
- Quality：质量，用于指定质量级别，有2项可供选择
    1. Only Background：仅背景，运行效率较高
    2. Background And Foreground：背景与前景，质量较高
- Simple tweak：简单调整，选中该项调整到简单聚焦模式
- Visualize focus：可视化焦点，选中该项可在游戏运行模式下显示当前的聚焦平面，从而方便调试
- Enable bokeh：开启Bokeh（焦外成像），选中后将生成更真实的透镜模糊效果，对非常明亮的图像部分进行放大并与背景重叠，选中后会显示焦外成像设置
    1. Destination：目标，用于选择开启Background、Forground、Background and Foreground
    2. Intensity：强度，控制将Bokeh光斑形状混合至背景的程度
    3. Min luminance：最小亮度，用于设置产生Bokeh光斑的亮度阈值
    4. Min contrast：最小对比度，用于设置产生Bokeh光斑的对比度阈值，小于该值的像素将不进行背景虚化效果计算
    5. Downsample：向下采样，用于设置渲染Bokeh光斑形状的内部渲染目标的尺寸
    6. Size scale：大小缩放尺寸，调整Bokeh光斑形状的最大大小
    7. Texture mask：纹理遮蔽图，用于指定纹理遮蔽图
- Focal Distance：聚焦距离，用于设置世界空间中摄像机与聚焦平面的距离
- Transform：几何变换，单击该项右侧的圆圈按钮可以指定场景中的游戏对象来确定聚焦的距离
- Smoothness：平滑度，用于设置从散焦区域进入到聚焦区域中的平滑度
- Focal size：聚焦大小，用于设置聚焦区域的大小
- Blurriness：模糊次数，用于设置模糊处理的迭代次数
- Blur spread：模糊半径，数值与分辨率有关，需要针对不同分辨率进行调节

#### Fisheye：鱼眼镜头特效

鱼眼镜头图像特效用于制造图像扭曲效果，模拟鱼眼镜头的成像，其属性为：

- Strenth X:拉伸X轴，用于控制画面横向的扭曲程度
- Strenth Y:拉伸Y轴，用于控制画面纵向的扭曲程度

#### Global Fog：全局雾特效

全局雾图像特效用于创建基于摄像机对象的指数型雾效，该方式基于世界空间计算，其属性为：

- Distance Fog：距离雾开关
- Exclude Far Pixels：不选中的话，将提出天空盒图像
- Use Radial Distance：使用圆形雾开关
- Height Fog：高度雾开关
- Height：雾的高度值
- Height Density：高度雾强度
- Start Distance：雾的开始距离

#### Grayscale Effect：灰度特效
 
 灰度特效可将画面转化为灰度图像，它可以使用一个Texture Ramp纹理来讲亮度值重新映射到任意的颜色，其属性为：

 - Texture Ramp：渐变纹理，单击该项右侧的圆圈按钮可以指定图片纹理，功能与Color Correction色彩校正特效中的Texture Ramp功能相似
 - Ramp Offset：位移，用于控制渐变纹理采样的位移

#### Motion Blur：运动模糊特效

 运动模糊特效通过保留之前渲染帧的图像形成的运动轨迹效果，从而增强快速运动的感觉。其属性如下：

 - Blur Amount：模糊量，用于设置在输出图像中最多保留多少帧之前渲染的图像，值越高运动轨迹越长
 - Extra Blur：额外模糊，选中此项，则在前帧的基础上计算更多的模糊效果，从而使运动轨迹更加模糊

 #### Noise And Grain：噪点与颗粒特效

 噪点与颗粒特效用于模拟电源、老旧电视或录像中的噪点、胶片颗粒等效果，其属性如下：

 - DirectX 11 Grain：DX11颗粒，选中则启用高质量的噪波和颗粒（只能用于DirectX 11）
 - Monochrome：单色，选中则仅使用灰度噪点
 - Intensity Multipiler：强度系数，用于控制整体亮度值
 - General：总体，用于为所有光亮区域均匀增加噪点效果
 - Black Boost：黑色增强，用于增强低亮度噪点效果
 - White Boost：白色增强，用于增强高亮度噪点效果
 - Mid Grey：中灰，用于定义低亮度噪点和高亮度噪点的范围
 - Texture：纹理，单击该选项右侧的圆圈按钮可以为噪点指定纹理，该项在未选择DirectX 11 Grain时有效
 - Filter：过滤器，为纹理指定过滤方式
    1. Point:点（最近采样）
    2. Bilinear：双线性
    3. Trilinear：三线性
- Softness：柔和度，用于定义噪点或颗粒的细碎程度，值越高，噪点或颗粒的细碎度越低。高柔和度的效果能获得更好的表现力
- Tilling：平铺，用于控制噪点或颗粒的平铺模式

#### Sepia Tone：棕褐色调特效

棕褐色调特效利用一种算法将游戏画面的色调调整为棕褐色，一般用于模拟老照片的效果。

#### Screen Space Ambient Occlusion：屏幕空间环境遮挡（SSAO）特效

 屏幕空间环境遮挡（SSAO）特效用于实时模拟场景的环境遮挡效果，在一定程度上模拟真实的全局光漫反射效果，其属性如下：

 - Radius：半径，用于控制环境遮挡效果的范围值
 - Sample Count：采样数量，用于设置环境遮挡效果所需采样点的数量，有3种选项
    1. Low：低品质，最多采样值
    2. Medium：中等品质，中等数量采样值
    3. High：高品质，最高的采样值
- Occlusion Intensity：遮蔽强度，与环境遮蔽相关的黑暗程度
- Blur：模糊度，用于控制环境遮蔽所产生的暗部区域的模糊程度
- Downsampling：低采样值，用于设置低采样的数值，值越大，渲染效率越高，但质量随之下降
- Occlusion Attenuation：遮挡衰减，用于控制遮挡程度随距离变化的衰减度
- Min Z：最小Z值，用于减少不必要的遮挡效果

#### Tonemapping：色调映射特效

色调映射特效需要在摄像机开启HDR模式时才能正常使用，设置较高的光源强度值会产生更明显的效果，配合Bloom特效一起使用会得到更好的效果，其属性为：

- Technique：技术，用于指定色调映射的计算方法，即如何将高动态光照渲染产生的高范围光照度映射至显示设备能显示的低范围内。共有7项可选择（Simple Reinhar、User Curve、Hable、Photographic、Optimized Heji Dawson、Adaptive Reinhard、Adaptive Reinhard Auto White）
- Exposure：曝光度，用于模拟曝光的程度用于定义亮度范围
- Remap Curve：映射曲线，该项只在Technique指定为User Curve模式下有效
- Middlegrey：中灰，用于控制场景中间灰度的亮度值，该项只在Technique指定为Adaptive Reinhard、Adaptive Reinhard Auto White模式下有效
- White：白色，用于指定被映射为白色的最小亮度值，该项只在Technique指定为Adaptive Reinhard模式下有效
- Adaption Speed：适应速度，用于控制所有自适应色调映射器的调整速度，该项只在Technique指定为Adaptive Reinhard、Adaptive Reinhard Auto White模式下有效
- Texture size：纹理尺寸，用于为所有自适应色调映射器指定内部纹理尺寸，比较大的纹理值可以在计算新亮度时获得更多细节

#### Twirl：扭曲特效

扭曲特效是指在一个圆形区域内扭曲所渲染图像的一种效果，该特效是将图像围绕着一个点进行旋转，其属性为：

- Radius：半径，基于屏幕坐标系来设置用于变形圆形区域的半径
    1. X：用于控制椭圆区域屏幕的宽度
    2. Y：用于控制椭圆区域屏幕的高度
- Angle：角度，用于控制扭曲效果的旋转角度
- Center：中心，基于屏幕坐标系设定用于变形的圆形区域中心点的位置
    1. X：用于控制椭圆区域中心点在屏幕横向上的位置
    2. Y：用于控制椭圆区域中心点在屏幕纵向上的位置

