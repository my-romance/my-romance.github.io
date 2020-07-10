---
layout: post
title:  "highway network와 residual network"
date:   2020-07-10
author: Romance
categories: NLP
---
딥러닝 모델은 모델이 깊을수록, 즉 레이어가 많을수록 학습이 잘 되지 않는 경향 (Vanishing Gradient Problem) → 입력값이 바로 통과될 수 있는 우회 경로(입력값이 특정 레이어를 건너뛰고 다음 레이어를 갈 수 있는 지름길을 만들 수 있음)를 만든 네트워크 → highway network, residual network

### highway network

$$y = H(x,W_H) \cdot T(x,W_T)+x \cdot C(x,W_C) \\=H(x,W_H) \cdot T(x,W_T)+x \cdot (1-T(x,W_T))$$

![](https://3.bp.blogspot.com/-i2bMTiZSp94/XSl8N_vkkhI/AAAAAAAAAKA/tltYBvNELbs8Lnr3K7lYr8K9iVNq5p3gwCK4BGAYYCw/s640/Screen%2BShot%2B2019-07-13%2Bat%2B3.37.28%2BPM.png)
기존 피드포워드 뉴럴 네트워크(수식 : $y=H(x,W_H)$ )에서 highway network는 두개의 게이트를 추가. 이때, Srivastava et al.(2015)은 $C = 1-T$로 설정.

- **게이트 1 : $T$. 입력값을 얼마나 변형할지 결정.** 
$C=1-T$이고 $T=1$이면, 네트워크에 의해 변형된 값이 아웃풋 값(벡터)으로 나옴.
- **게이트 2 : $C$. 입력 벡터 $x$를 얼마나 변형하지 않을지를 결정** 
$C=1-T$이고 $T=O$(즉, $C=1$)이면, 입력 벡터 $x$는 어떠한 변형없이 아웃풋 값(벡터)으로 나옴.

### residual network

$$H(x) = F(x)+x$$

![](https://neurohive.io/wp-content/uploads/2019/01/resnet-e1548261477164.png)

- $F(x)$ : weight layer를 거친 벡터
- $x$ : 입력 벡터

### 출처 :

- 한국어 임베딩 책
- higtway network

    https://jyunleee.blogspot.com/2019/07/highway-network.html?m=1

- residual network

    https://neurohive.io/en/popular-networks/resnet/
