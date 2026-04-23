---
title: "1-D Linear Convection Equation`"
date: 2026-03-01T12:00:00+08:00
lastmod: 2026-03-02T15:00:00+08:00
draft: false  
summary: "方程式の勉強とコード練習"
tags: ["有限差分法", "code", "N-S", "CFD"]
categories: ["code", "CFD"]

featureimage: ""        
featureimagecaption: "勉強記録"
showHero: true                      
heroStyle: "basic"                     
showTableOfContents: true          
showReadingTime: true                 
showWordCount: true               
showComments: true             
showBreadcrumbs: true               

series: ["12 steps to Navier-Stokes Part 1"]
series_order: 1                       
externalLinkForceNewTab: true         
sharingLinks: ["twitter", "facebook"] 
---

{{< katex >}}


# Steps 1 一维线性对流方程 (1-D Linear Convection Equation)
## 控制方程

一维线性对流方程是CFD起点，以下是它的数学形式
$$\frac{\partial u}{\partial t} + c \frac{\partial u}{\partial x} = 0$$


CFD核心的两个底层逻辑：数值离散的物理意义和数值稳定性的本质。

## 1、方程的物理意义：用污染物扩散举个例子

结合我自身的研究方向——城市街区的污染物扩散来理解这个方程：
 
 $u(x,t)$：可理解为某条街道上，污染物的浓度随位置$x$和时间$t$的分布

 $c$：恒定风速，代表污染物被风吹动的速度，方向沿$x$正方向；

 整个方程的物理意义：污染物会以恒定的风速，沿着风向（正方向）整体移动，波形形状保持不变。
这是一个理想的“波形平移”过程——如果初始时刻街道中间有一个“污染物浓度峰”，随着时间推移，这个峰会沿着风向匀速移动，不会变宽、变矮，也不会出现扭曲，完全符合无扩散、无损耗的理想对流规律。

## 2、有限差分离散：对比了迎风格式和中心差分格式

### 迎风格式
$$\frac{\partial u}{\partial x} \approx \frac{u_i^n - u_{i-1}^n}{\Delta x}$$
向前差分，迎风格式用上游网格来求解下一网格，下游的污染物不会影响上游，符合物理规律。
### 中心差分格式

$$\frac{\partial u}{\partial x} \approx \frac{u_{i+1} - u_{i-1}}{2\Delta x}$$
中心差分，虽然是二阶精度，但是要下游的网格值，相当于“下游的信息提前影响了上游”，导致数值误差被不断放大，最终出现振荡。

### 得到一个允许我们推进时间的方程

$$u_i^{n+1} = u_i^n - c \frac{\Delta t}{\Delta x} (u_i^n - u_{i-1}^n)$$

## 3、代码实现
原教程用了嵌套的for循环实现求解过程，我使用后期的NumPy向量化操作来求解。

核心计算 完整代码请看我的GitHub仓库

    for n in range(nt):

        un = u.copy()  
    
        u[1:] = un[1:] - c * dt / dx * (un[1:] - un[:-1])
    
        u[0] = 1.0

使用切片来推进计算，u[1:] 切片代表了第一步到最后一步

un[:-1] 切片代表了左边界到倒数第二个网格

u[0] = 1.0 为左边界的边界条件

## 4、CFL 稳定性条件

$$\text{CFL} = c \cdot \frac{\Delta t}{\Delta x} \leq 1$$
一般认为 $C \le 1$：计算是稳定的，能够得到合理的物理解。

为了知道CFL的影响，测试了3组
- CFL=0.5（安全范围）：波形完美平移，无振荡、无发散，结果符合物理规律；
- CFL=0.9（接近）：波形基本稳定，有轻微的数值耗散（浓度峰略有变矮），但结果仍有效；
- CFL=1.5（超过）：计算快速发散，数值无意义，无法得到有效结果。
## 5、核心结果解读




> [!TIP]
> 课程来源：CFD Python: 12 Steps to Navier-Stokes，Lorena A. Barba 教授.
