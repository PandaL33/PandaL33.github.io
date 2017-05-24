---
layout:     post
title:      "Vertex and Fragment Shaders"
subtitle:   " \"顶点片段着色器\""
date:       2017-05-24 14:07:33
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity3D
    - Shader
---
## 顶点片段着色器

顶点片段着色器运行于具有可编程渲染管线的硬件上，包括顶点程序（Vertex Programs）和片段程序（Fragment Programs）。当使用顶点程序或片段程序渲染时，图形硬件的固定功能管线将会关闭，顶点程序会替换固定管线中标准的3D变换、光照、纹理坐标生成等功能，片段程序会替换掉SetTexture命令中的纹理混合模式。

编写顶点片段着色器的形式如下：

```
Pass{
    //通道设置
    CGPROGRAM
    //本段CG代码的编译指令
    #pragma vertex vert
    #pragma fragment frag
    //CG代码
    ENDCG
    //其他通道设置
}
```
编译命令说明：
- 编译命令（Compilation Directive），可用的编译命令如下表：

|命令    | |说明| 
|:--------|---------:|:---------|
| #pragma vertex name || 将函数name的代码编译成顶点程序|
| #pragma fragment name  ||将函数name的代码编译成片段程序| 
| #pragma geometry name  ||将函数name的代码编译成DX10的几何着色器（Geometry Shader）| 
| #pragma hull name  ||将函数name的代码编译成DX11的hull着色器| 
| #pragma domain name ||将函数name的代码编译成DX11的domain着色器| 
| #pragma fragmentoption option ||添加选项到编译的OpenGL片段程序。对于顶点程序或编译目标不是OpenGL的程序无效| 
| #pragma targe name ||设置着色器的编译目标| 
| #pragma only_renders space separated names ||仅编译到指定的渲染平台| 
| #pragma exclude_renderers space separated names ||不编译到指定的渲染平台| 
| #pragma glsl ||为桌面系统的OpenGL进行编译时，将Cg/HLSL代码转换成GLSL代码| 
| #pragma glsl_no_auto_normalization ||编译到移动平台GLSL时，关闭在顶点着色器中对法线向量和切线向量自动进行规范化| 

- 编译目标（Shader targets）：编译目标可以设置为#pragma target 2.0、#pragma target 3.0、#pragma target 4.0、#pragma target 5.0；

- 渲染平台（Rendering Platforms），Unity支持多种图形API包括：
    1. d3d9
    2. d3d11
    3. opengl
    4. gles
    5. gles3
    6. metal
    7. d3d11_9x
    8. xbox360
    9. ps3
    10. ps4
    11. psp2

顶点片段着色器代码：

```
Shader "Tutorial/Display Normals"{
    SubShader{
        CGPROGRAM
        #pragma vertex vert
        #pragma fragment frag
        #include "UnityCG.cginc"
        struct v2f{
            //顶点数据结构体
            float4 pos:SV_POSITION;
            float3 color:COLOR0;
        };
        v2f vert(appdata_base v)
        {
            //顶点程序代码，计算位置和颜色
            v2f o;
            o.pos=mul(UNITY_MATRIX_MVP,v.vertex);
            o.color=v.normal*0.5+0.5;
            returen o;            
        }
        half4 frag(v2f i):COLOR
        {
            //片段程序代码，直接返回输入的颜色，并把透明度设置为1
            return half4(i.color,l);
        }
        ENDCG
    }
    Fallback "VertexLit"
}
```
