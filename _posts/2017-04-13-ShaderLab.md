---
layout:     post
title:      "ShaderLab"
subtitle:   " \"Unity3D\""
date:       2017-04-13 20:31:22
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity3D Shader
---
## ShaderLab

Unity中的着色器程序使用的是ShaderLab着色语言，同时支持使用Cg、HLSL和GLSL编写的着色器程序。

### Shader

Shader命令的语法为
```
Shader "name"{
    [Properties]
    Subshaders{…}
    [Fallback]
}
```

Subshaders中包含一个子着色器的列表，其中至少有一个子着色器。当加载一个着色器程序时，Unity将遍历这个列表，获取第一个能被用户机器支持的子着色器。如果没有子着色器被支持，Unity将尝试使用降级着色器，即Fallback操作。

### Properties
Properties用来定义在显示材质设定界面的属性列表。 

|类型    | |说明| 
|:--------|---------:|:---------|
| name("display name",Range(min,max))=num  || 定义浮点数范围属性，在属性查看器中可通过一个标注了最大值和最小值的滑动条来修改|
| name("display name",Float)=num  ||定义浮点数属性| 
| name("display name",Int)=num  ||定义整形属性| 
| name("display name",Color)=(num,num,num,num)  ||定义颜色属性，num取值范围0~1| 
| name("display name",Vector)=(num,num,num,num) ||定义四维向量属性| 
| name("display name",2D)="name"{options}   ||定义2D纹理属性，缺省值为："while""black""gray""bump"| 
| name("display name",Rect)="name"{options}  ||定义矩形纹理（尺寸非2次方）属性，缺省值同2D纹理属性| 
| name("display name",Cube)="name"{options}  ||定义立方贴图纹理属性，缺省值同2D纹理属性| 
| name("display name",3D)="name"{options}  ||定义3D纹理属性| 

例：
```
Properties{
    _RangeValue("Range Value",Range(0.1,0.5))=0.3
    _FloatValue("Float Value",Float)=0.3
    _Color("Color",Color)=(1,1,1,1)
    _Vector("Vector",Vector)=(1,1,1,1)
    _MainTex("Albedo (RGB)",2D)="while"{TexGen EyeLinear}
    _Cube("CubeTex",Cube)="skybox"{TexGen CubeReflect}
}
```

options选项包含如下几项：

- TexGen：纹理生成模式，可以是ObjectLinear、EysLinear、SphereMap、CubeReflect、CubeNormal中一种，这些模式与OpenGL的纹理生成模式相对应。如果使用了自定义的顶点程序，那么该参数将被忽略；
- LightmapMode：选择该选项，则纹理 将受渲染器的光照贴图参数影响。纹理将不会从材质中获取，而是取自渲染器的设置。

### Subshader
Subshader包含子着色器标签（可选）、通用状态（可选）和Pass列表组成，基本语法：
```
Subshader{[Tags] [CommonState] Pass{}}
```
定义Pass通道的类型有：RegularPass、UsePass和GrabPass。

例：
```
Subshader{
    Tags{"Queue"="Transparent"}      //渲染队列为透明队列
    Pass{
        Lighting Off                 //关闭光照
        SetTexture[_MainText]{}      //设置纹理
    }
}
```

### Subshader Tags
子着色器使用标签Tags告诉Unity渲染引擎或者其他用户如何认证这个SubShader。
Tags基本语法：
```
Tags{"标签1"="值1" "标签2"="值2"}
```
标签的标准是键值对，可以有任意个。常用的标签如下：
- Queue tag——队列标签，Queue标签用来决定对象被渲染的次序。 着色器决定对象所归属的渲染队列，任何透明物体都可以通过这种方法确保自身在不透明物体渲染之后渲染。预选值有：
    1. Background（背景），默认值为1000；
    2. Geometry（几何体），默认值为2000；
    3. AlphaTest（Alaph测试），默认值为2450；
    4. Transparent（透明），默认值为3000；
    5. Overlay（覆盖），默认值为4000；
- 自定义队列标签
自定义队列通过自定义一个队列，来满足特殊的需要；
例如：
```
Tags{"Queue"="Geometry+600"}
```

    上述代码将对象的设置渲染队列设为2600，介于AlphaTest队列和Transparent队列之间。
- RenderType tag——渲染类型标签，RenderType标签将着色器分为若干个预定义组。

|对列名称    | |说明| 
|:--------|---------:|:---------|
| Opaque  || 不透明，用于大多数着色器（法线着色器、自发光着色器、反射着色器以及地形着色器）|
| Transparent  ||透明，用于大多数半透明着色器（透明着色器、粒子着色器、字体着色器、地形额外通道着色器）| 
| TansparentCutout  ||遮蔽的透明着色器（透明镂空着色器、两个通道植被着色器）| 
| Background  ||天空盒着色器| 
| Overlay ||GUITexture、光晕着色器、闪光着色器| 
| TreeOpaque || 地形引擎树皮着色器 | 
| TreeTransparentCutout || 地形引擎树叶 | 
| TreeBillboard ||地形引擎布告板树 | 
| Grass ||地形引擎草| 
| GrassBillboard ||地形引擎布告板草| 

- DisableBatching tag——禁用批处理标签。
    1. True:着色器一直禁用批处理；
    2. False：不禁用批处理，默认值；
    3. LODFading：当LOD fading被激活时禁用批处理；
- ForceNoShadowCasting tag——强制不投射阴影标签。
- IgnoreProjecttor——忽略投影标签。
- CanUseSpriteAtlas tag——使用精灵图集标签。
- PreviewType tag——预览类型标签，默认以材质球形式展示。

### Pass
Subshader的渲染方案由一个个通道（Pass）来执行，Subshader可以包含多个Pass块，每个Pass都使几何对象被渲染一次。
Pass的基本语法为：
```
Pass{[Name and Tags] [RenderSetup] [TextureSetup]}
```

- 名称和标签（Name and Tags）：可以定义Pass名字以及任意数量的标签，为Pass命名后，可以在别的着色器上通过Pass名称来引用它。标签则可以用来向渲染管线说明Pass的意图，形式为“键-值”。

- 通道渲染设置命令（RenderSetup）有：

|命令    |含义 |说明| 
|:--------|:---------|:---------|
| Lighting  |光照| 开启或关闭顶点光照，开关状态的值为On或Off|
| Material{材质块}  |材质|定义一个使用顶点光照管线的材质| 
| ColorMaterial  |颜色集|当计算顶点光照时使用顶点颜色，颜色集可以是AmbientAndDiffuse或Emission| 
| Color  |颜色|设置当顶点光照关闭时所使用的颜色| 
| Fog{雾块} |雾|设置雾参数| 
| AlphaTest |Alpha测试| Less、Greater、LEqual、GEqual、Equal、NotEqual、Always,默认值为LEqual | 
| ZTest |深度测试模式| 设置深度测试模式：Less、Greater、LEqual、GEqual、Equal、NotEqual、Always| 
| ZWrite |深度写模式|开启或关闭深度写模式，开关状态的值为On、Off | 
| Blend |混合模式|设置混合模式：SourceBlendMode、DestBlendMode、AlphaSourceBlendMode、AlphaDestBlendMode| 
| ColorMask |颜色遮罩|设置颜色遮罩，颜色值可以是RGB或A或0或任何R、G、B、A的组合，设置为0将关闭所有颜色通道的渲染| 
| Offset |偏移因子|设置深度偏移，这个命令仅接收常数参数| 
| Cull |裁剪|设置裁剪模式，模式包含Back、Front和Off| 
| Stencil |蒙版|用蒙版来实现像素的取舍，选项有Keep、Zero、Replace、IncrSat、DecrSat、Invert、IncrWarp和DecWrap| 
| ColorMask |颜色遮罩|设置颜色遮罩，当值为0时关闭所有颜色通道的渲染，取值为RGB、A、0或R,G,B,A的任意组合| 
| SeparateSecular |高光颜色|开启或关闭顶点光照的独立高光颜色，取值为On或Off| 

- 纹理设置（TextureSetup）
纹理设置用于固定管线，如果使用表面着色器或自定义的顶点及片段着色器，那么纹理设置将被忽略。

纹理设置语法为：
```
SetTexture 纹理属性 {[命令选项]}
```
命令选项包括：

1. combine：将两个颜色源混合，混合的源可以是Previous（上一次SetTexture的结果）、constant（常量颜色值）、primary（顶点颜色）和texture（纹理颜色）。
2. constantColor:设置一个常量颜色值。
3. matrix：设置矩阵对纹理坐标进行转换。

* UsePass：可以通过UsePass来重用其他着色器中命名的Pass，例如：

```
UsePass "Specular/BASE" //使用高光着色器Specular中名为BASE的Pass
```

* GrabPass：将屏幕抓取到一个纹理中，供后续的Pass使用，可以通过_GrabTexture来访问。

### Fallback
降级（Fallback）定义在所有子着色器后，如果没有任何子着色器能被运行到当前硬件上，将使用降级着色器。常用语法如下：
- Fallback "着色器名"
    退回到给定名称的着色器。
- Fallback Off
    显示声明没有降级并且不会打印任何警告，甚至没有子着色器会被当前硬件运行。

### Category（分类）
分类用于提供子着色器继承的命令，比如着色器中的多个子着色器都需要关闭雾效、设置混合模式。例：
```
Shader "example"{
    Category{
        Fog{Mode Off}
        Blend One One
        SubShader{

        }
        //...
        SubShader{

        }
    }
}
```

### Unity3D 5.X 内置着色器

#### 根据应用对象的不同分为：

- 普通（Normal）：用于不透明的对象；
- 透明（Transparent）：用于半透明和全透明的对象；
- 透明镂空效果（TransparentCutOut）：用于由完全透明和完全不透明部分组成（不含半透明部分）的对象，像栅栏一样；
- 自发光（Self-IIIuminated Shader Family）：用于有自发光效果的对象；
- 反射（Reflective Shader Family）：用于能反射环境立方体贴图的不透明对象；

#### 不同光照效果的计算开销从低到高排序

- Unlit：仅适用纹理颜色，不受光照影响；
- VertexLit：顶点光照；
- Diffuse：漫反射；
- Specular：在漫反射基础上增加高光计算；
- Normal Mapped：法线贴图，增加一张法线贴图和几个着色器指令；
- Normal Mapped Specular：带高光法线贴图；
- Parallax Normal Mapped：视差法线贴图，增加了视差贴图的计算开销；
- Parallax Normal Mapped Specular：带高光视差法线贴图

#### StandardShader 属性

|属性    | |含义| 
|:--------|---------:|:---------|
| Rendering Model  || 渲染模式，包含Opaque（不透明）、Fade（渐变）、Transparent（透明）和Cutout（镂空）|
| Albedo  ||漫反射，设置漫反射的贴图或颜色值| 
| Metallic ||金属，设置金属的贴图或颜色值（不能和Smoothness属性同时应用）| 
| Smoothness ||光滑度，设置物体表面的光滑程度（不能和Metallic属性同时应用）| 
| Normal Map ||法线贴图，用于描绘物体表面凹凸程度的法线贴图| 
| Height Map ||高度图，用于描述视差偏移的灰度图| 
| Occlusion ||散射，用于设置照射到物体表面的非直接光照散射的贴图| 
| Emission ||自发光，用于控制物体表面自发光颜色和强度的贴图| 
| Detail Mask ||细节蒙版，用于设置Secondary Maps的蒙版贴图| 
| Tilling ||平铺，用于设置贴图在物体表面的平铺值| 
| Offset ||偏移，用于设置贴图在物体表面的偏移值| 
| Secondary Maps ||2号贴图，带UV通道的2号贴图| 
| Detail Albedo  ||细节漫反射，2号贴图的漫反射贴图| 
| Normal Map  ||法线贴图，2号贴图的法线贴图| 
| Tilling  ||平铺，用于设置2号贴图在物体表面的平铺值| 
| Offset  ||偏移，用于设置2号贴图在物体表面的偏移值| 
| UV Set  ||UV集，用于设置物体的UV集| 