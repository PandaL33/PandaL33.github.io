---
layout:     post
title:      "Navigation"
subtitle:   " \"Unity3D\""
date:       2017-05-22 15:36:12
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity3D
---
## Navigation 导航

### Object参数面板

- Navigation Static：该对象将参与导航网格的烘焙；
- Generate OffMeshLinks：可以自动根据Drop Height（下落高度）和Jump Distance（跳跃距离）的参数设置用关系线来连接分离的网格；
- Navigation Area：导航区域设置。在默认情况下分为Walkable（行走区域）、Not Walkable（不可行走层）和Jump（跳跃层）；

### Bake参数面板

- Agent Radius：具有代表性的物体半径。物体半径越小，生成网格的面积越大，越靠近静态物体的边缘；
- Agent Height：具有代表性的物体高度；
- Max Slope：最大可行进的斜坡斜度；
- Step Height：台阶高度；
- Drop Height：允许的最大下落距离；
- Jump Distance：允许的最大跳跃距离；
- Advanced：高度参数调节
    1. Manual Voxel Size：是否手动调整烘焙的单元尺寸；
    2. Voxel Size：烘焙单元尺寸，控制烘焙的精度；
    3. Min Region Area：网格面积小于该值的地方，将不生成导航网格；
    4. Height Mesh：选中该选项，将会保存高度信息，同时也会消耗一些性能和存储空间；

### Nav Mesh Agent参数面板

- Agent Size：尺寸控制
    1. Radius：物体的半径；
    2. Height：物体的高度；
    3. Base Offset：偏移值；
- Steering：行动控制
    1. Speed：物体的行进最大速度；
    2. Angular Speed：行进过程中转向时的角速度；
    3. Acceleration：物体的行进加速度；
    4. Stopping Distance：距离目标点小于多远距离后便停止行进；
- Auto Braking：选中后自动制动
- Obstacle Avoidance：躲避障碍参数
    * Quality：质量
        1. None：无；
        2. Low Quality：低质量；
        3. Medium Quality：中等质量；
        4. Good Quality：较好质量；
        5. High Qulaity：高等质量；
    * Priority：优先权
- Path Finding：路径寻找
    1. Auto Traverse Off Mesh Link：是否采用默认方式度过连接路径；
    2. Auto Repath：在行进过程中，因某些原因中断的情况下，是否重新开始寻路；
    3. Auto Mashk：自动遮罩；

