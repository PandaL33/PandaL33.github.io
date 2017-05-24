---
layout:     post
title:      "Surface Shaders"
subtitle:   " \"表面着色器\""
date:       2017-05-24 10:11:13
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity3D
    - Shader
---
## 表面着色器

使用Unity3D的表面着色器，只需要编写最关键的表面函数，其余周边代码将由Unity3D自动生成，包括适配各种光源类型、渲染实时阴影以及集成到前向/延迟渲染管线中等。

编写表面着色器的规则如下：
- 表面着色器的实现代码需要放置在CGPROGRAM...ENDCG代码块中，而不是Pass结构中，它会自己编译到各个Pass。
- 使用#pragma surface...命令来指明它是一个表面着色器，例

```
#pragma surface 表面函数 光照模型 [可选参数]
```

其中“表面函数”用来说明那个CG函数包含有表面着色器代码，其作用是接收输入的UV或者附加数据，然后进行处理最后将结果填充到输出结构体SurfaceOutput中。表面函数形式为：

```
void surf(Input IN,inout SurfaceOutput 0)
```

“光照模型”可以是内置的Lambert和BlinnPhong，或者是自定义的光照模型。输入结构体Input一般包含着色器所需的纹理坐标，纹理坐标的命名规则为uv加纹理名称（当使用第二张纹理时使用uv2加纹理名称），另外还可以在输入结构中设置一些附加数据，具体内容如下：

|附加数据    | |说明| 
|:--------|---------:|:---------|
| float3 viewDir|| 视角方向|
| float4 COLOR  ||每个顶点的插值颜色| 
| float4 screenPos ||屏幕坐标（使用.xy/.w来获取屏幕2D坐标）| 
| float3 worldPos  ||世界坐标| 
| float3 worldRefl ||世界坐标系中的反射向量| 
| float3 worldNormal ||世界坐标系中的法线向量| 
| INTERNAL_DATA ||当输入结构包含worldRefl或worldNormal且表面函数会写入输出结构的Normal字段时需包含此声明| 

SurfaceOutput描述了表面的各种参数，标准结构为:
```
struct SurfaceOutput{
    half3 Albedo;       //反射光
    half3 Normal;       //法线
    half3 Emission;     //自发光
    half Specular;      //高光
    half Gloss;         //光泽度
    half Alpha;         //透明度
}
```

表面着色器代码：

```
Shader "Surface Shader Example/WorldRefl"{
    Properties{//属性定义
        _MainTex("Base (RGB)",2D)="white"{}
        _Cube("Cubemap",CUBE)=""{}//立方体贴图属性
    }
    SubShader{
        Tags{"RenderType"="Opaque"}
        CGPROGRAM
        #pragma surface surf Lambert
        struct Input{
            float2 uv_MainTex;
            float3 worldRefl;//输入反射参数
        };
        smapler2D _MainTex;
        samplerCUBE _Cube;
        void surf(Input IN,inout SurfaceOutput o)
        {
            o.Albedo=text2D(_MainTex,IN.uv_MainTex).rgb*0.5;
            O.Emission=texCUBE(_Cube,IN.worldRefl).rgb;//将反射颜色设置给自发光颜色
        }
        ENDCG
    }
    Fallback "Diffuse"
}
```
