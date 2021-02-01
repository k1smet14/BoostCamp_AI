# Day 11 Deep Learning Basic

# 베이즈 통계학 맛보기

## 베이즈 정리

- 베이즈 정리는 조건부확률을 이용하여 정보를 갱신하는 방법을 알려줌

![Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled.png](Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled.png)

![Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%201.png](Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%201.png)

## 베이즈 정리: 예제

- COVID-99의 발병률이 10%
- 실제로 걸렸을 때 검진될 확률은 99%
- 실제로 걸리지 않았을 때 오검진될 확률이 1%
- 이 때, 질병에 걸렸다고 검진결과가 나왔을 때 정말로 감염되었을 확률은?

![Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%202.png](Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%202.png)

## 조건부 확률의 시각화

![Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%203.png](Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%203.png)

## 베이즈 정리를 통한 정보의 갱신

- 베이즈 정리를 통해 새로운 데이터가 들어왔을 때 앞서 계산한 사후확률을 사전확률로 사용하여 **갱신된 사후확률을 계산**할 수 있다
- 앞서 COVID-99 판정을 받은 사람이 두 번째 검진을 받았을 때도 양성이 나왔을 때 진짜 걸렸을 확률은?

![Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%204.png](Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%204.png)

## 조건부 확률 → 인과관계?

- 조건부 확률은 유용한 통계적 해석을 제공하지만 **인과관계(causality)**를 추론할 때 함부로 사용해서는 안된다.
- 인과관계는 데이터 분포의 변화에 강건한 예측모형을 만들 때 필요
- 인과관계를 알아내기 위해서는 **중첩요인(confounding factor)의 효과를 제거**하고 원인에 해당하는 변수만의 인과관계를 계산해야 함
- 인과관계 추론: 예제) 지능지수와 키의 상관관계
    - 키와 지능지수의 관계를 데이터 분석 했을 때, 관계가 없어 보이지만, 실제로는 키가 클수록 지능지수가 높다는 분석 결과가 나온다.
    - 나이 라는 중첩효과를 제거하지 않았기 때문
    - 나이가 들수록 키가 크고 지능지수가 높아진다는 식의 데이터 분석이 나옴

## 인과관계 추론: 예제

![Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%205.png](Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%205.png)

- a, b라는 치료법에 따른 완치율 분석
- 전체적으로 치료법 b가 완치율이 높으나, 각각의 환자군을 봤을 때 a의 완치율이 높게 나옴
- 조건부확률만 계산해서는 안되고 신장 결석 크기에 따른 중첩요인의 효과를 제거해야 함
- 치료법 a를 선택 했을 때의 완치율 :

![Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%206.png](Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%206.png)

- 치료법 b를 선택 했을 때의 완치율:

![Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%207.png](Day%2011%20Deep%20Learning%20Basic%20ec356cd891234ee4b161214f55d69c79/Untitled%207.png)

# Historical Review

## Key Components of Deep Learning

- The **data** that the model can learn from
- The **model** how to transform the data
- The **loss** function that quantifies the badness of the model
- The **algorithm** to adjust the parameters to minimize the loss

## Historical Review

- 2012 - AlexNet
- 2013 - DQN
- 2014 - Encoder / Decoder, Adam
- 2015 - Generative Adversarial Network, Residual Networks
- 2017 - Transformer
- 2018 - BERT (fine-tuned NLP models)
- 2019 - BIG Language Models
- 2020 - Self Supervised Learning

# Multi-Layer Perceptron

## Neural Networks

- 시작은 인간의 뇌를 모방했지만, 지금은 뇌의 작용과는 많이 달라져 인간의 뇌를 모방했기에 학습이 잘된다는 맞지 않는다는 느낌

## Beyond Linear Neural Networks

- 맵핑이 표현할 수 있는 표현력을 극대화 하기 위해 선형결합을 반복하는 것이 아니라 선형결합 후 activatino function(non linear function)을 사용해서 변환 시키는 것을 반복한다
- 히든레이어가 하나 있는 뉴럴 네트워크의 표현력은 일반적으로 우리가 생각 할 수 있는 대부분의 continuous 함수들을 포함한다
- 그러나 표현력의 잠재성은 높지만 그것을 아는 방법은 어렵다

## loss function

- loss function이 어떤 성질을 가지고 있고, 이것이 왜 내가 원하는 결과를 얻어낼 수 있는지를 생각하고 사용 해야 한다
- Regression Task → MSE = $\frac{1}{N}\Sigma_{i=1}^{N}\Sigma_{d=1}^{D}(y_i^{(d)}-\hat{y}_i^{(d)})^2$
- Classification Task → CE = $-\frac{1}{N}\Sigma_{i=1}^N\Sigma_{d=1}^Dy_i^{(d)}\log\hat{y}_i^{(d)}$
- Probabilistic Task → MLE = $\frac{1}{N}\Sigma_{i=1}^N\Sigma_{d=1}^D\log\mathcal{N}(y_i^{(d)};\hat{y}_i^{(d)}, 1)$

# Pytorch

- 환경설정 설치! - 필요시 강의 다시 보자
- Numpy + AutoGrad + Function
- Pytorch 자료 : 파이토치 튜토리얼, PyTorch로 시작하는 딥 러닝 입문