---
{"dg-publish":true,"permalink":"/adv/plan-path-finder/","title":"Plan - Path Finder","noteIcon":""}
---

# 机器人项目

## 开源

1. ROS2 （Robot Operating System 2）是机器人操作系统
2. OCS2 (Optimal Control for Switched Systems）是一个专为切换系统优化控制设计的C++工具箱。它提供了多种高效算法的实现。
	1. SLQ：连续时域约束的DDP。（Continuous-time domain constrained DDP.
	2. iLQR：离散时间域约束DDP。（Discrete-time domain constrained DDP.）
	3. SQP：基于HPIPM的多重拍摄算法。（Multiple-shooting algorithm based on [HPIPM](https://github.com/giaf/hpipm)
	4. SLP：基于PIPG的顺序线性规划。（Sequential Linear Programming based on [PIPG](https://arxiv.org/abs/2009.06980).）
	5. IPM：基于非线性内点法的多次射击重算法。（Multiple-shooting algorithm based on nonlinear interior point method.）
3. Mujoco 物理引擎

## 组件

1. 谐波减速电机，RV减速电机，直驱轮毂电机，直线推杆电机，摆线减速执行器
2. 增材制造仿真
3. 工业零部件超市 - McMaster-Carr
4. 数据交换格式 - STEP
5. 无刷电机FOC驱动器
6. CAN总线控制
7. 下位机MCU：Cortex-M7，Wemos Mini Pro 开发板
8. 上位机: GPD Micro PC 控制，规划算法
9. 深度相机：RealSense视觉算法
10. 足底传感器，足底触地检测，地面反作用力矩检测
11. FPV 操控
12. 线性倒立摆+ZMP
13. 关节输出力矩闭环
14. 轴承：推力轴承
15. 万向节：鱼眼万向节
16. 串并联驱动组合变形，串联驱动（Series Drive）：动力源（如电机）通过一系列机械元件（如齿轮、链条、皮带等）直接驱动负载，并联驱动（Parallel Drive）：多个动力源（如电机）同时驱动同一个负载
17. SOC芯片，全称为System on Chip（系统级芯片）。
18. 域控制器DCU
19. 机器人腿部准直驱驱动 OpenTorque

## 概念

1. MPC（Model Predictive Control 模型预测控制）是一种控制策略，它通过建立系统的模型，预测未来的状态，并优化控制输入以实现期望的输出。
2. WBC（Whole Body Control全身体平衡控制）它将机器人视为一个整体，通过优化各个关节的运动来实现整体的平衡和稳定。
3. ZMP（Zero Moment Point）是指在动态平衡条件下，地面与机器人足部接触区域的反作用力在水平方向上**不产生力矩**的点。
4. LIPM（Linear Inverted Pendulum Model, 线性倒立摆）是一种在控制理论和机器人学中常用的简化模型