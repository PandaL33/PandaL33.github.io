---
layout:     post
title:      "Rigidbody And Collider"
subtitle:   " \"Unity3D\""
date:       2017-04-07 21:00:00
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity3D
---
## 前言

物理引擎通过刚性物体赋予真实的物理属性的方式来计算它们的运动、旋转和碰撞反应。

## 刚体

#### 刚体属性

- 质量（Mass）：表示刚体的质量，数据类型为float，默认值为1（一般取值范围为0.1~10.0）。
- 阻力（Drag）：物体移动的阻力，数据类型为float，默认值为0（方向与物体运动方向相反）。
- 旋转阻力（Angular Drag）：物体在因受瞬时力而旋转后，受到的阻碍力，数据类型为float，默认值为0.05。
- 使用重力（Use Gravity）：是否启用重力，数据类型为bool，默认值为true。
- 是否遵循运动学（Is Kinematic）：游戏对象是否遵循牛顿运动学物理定律，数据类型bool，默认值为false（属性值为true时，表示对象的运动只受到脚本和动画的影响，作用力、关节和碰撞都不会对其产生任何作用）。
- 插值方式（Interpolate）：物体运动的插值模式，默认值为None（内插值Interpolate和外插值Extrapolate）。
- 碰撞检测模式（Collision Detection）：一个高速运动的物体，两个相邻物理模拟时间点所进行的位移大于被碰撞物体的厚度，且本身厚度足够小，则该物体将有可能直接穿透被碰撞的物体。
    1. 离散模式（Discrete）：静止或运动速度较慢的物体使用该模式；
    2. 连续模式（Continuous）：高速运动或体积较小的物体建议使用该模式；
    3. 动态连续模式（Continuous Dynamic):被使用了连续检测模式的物体所撞击的物体建议使用该模式；
- 约束条件（Constraints）：物体的位移或旋转是否受到物理定律的约束，默认状态下物体的任意方向的位移和任意轴的旋转都受到物理定律的约束。

#### 刚体变量
- 角速度（angularVelocity）：表示刚体的角速度向量，数据类型为Vector3，该向量的方向为刚体旋转轴的方向（遵循左手法则）。
- 位移速度（velocity）：表示物体的位移速度值。
- 重心（centerOfMass）：表示物体的重心，以模型坐标系为准，不是世界坐标系。
- 碰撞检测开关（detectCollisions）：是否启用碰撞检测，数据类型为bool，默认值为true。
- 惯性张量（inertiaTensor）：表示物理的转动惯量，数据类型为Vector3。
- 惯性张量旋转（inertiaTensorRotation）：表示无力惯性张量的旋转值，数据类型为Quaternion。
- 最大角速度（maxAngularVelocity）：用于设置物体的最大角速度，数据类型为float，默认值为7（非负数）。
- 最大穿透速度（maxDepenetrationVelocity）：限制物体碰撞穿透的速度，数据类型为float，默认值为无限大。
- 坐标（position）：刚体在世界坐标系中的坐标（物理模拟中的坐标值与transform.position不同），数据类型为Vector3。
- 旋转（rotation）：表示刚体在世界坐标系中的旋转（物理模拟中的旋转值与transform.rotation不同），数据类型为Quaternion。
- 是否使用锥形摩擦（useConeFriction）：是否使用锥形摩擦，数据类型为bool，默认值为false（对资源消耗较大）。

#### 刚体方法
- 施加力（"void AddForce(Vector3 force,ForceMode mode)"）：给刚体施加一个沿着force（基于世界坐标）方向的力，类型ForceMode包括计算重力的连续力、忽略重力的连续加速力、计算重力的瞬时力、忽略重力的瞬时力。
- 移动刚体（"void MovePosition(Vector3 position)"）：将刚体移动到相对应的位置，该方法用于FixedUpdate方法中。
- 旋转刚体（"void MoveRotation(Quaternion rot)"）：将刚体旋转到相对应的角度。
- 添加爆炸力（"void AddExplosionForce(float explosionForce,Vector3 explosionPosition,float explosionRadius,flaot upwardsModifier,ForceMode mode)"）：表示将在explosionPosition处产生模式为mode，大小explosionForce的爆炸力，半径为explosionRadius，并在物体下方upwardsModifier向上施力。
- 在指定点施加力（"void AddForceAtPosition(Vector3 force,Vector3 position,ForceMode mode)"）：表示将在position（世界坐标系）处添加一个mode模式、force大小的力。
- 施加相对力（"void AddRelativeForce(Vector3 force,ForceMode mode)"）：表示将向物体施加一个沿着force（基于物体的模型坐标）方向的力，该力的模式为mode。
- 施加力矩（"void AddTorque(Vector3 torque,ForceMode mode)"）：表示将向刚体施加一个torque的力矩(基于世界坐标系)。
- 施加相对力矩（"void AddRelativeTorque(Vector3 torque,ForceMode mode)"）：表示将向刚体施加一个torque的力矩（基于物体坐标系）。
- 计算相对刚体的最近点（"void ClosestPointOnBounds(Vector3 position)"）：用于计算在刚体所包含的三维空间内，与position距离最短的点的坐标。
- 获取基于点坐标系的速度（"void GetPointVelocity(Vector3 worldPoint)"）：给定一个基于世界坐标系的点worldPoint，调用此方法可以计算出刚体在以worldPoint为原点的坐标系中的速度。
- 获取基于相对点坐标系的速度（"void GetRelativePointVelocity(Vector3 relativePoint)"）：给定一个基于刚体模型坐标系的点relativePoint，调用此方法可以计算出刚体在以relativePoint为原点的坐标系中的速度。
- 是否处理休眠（"void IsSleeping()"）：表示刚体是否处于休眠状态。
- 设置密度（"void SetDensity(float density)"）：给刚体设置一个密度值，该密度值基于碰撞器的体积，而不是物体的体积。
- 强制休眠 （"void Sleep()"）：表示将刚体强制进行休眠，不参与物理模拟计算。
- 唤醒（"void WakeUp()"）：表示将处于休眠状态的刚体进行唤醒，重新加入物理模拟计算。
- 扫描检测（"void SweepTest(Vector3 direction,out RaycastHit hitInfo,float maxDistance)"）：表示将沿着direction的方向产生一条长度为maxDistance的射线hitInfo，若该射线碰撞到其他物体，则返回true，否则返回false。
- 扫描检测所有（"RaycastHit[] SweepTestAll(Vector3 direction,float maxDistance)"）：返回储存了direction方向所有检测到的刚体的信息，数组长度不超过128。

## 碰撞器

####


