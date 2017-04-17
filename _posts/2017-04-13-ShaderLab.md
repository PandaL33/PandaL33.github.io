---
layout:     post
title:      "ShaderLab"
subtitle:   " \"Unity3D\""
date:       2017-04-13 20:31:22
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity3D
---
## ShanderLab

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
||||

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

### Subshader
Subshader基本语法：
```
Subshader{[Tags] [CommonState] Pass{}}
```
定义通道的类型有：RegularPass、UsePass和GrabPass。

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
||||

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
通道渲染设置命令（RenderSetup）有：
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
||||