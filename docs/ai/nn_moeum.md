---
layout: default
title: 딥러닝 정리
parent: AI
nav_order: 3
---

# 딥러닝 용어 모음
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

신경망, 순전파, 역전파, 최적화

## 신경망 학습 단계

신경망 : input에 대한 중첩된 함수 모음<br>

-> 신경망 학습 2단계

1. **순전파** <br>
   input을 함수에 넣어 신경망을 거치며 정답을 추측하는 단계  
2. **역전파** <br>
   추측한 값의 오류에 비례해 매개변수를 적절히 조정하는 단계
   출력에서 시작하여 신경망을 거꾸로 거치며, 오류에 대한 함수들의 미분값 수정
   보통 경사하강법으로 매개변수를 최적화한다.

## 최적화 (optimizer)
올바른 파라미터를 찾아가는 과정

**매개변수 (parameter)** : 각 노드의 값 / 모델 내부에서 볼 수 있는 변수, 모델의 능력을 결정하고 측정 또는 학습된다. -> 가중치<br>
**hyper-parameter** : 매개변수 측정을 위해 알고리즘 구현 과정에 사용. 경험적으로 산출됨. -> learning rate 등<br>

**loss** : output이 정답과 얼마나 떨어져 있는가 -> MSE 등


## 예시 - 숫자 이미지 분류

과정
1. 신경망 정의
2. data set 입력 반복
3. input을 신경망에 전파
4. loss 계산
5. 변화도(gradient)를 신경망 매개변수들에 역전파
6. 가중치 갱신 : New Weight = Weight - (learning rate*gradient)

📌 torch에서 forward()를 구현하면 backward()는 autograd에 의해 자동 정의됨!<br>
![image](https://github.com/cheongsan16/cheongsan16.github.io/assets/57765638/dea8027e-39ec-472c-a2e7-a90a6f0eaca2)

위와 같은 신경망 구축.

conv -> pooling -> conv -> pooling -> flatten -> linear -> relu -> linear -> relu -> linear<br>
❔ flatten 이후 linear layer에 input channel이 400개인 이유..? 이전 output이 16개, 이미지 사이즈 왜 5x5? 

### 레이어 종류

**Linear layer** : 선형 변환을 수행하는 layer. n차원의 데이터를 m차원으로 변환<br>
 -> 피처의 범위, 연산량 변화 / 피처↑, 데이터 표현력↑, 연산량↑
 -> 단, 성질은 그대로 : 동일한 연산 수행 시 동일한 결과 출력

**Convolution layer** : <br>
**Pooling layer** : 데이터의 크기를 줄인다. -> 모델 전체의 매개변수 수를 줄일 수 있음!<br>
**Flatten layer** : 평탄화... <br>


### 활성화 함수
가중치 편향 갱신에 사용되는 함수. 가중치가 높으려면 해당 뉴런이 더욱 활성화되어야 함!<br>
-> 선형구조는 불가능. why? 기울기가 일정하여 미분값이 항상 상수이기 때문<br>
-> 입력을 정규화하는 과정

- 종류
  sigmoid :<br>
  relu :<br>
  selu :<br>
  tanh :<br>




