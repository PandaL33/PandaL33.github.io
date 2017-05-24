---
layout:     post
title:      "Fixed Function Shaders"
subtitle:   " \"固定功能管线着色器\""
date:       2017-05- 09:37:26
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity3D
    - Shader
---
## 固定功能管线着色器

固定功能管线着色器一般用于不支持高级着色器特性的旧硬件上，采用ShaderLab语言进行编写，其关键代码一般在Pass的材质设置Material{}和纹理设置SetTexture{}部分。

下面以Unity3D内建的顶点光照着色器为例进行说明：

```
//顶点光照着色器代码
Shader "VertexLit"{
    Properties{//属性定义
        _Color("Main Color",Color)=(1,1,1,0.5)
        _SpecColor("Sec Color",Color)=(1,1,1,1)
        _Emission("Emmisive Color",Color)=(0,0,0,0)
        _Shininess("Shinniness",Range(0.01,1))=0.7
        _MainTex("Base (RGB)",2D)="white"{}
    }
    SubShader{
        Pass{//Pass定义
            Material{//设置光照所需的材质参数
                Diffuse[_Color]
                Ambient[_Color]
                Shininess[_Shininess]
                Specular[_SpecColor]
                Emission[_Emission]
            }
            Lighting On  //开启光照
            SeparateSecular On //开启高光颜色
            SetTexture[_MainTex]{//设置纹理
                constantColor[_Color] //设置一个常量颜色值
                combine texture*primary DOUBLE,texture*constant //混合命令
            }
        }
    }
}
```
