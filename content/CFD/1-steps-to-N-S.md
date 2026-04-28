---
title: "一次元線形移流方程式"
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

series: ["12 steps to Navier-Stokes"]
series_order: 1                       
externalLinkForceNewTab: true         
sharingLinks: ["twitter", "facebook"] 
---

{{< katex >}}


# Step 1 一次元線形移流方程式（1-D Linear Convection Equation）
## 支配方程式

一次元線形移流方程式はCFDの出発点であり、その数学的な形式は以下の通りです。

$$\frac{\partial u}{\partial t} + c \frac{\partial u}{\partial x} = 0$$

CFDの核心にある二つの基本概念：**数値離散の物理的意味**と**数値安定性の本質**。

## 1. 方程式の物理的意味：汚染物質の拡散を例に

自分の研究対象である都市街区の汚染物質拡散と結びつけてこの方程式を理解します。

- $u(x,t)$：ある街路上における汚染物質濃度の、位置 $x$ と時刻 $t$ に対する分布
- $c$：一定の風速。汚染物質が風によって運ばれる速さを表し、方向は $x$ 正方向
- **方程式全体の物理的意味**：汚染物質は一定の風速で風向き（正方向）に沿って全体として移動し、波形の形状は変化しない

これは理想的な「波形の平行移動」を表しています。初期状態で街路の中央に「濃度ピーク」があった場合、時間が経つにつれてそのピークは風向きに沿って等速で移動し、広がったり低くなったり歪んだりすることはありません。これは拡散・損失のない理想的な移流則に完全に従った挙動です。

## 2. 有限差分による離散化：風上差分と中心差分の比較

### 風上差分（Upwind Scheme）

$$\frac{\partial u}{\partial x} \approx \frac{u_i^n - u_{i-1}^n}{\Delta x}$$

前方差分を用いた風上格差分です。風上側（上流）の格子点を使って次の格子点を解くため、下流の汚染物質情報が上流に影響を与えません。物理的な因果関係と整合しています。

### 中心差分（Central Difference Scheme）

$$\frac{\partial u}{\partial x} \approx \frac{u_{i+1} - u_{i-1}}{2\Delta x}$$

中心差分は2次精度ですが、下流側の格子値を必要とします。これは「下流の情報が上流に先に伝わる」ことを意味し、数値誤差が増幅され続け、最終的に数値振動が発生します。

### 時間を進めるための離散式

$$u_i^{n+1} = u_i^n - c \frac{\Delta t}{\Delta x} (u_i^n - u_{i-1}^n)$$

## 3. コード実装

元のチュートリアルではネストしたforループで解法を実装していましたが、ここではNumPyのベクトル化演算を使用しています。

**核心となる計算部分**（完全なコードはGitHubリポジトリを参照）

```python
for n in range(nt):
    un = u.copy()  
    u[1:] = un[1:] - c * dt / dx * (un[1:] - un[:-1])
    u[0] = 1.0
```

スライスを使って計算を進めています。
- `u[1:]`：1番目から最後の格子点までのスライス
- `un[:-1]`：左端境界から後ろから2番目の格子点までのスライス
- `u[0] = 1.0`：左端の境界条件

## 4. CFL安定性条件

$$\text{CFL} = c \cdot \frac{\Delta t}{\Delta x} \leq 1$$

一般に $C \leq 1$ であれば計算は安定し、物理的に合理的な解が得られます。

CFLの影響を確認するため、3ケースをテストしました。

- **CFL = 0.5（安全範囲）**：波形が完全に平行移動し、振動・発散なし。物理的に正しい結果が得られた
- **CFL = 0.9（臨界に近い）**：波形はほぼ安定しているが、わずかな数値拡散が見られる（濃度ピークがやや低くなる）。結果としては依然有効
- **CFL = 1.5（超過）**：計算が急速に発散し、数値的に無意味な結果となる。有効な解は得られない

## 5. 主要な結果の考察



> [!TIP]
> 参考教材：*CFD Python: 12 Steps to Navier-Stokes*、Lorena A. Barba 教授