---
title: "風速実験の日常"
date: 2026-04-20T12:00:00+08:00
lastmod: 2026-04-21T15:00:00+08:00
draft: false  
summary: "風洞実験の日常記録、風速計の校正作業について"
tags: ["風洞実験", "風速計", "校正"]
categories: ["実験"]

featureimage: "huji.JPEG"        
featureimagecaption: "風洞実験の日常"
showHero: true                      
heroStyle: "basic"                     
showTableOfContents: true          
showReadingTime: true                 
showWordCount: true               
showComments: true             
showBreadcrumbs: true               
# 补充了系列名称，否则series_order参数不会生效
series: ["風速実験シリーズ"]
series_order: 1                       
externalLinkForceNewTab: true         
sharingLinks: ["twitter", "facebook"] 
---

{{< katex >}}

# 風洞実験の流れ

## 第一日　実験の下準備
### 朝　
風速計端子は中継ボックスに挿入し、センサー端は新しいテープを交換した。
使用したのは熱タイプの風速計であり、一定温度を維持する原理に基づいている。
気流によって温度が変化するため、その温度を一定に保つために必要な電圧を測定することで、風速を算出する。

{{< figure 
  src="sensor_block.jpg"
  alt="風速計をボックスに入れる"
  caption="風速計をボックスに入れる"
>}}

### 昼
先ず、送風機の回転数(60、90、120、150、200、250 RPM)に対して、ピトー管[^ピトー管]を用いて風速を測定した。\
ベルヌーイの定理より、

$$
\Delta p + \frac{1}{2}\rho v^2 = \text{定数}
$$
が成り立ち、風速は次式で求められる:
$$
v = \sqrt{\frac{2\Delta p}{\rho}}
$$
これにより、風速と送風機回転数(RPM)との関係式
$$
v_{\text{ref}} = f(\text{RPM})
$$
を得た。\
更に、得られた風速をMass Flow Controllerを用いて、多点風速計校正プログラムというソフトに入力した。各測定点において一分間の平均電圧(18点)を取得し、風速と電圧の対応関係を求めた。その結果、精度の高い校正曲線を作成した。

{{< figure 
  src="pilot.jpg"
  alt="ピトー管"
  caption="ピトー管"
>}}

## 第二日　風洞内の設置

### 朝
風速計(1~16番)を板に順番に取り付け、板から垂直距離を6mmに設定した。その後、板をターンテーブルに設置した。

{{< figure 
  src="ita.jpg"
  alt="風速計を取り付ける"
  caption="風速計を取り付ける"
>}}

### 昼
風洞内の境界層を形成するため、ソー、スパイヤ（三基）、およびラフネス（大13列、中25列、小9列）を設置した。
これにより、べき指数0.25（=1/4）の平均風速分布を再現した。

{{< figure 
  src="wind_tunnel.jpg"
  alt="設置した様子"
  caption="設置した様子"
>}}

## 第三日　実験開始

### 朝

先ず、ピトー管を用いて今日の基準風速を測定した。その後、実験用ソフトに第一日に作成した校正曲線を読み込み、測量を開始した。


[^ピトー管]: 全圧と静圧の差から動圧を求め、ベルヌーイの定理に基づいて流速を算出する測定装置である。