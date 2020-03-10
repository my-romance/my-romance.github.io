---
layout: post
title:  "장단기 메모리(Long Short-Term Memory, LSTM)"
date:   2020-03-10
author: Romance
categories: ML
---

# 장단기 메모리(Long Short-Term Memory, LSTM)

### 바닐라 RNN의 한계

바닐라 RNN : 가장 단순한 형태의 RNN을 의미 (RNN : 이전의 계산 결과($h_{t-1 }$)와 현재 입력($x_t$)이 출력 결과( $h_{t}$ )에 영향을 주는 sequence 모델)

바닐라 RNN의 단점 : 비교적 짧은 시퀀스에 대해서만 효과를 보임. ← 바닐라 RNN의 시점(time step)이 길어질수록 앞의 정보가 뒤로 충분히 전달되지 못함.이를 **장기 의존성 문제(the problem of Long-Term Dependencies)**라고 함

<img src="/assets/image/vanila_rnn_1.PNG">



### LSTM(Long Short-Term Memory)

**LSTM : 바닐라 RNN의 장기 의존성 문제를 보완**하기 위한 RNN 모델

<img src="/assets/image/lstm_1.PNG">

LSTM의 특징 : **은닉층의 메모리 셀에 input gate, forget gate, output gate**를 추가하여 **불필요한 기억을 지우고, 기억해야할 것들을 정함**

즉, LSTM은 hidden state를 계산하는 식이 더 복잡해졌으며, **셀 상태(cell state)**라는 개념이 추가됨



**LSTM 특징**

- cell state

  cell state는 아래 그림에서 왼쪽에서 오른쪽으로 가는 굵은 선. $C_{t-1}$가 $C_t$를 계산하기 위한 입력으로 사용됨.
  <img src="/assets/image/lstm_2.PNG">



-  3개의 gate 
  3개의 gate에는 공통적으로 시그모이드 함수가 존재 → 0~1의 값을 가지는데 이 값들을 이용해 cell state와 hidden state를 반환



**LSTM의 단계**

1. forget gate layer : **기억을 삭제하기 위한 게이트**

   <img src="/assets/image/lstm_3.PNG">

   $f_t :$ 현재 시점의 입력인 $x_t$와 이전 시점의 hidden state인 $h_{t-1}$가 MLP를 거쳐 시그모이드 함수를 지남 → 0~1의 값을 가짐 →  0에 가까울수록 정보를 많이 삭제하고 1에 가까울수록 정보를 온전히 기억. 

2. input gate layer : **현재 정보를 기억하기 위한 게이트** 
   <img src="/assets/image/lstm_4.PNG">
   $i_t$ : 현재 시점의 입력인 $x_t$와 이전 시점의 hidden state인 $h_{t-1}$가 MLP를 거쳐 시그모이드 함수를 지남 → 0~1의 값을 가짐 → 0에 가까울수록 정보를 많이 삭제하고 1에 가까울수록 정보를 온전히 기억. 
   $\tilde{C}$ : **새로운 cell state의 후보 값 vector.** (새로운 cell state로 업데이트 될 vector)

3. cell state (장기 상태) : cell state update

   <img src="/assets/image/lstm_5.PNG">
   우선 이전 state에 $f_t$를 곱해서 가장 첫 단계에서 잊어버리기로 정했던 것들을 진짜로 잊어버린다. 
   그리고나서 $i_t*\tilde{C}_t$를 더한다. 이 값은 input gate layer 단계에서 업데이트하기로 한 값을 얼마나 업데이트할 지 정한 만큼 scale한 것.

4. output gate layer와 hidden state(단기 상태) : 무엇을 output으로 내보낼지 정함

   <img src="/assets/image/lstm_6.PNG">
   $o_t$ :  $x_t$와 $h_{t-1}$가 MLP와 시그모이드 함수를 거쳐 **cell state의 어느 부분을 output으로 내보낼지 결정**

   $h_t$ : $C_t$가 tanh를 거쳐 -1~1의 값을 받은 뒤, 이전에 계산된 $o_t$와 곱해준다. → **output으로 보내고자 하는 부분만 내보낼 수 있게됨**



### 참조

- https://wikidocs.net/22888
- https://dgkim5360.tistory.com/entry/understanding-long-short-term-memory-lstm-kr
