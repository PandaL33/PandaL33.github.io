---
layout:     post
title:      "Introduction To Light System"
subtitle:   " \"Unity3D\""
date:       2017-03-29 21:00:00
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity3D
---
## 灯光种类

灯光种类分为：

- Directional Light:平行光
- Point Light：点光源
- Spot Light：聚光灯
- Area Light：区域灯（在渲染烘焙后才产生照明效果）

## 灯光参数介绍

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/introduction-to-light-system/introduction-to-light-system-1.png)

#### Baking 烘培
- Realtime：实时烘培
- Baked：手动烘培
- Mixed:：混合烘培

#### Range 照明范围
#### Spot Angle 照明角度
#### Color 照明颜色
#### Intensity 照明强度
#### Bounce Intensity 反射照明角度
#### Hard Shadows 阴影类型
- Strength 阴影强度
- Resolution 阴影分辨率
- Bias 调节阴影与物体之间的距离
- Normal Bias 阴影法线参数
- Shadow Near Plane 阴影贴面

#### Cookie 灯光可将透明通道的图片投影到物体
#### Draw Halo 光晕
#### Culling Mask 灯光蒙板（用于剔除不需要照明的物体）

## 阴影的常见问题及处理

1、老显卡不支持阴影；

2、Edit->Project Settings->Quality->Shadows
Quality Settings 质量设置中关闭了阴影显示。确保质量层级Quality Level设置合理；

![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/introduction-to-light-system/introduction-to-light-system-2.png)

3、场景中所有物体的面渲染器MeshRenderers必须设置为可接受阴影和产生阴影；

4、只有不透明的物体才产生阴影，使用了内置的透明Shader的物体不产生阴影；

5、使用了VertexLit的Shader的物体不能接受阴影，但可以产生阴影；