# Day 25 Graph Neural Network

# 그래프 신경망

## 그래프 신경망의 구조

그래프 신경망은 그래프와 정점의 속성 정보를 입력으로 받는다

그래프의 인접 행렬을 $A$라고 할 때, 인접 행렬 $A$는 $|V|\times|V|$의 이진 행렬이다

각 정점 $u$의 속성(Attribute) 벡터를 $X_u$라고 할 때, 벡터$X_u$ 는 $m$차원 벡터이고, $m$은 속성 수를 의미

정점의 속성의 예시는 다음과 같다

- 온라인 소셜 네트워크에서 사용자의 지역, 성별, 연령, 프로필 사진 등
- 논문 인용 그래프에서 논문에 사용된 키워드에 대한 원-핫 벡터
- PageRank 등의 정점 중심성, 군집 계수(Clustering Coefficient) 등

그래프 신경망은 이웃 정점들의 정보를 집계하는 과정을 반복하여 임베딩을 얻는다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/_2021-02-27__8.21.38.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/_2021-02-27__8.21.38.png)

- 각 집계 단계를 층(Layer)라고 부르고, 각 층마다 임베딩을 얻는다
- 0번 층, 즉 입력 층의 임베딩으로는 정점의 속성 벡터를 사용한다

대상 정점 마다 집계되는 정보가 상이하다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/_2021-02-27__8.27.37.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/_2021-02-27__8.27.37.png)

- 대상 정점 별 집계되는 구조를 계산 그래프(Computation Graph)라고 부른다

서로 다른 대상 정점간에도 **층 별 집계 함수**는 공유한다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled.png)

서로 다른 구조의 계산 그래프를 처리하기 위해서는(입력의 갯수가 다름) 어떤 형태의 집계함 수가 필요할까?

집계 함수는 (1) 이웃들 정보의 평균을 계산하고, (2) 신경망에 적용하는 단계를 거친다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%201.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%201.png)

구체적인 수식은 다음과 같다 

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%202.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%202.png)

마지막 층에서의 정점 별 임베딩이 해당 정점의 **출력 임베딩** 이다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%203.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%203.png)

## 그래프 신경망의 학습

그래프 신경망의 학습 변수(Trainable Parameter)는 **층 별 신경망의 가중치**이다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%204.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%204.png)

먼저 손실함수를 결정한다. 정점간 거리를 **보존**하는 것을 목표로 할 수 있다

만약, 인접성을 기반으로 유사도를 정의한다면, 손실 함수는 다음과 같다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%205.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%205.png)

후속 과제(Downsteam Task)의 손실함수를 이용한 종단종(End-to-End) 학습도 가능하다

- 정점 분류가 최종 목표인 경우를 예를 들면,
    1. 그래프 신경망을 이용하여 정점의 임베딩을 얻고
    2. 이를 분류기(Classifier)의 입력으로 사용하여
    3. 각 정점의 유형을 분류하려고 한다
- 이 경우 분류기의 손실함수, 예를 들어 교차 엔트로피(Cross Entropy)를, 전체 프로세스의 손실함수로 사용하여 End-to-End 학습을 할 수있다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%206.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%206.png)

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%207.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%207.png)

- 그래프 신경망의 End-to-End 학습을 통한 분류는, 변환적 정점 임베딩 이후에 별도의 분류기를 학습하는 것보다 정확도가 대체로 높다

그래프 신경망은 비지도 학습, 지도 학습이 모두 가능하다

- 비지도 학습에서는 정점간 거리를 "보존"하는 것을 목표로 한다
- 지도 학습에서는 후속 과제의 손실함수를 이용해 End-to-End 학습을 한다

손실함수를 정의하고 난 뒤, 학습에 사용할 대상 정점을 결정하여 학습 데이터를 구성한다

- 학습에 모든 대상 정점을 사용할 필요는 없다. 일부를 사용해서 학습해도 된다
    - 층 별 집계 함수를 공유 하기 때문

마지막으로 오차역전파(Backpropagation)을 통해 손실함수를 최소화 한다

- 구체적으로, 오차역전파를 통해 신경망의 학습 변수들을 학습한다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%208.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%208.png)

학습된 신경망을 적용하여, 학습에 사용되지 않은 정점의 임베딩을 얻을 수 있다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%209.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%209.png)

마찬가지로, 학습 이후에 추가된 정점의 임베딩도 얻을 수 있다

- 온라인 소셜네트워크 등 많은 실제 그래프들은 시간에 따라서 변화한다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2010.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2010.png)

학습된 그래프 신경망을, **새로운 그래프에 적용**할 수도 있다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2011.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2011.png)

## 그래프 신경망 변형

소개한 것 이외에도 다양한 형태의 **집계 함수**를 사용할 수 있다

그래프 합성곡 신경망(Graph Convolutional Network, GCN)의 집계 함수

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2012.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2012.png)

기존의 집계 함수와 비교

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2013.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2013.png)

- v또한 W신경망을 이용해 함께 평균내서 사용함
- 정규화에서 u와 v의 연결성의 기하평균을 이용함

GraphSAGE의 집계 함수

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2014.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2014.png)

- 이웃들의 임베딩을 AGG 함수를 이용해 합친다
- 자신의 임베딩과 연결(Concatenation)한다

AGG 함수로는 평균, 풀링, LSTM 등이 사용될 수 있다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2015.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2015.png)

- $\pi$는 이웃들의 임베딩을 가져와 순서를 섞는 것

## 합성곱 신경망과의 비교

합성곱 신경망과 그래프 신경망은 모두 이웃의 정보를 집계하는 과정을 반복한다

합성곱 신경망에서는 이웃의 수가 균일하지만, 그래프 신경망에서는 정점 별로 집계하는 이웃의 수가 다르다

그래프의 인접 행렬에 합성곡 신경망을 적용하면 효과적일까?  정답은 아니다!

그래프에는 그래프 신경망을 적용하여야 한다

- 합성곱 신경망이 주로 쓰이는 이미지에서는 인접 픽셀이 유용한 정보를 담고 있을 가능성이 높다
- 하지만, 그래프의 인접 행렬에서의 인접 원소는 제한된 정보를 가진다
- 특히, 인접 행렬의 행과 열의 순서는 임의로 결정되는 경우가 많기 때문이다

## 그래프 어텐션 신경망

기본 그래프 신경망에서는 이웃들의 정보를 동일한 가중치로 평균을 낸다

그래프 합성곱 신경망 역시 단순히 연결성을 고려한 가중치로 평균을 낸다

그래프 어텐션 신경망(Graph Attention Network, GAT)에서는 **가중치 자체도 학습**한다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2016.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2016.png)

- 실제 그래프에서는 이웃 별로 미치는 영향이 다를 수 있기 때문
- 가중치를 학습하기 위해서 셀프-어텐션(Self-Attention)이 사용된다

각 층에서 정점 $i$로부터 이웃$$j$$로의 가중치 $\alpha _{ij}$는 세 단계를 통해 계산한다

1. 해당 층의 정점 $i$의 임베딩 $\mathbf{h}_i$에 신경망 $W$를 곱해 새로운 임베딩을 얻는다

    $\tilde{\mathbf{h}}_i = \mathbf{h}_i\mathbf{W}$

2. 정점 $i$와 정점 $j$의 새로운 임베딩을 연결한 후, 어텐션 계수 $a$를 내적한다

    어텐션 계수 $a$는 모든 정점이 공유하는 학습 변수이다

    $e_{ij} = a^\top[\mathrm{CONCAT}(\tilde{\mathbf{h}}_i,\tilde{\mathbf{h}}_j)]$

3. 2.의 결과에 소프트맥스(Softmax)를 적용한다

    $\alpha_{ij} = \mathrm{softmax}_j(e_{ij}) = \frac{\mathrm{exp}(e_{ij})}{\sum_{k\in\mathcal{N}_i}exp(e_{ik})}$

- 학습 변수는 $W$와 $a$ 이다

여러 개의 어텐션을 동시에 학습한 뒤, 결과를 연결하여 사용한다

- 멀티헤드 어텐션(Multi-head Attention)이라고 한다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2017.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2017.png)

## 그래프 표현 학습과 그래프 풀링

그래프 표현학습, 혹은 그래프 임베딩이란 **그래프 전체를 벡터의 형태로 표현**하는 것

- 개별 정점을 벡터의 형태로 표현하는 정점 표현하는 정점 표현 학습과 구분 됨
- 그래프 임베딩은 벡터의 형태로 표현된 그래프 자체를 의미하기도 한다
- 그래프 임베딩은 그래프 분류 등에 활용된다
    - 예시) 그래프 형태로 표현된 화합물의 분자 구조로부터 특성을 예측하는 것

그래프 풀링(Graph Pooling)이란 **정점 임베딩들로부터 그래프 임베딩을 얻는 과정**

- 평균 등 단순한 방법보다 그래프의 구조를 고려한 방법을 사용할 경우 그래프 분류 등의 후속 과제에서 더 높은 성능을 얻는 것으로 알려져 있다
- 아래 그림의 미분가능한 풀링(Differentiable Pooling, DiffPool)은 군집 구조를 활용하여 임베딩을 계층적으로 집계한다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2018.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2018.png)

1. 개별 정점의 임베딩을 얻는데에 사용된다
2. 군집구조로 묶어주는데에(군집을 찾는) 사용된다
3. 군집내에서 합산 하는데에 사용된다

## 지나친 획일화 문제

지나친 획일화(Over-smoothing) 문제란 그래프 신경망의 층의 수가 증가하면서 정점의 임베딩이 서로 유사해지는 현상을 의미한다

- 지나친 획일화 문제는 작은 세상 효과와 관련이 있다
- 적은 수의 층으로도 다수의 정점에 의해 영향을 받게 된다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2019.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2019.png)

지나친 획일화의 결과로 그래프 신경망의 층의 수를 늘렸을 때, 후속 과제에서의 정확도가 간소하는 현상이 발견됨

- 그래프 신경망의 층이 2개 혹은 3개 일 때 정확도가 가장 높았다
- 잔차항(Residual)을 넣는 것, 즉 이전 층의 임베딩을 한 번 더 더해주는 방법이 있지만 효과는 제한적이다

    $\mathbf{h}^{(l+1)}_u = \mathbf{h}_u^{(l+1)} + \mathbf{h}_u^{(l)}$

획일화 문제에 대한 대응으로 JK 네트워크(Jumping Knowledge Network)는 마지막 층의 임베딩 뿐 아니라, 모든 층의 임베딩을 함께 사용한다

APPNP는 0번째 층을 제외하고는 신경망 없이 집계 함수를 단순화하였다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/_2021-02-27__9.55.26.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/_2021-02-27__9.55.26.png)

- APPNP의 경우, 층의 수 증가에 따른 정확도 감소 효과가 없는 것을 확인됐다

## 그래프 데이터의 증강

데이터 증강(Data Augmentation)은 다양한 기계학습 문제에서 효과적이다

그래프에도 누락되거나 부정확한 간선이 있을 수 있고, 데이터 증강을 통해 보완할 수 있다

임의 보행을 통해 정점간 유사도를 계산하고, 유사도가 높은 정점 간의 간선을 추가하는 방법이 제안되었다

![Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2020.png](Day%2025%20Graph%20Neural%20Network%200f81608fd9b04ae0a85a0944c2c7387e/Untitled%2020.png)

- 그래프 데이터 증강의 결과, 정점 분류의 정확도가 개선 되었다

# Appendix

Graph에 관한 책

- Mining of Massive Datasets
- Networoks Crowds and Markets
    - 두 책 모두 인터넷에 공개되어 있다

Graph 관련 강의

- CS224W: Machine Learning with Graphs
- CS246: Mining Massive Data Sets
    - 유투브에 공개되어 있다