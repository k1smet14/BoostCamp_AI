# Day 24 Node Embedding, Recommendation system

# 정점 표현 학습

- 정점 표현 학습이란 그래프의 정점들을 벡터의 형태로 표현하는 것
- 간단히 정점 임베딩(Node Embedding)라고 한다
- 정점이 표현되는 벡터 공간을 임베딩 공간이라 부른다
- 주어진 입력 그래프의 각 정점 $u$에 대한 임베딩, 즉 벡터 표현 $z_u$가 정점 임베딩의 출력
- 정점 임베딩의 결과로, 벡터 형태의 데이터를 위한 도구들을 그래프에도 적용 할 수 있다
    - 기계학습 도구들이 한가지 예, 벡터 형태로 표현된 사례(Instance)들을 입력으로 받는다
    - 최신 기계 학습도구들을 정점 분류(Node Classification), 군집 분석(Community Detection)등에 활용할 수 있다
- 그래프에서의 정점간 유사도를 임베딩 공간에서도 **보존**하는 것을 목표로 한다
- 임베딩 공간에서의 유사도로는 내적(Inner Product)를 사용
- $u$와 $v$의 유사도는 둘의 임베딩 내적 $z_v^{\top}z_u = \lVert z_u\rVert\cdot \lVert z_v\rVert \cdot cos(\theta)$이다
- $\mathrm{similarity}(u, v)\approx z_v^{\top}z_u$
- 정점 임베딩은 다음 두 단계로 이루어짐
    1. 그래프에서의 정점 유사도를 정의하는 단계
    2. 정의한 유사도를 보존하도록 정점 임베딩을 학습하는 단계

# 인접성 기반 접근법

- 인접성(Adjacency) 기반 접근법에서는 두 정점이 인접할 때 유사하다고 간주
- 두 정점 $u$와 $v$가 인접하다는 것은 둘을 직접 연결하는 간선 $(u,v)$가 있음을 의미
- 인접행렬(Adjacency Matrix) $\mathbf A$의 $u$행 $v$열 원소 $\mathbf{A}_{u,v}$는 $u$와 $v$가 인접한 경우 1, 아닌 경우 0

![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled.png)

- 인접성 기반 접근법의 손실 함수(Loss Function)은 아래와 같다

![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%201.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%201.png)

- 손실 함수가 최소가 되는 정점 임베딩을 찾는 것을 목표
    - 손실 함수 최소화를 위해서는 (확률적) 경사하강법 등이 사용됨
- 인접성만으로 유사도를 판단하는 것은 한계가 존재
    - 거리에 대한 고려가 없다
    - 군집 관점에서의 고려가 없다

# 거리/경로/중첩 기반 접근법

- 거리 기반 접근법
    - 두 정점 사이의 거리가 충분히 가까운 경우 유사하다고 간주
    - "충분히"의 기준을 2라고 가정하면 거리가 2 이하인 정점들은 유사도가 1이고, 그 이상은 0이다
- 경로 기반 접근법
    - 두 정점 사이의 경로가 많을 수록 유사하다고 간주
    - 두 정점 사이의 경로 중 거리가 $k$인 것은 $\mathbf{A}^k_{u,v}$와 같다
        - 즉, 인접행렬 $A$의 $k$ 제곱의 $u$행 $v$열 원소와 같다
    - 경로 기반 접근법의 손실 함수는 다음과 같다

    ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%202.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%202.png)

- 중첩 기반 접근법
    - 두 정점이 많은 이웃을 공유할 수록 유사하다고 간주
    - 정점 u의 이웃 집합을 N(u), 정점 v의 이웃 집합을 N(v)라고 하면, 두 정점의 공통 이웃 수 S_{u,v}는 다음과 같이 정의 된다

        ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%203.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%203.png)

    - 중첩 기반 접근법의 손실 함수는 다음과 같다

        ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%204.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%204.png)

    - 공통 이웃 수 대신 자카드 유사도 혹은 Adamic Adar 점수를 사용할 수도 있다
        - 자카드 유사도(Jaccard Similarity)는 공통 이웃의 수 대신 비율을 계산하는 방식

            ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%205.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%205.png)

            - 0~1사이의 값을 가짐, 공통 이웃이 같을 때 1
        - Adamic Adar 점수는 공통 이웃 각각에 가중치를 부여하여 가중합을 계산하는 방식

            ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%206.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%206.png)

            - 연결성으로 나누어 주는 이유 : 연결성이 많은 공통 이웃을 가질 경우 유사도가 높다고 할수 없다

# 임의보행 기반 접근법

- 한 정점에서 시작하여 임의보행을 할 때 다른 정점에 도달할 확률을 유사도로 간주
- 임의보행을 사용할 경우 시작 정점 주변의 지역적 정보와 그래프 전역 정보를 모두 고려한다는 장점
- 임의보행 기반 접근법은 세 단계를 거친다
    1. 각 정점에서 시작하여 임의보행을 반복 수행한다
    2. 각 정점에서 시작한 임의보행 중 도달한 정점들의 리스트를 구성한다
        - 이 때, 정점 $u$에서 시작한 임의보행 중 도달한 정점들의 리스트를 $N_R(u)$라고 하자
        - 한 정점을 여러 번 도달한 경우, 해당 정점은 $N_R(u)$에 여러 번 포함될 수 있다
    3. 다음 손실함수를 최소화하는 임베딩을 학습한다

        ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%207.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%207.png)

    - 정점 $u$에서 시작한 임의보행이 정점 $v$에 도달할 확률 $P(v|z_u)$을 다음과 같이 추정

        ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%208.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%208.png)

    - 즉 유사도 $z_v^{\top}z_u$가 높을 수록 도달 확률이 높다
    - 추정한 도달 확률을 사용하여 손실함수를 완성하고 이를 최소화하는 임베딩을 학습한다

    ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%209.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%209.png)

- 임의보행 방법에 따라 DeepWalk와 Node2Vec이 구분 된다
- DeepWalk는 앞서 설명한 기본적인 임의보행을 사용함
    - 현재 정점의 이웃 중 하나를 균일한 확률로 선택하여 이동하는 과정을 반복
- Node2Vec은 2차 치우친 임의보행(Second-order Biased Random Walk)을 사용
    - 현재 정점과 직전에 머물렀던 정점을 모두 고려하여 다음 정점을 선택
    - 직전 정점의 거리를 기준으로 경우를 구분하여 차등적인 확률을 부여

        ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2010.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2010.png)

    - Node2Vec에서는 부여하는 확률에 따라서 다른 종류의 임베딩을 얻는다
    - 아래 그림은 Node2Vec으로 임베딩을 수행한 뒤, K-means 군집 분석을 수행한 결과

    ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2011.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2011.png)

    - 멀어지는 방향에 높은 확률을 부여한 경우, 정점의 역할(다리 역할, 변두리 정점 등)이 같은 경우, 임베딩이 유사하다
    - 가까워지는 방향에 높은 확률을 부여한 경우, 같은 군집(Community)에 속한 경우, 임베딩이 유사하다
- 손실 함수 근사
    - 임의보행 기법의 손실함수는 계산에 정점의 수의 제곱에 비례하는 시간이 소요된다
    - 따라서 많은 경우 근사식을 사용한다
    - 모든 정점에 대해서 정규화하는 대신 몇 개의 정점을 뽑아서 비교하는 형태
    - 이 때 뽑힌 정점들을 네거티브 샘플이라고 부른다
    - 연결성에 비례하는 확률로 네거티브 샘플을 뽑으며, 네거티브 샘플이 많을 수록 학습이 더욱 안정적이다

        ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2012.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2012.png)

# 변환식 정점 표현 학습의 한계

- 지금까지 소개한 정점 임베딩 방법들을 변환식(Transductive) 방법이다
- 변환식 방법은 학습의 결과로 정점의 임베딩 자체를 얻는다는 특성이 있다
- 정점을 임베딩으로 변화시키는 함수, 즉 인코더를 얻는 귀납식(Inductive)방법과 대조됨
- 변환식 임베딩 방법의 한계
    1. 학습이 진행된 이후에 추가된 정점에 대해서는 임베딩을 얻을 수 없다
    2. 모든 정점에 대한 임베딩을 미리 계산하여 저장해두어야 한다
    3. 정점이 속성(Attribute) 정보를 가진 경우에 이를 확용할 수 없다
- 이런 단점을 극복한 대표적인 귀납식 임베딩 방법이 그래프 신경망(Graph Neural Network)이다

---

 

# 그래프를 추천시스템에 어떻게 활용할까? (심화)

- 넷플릭스 챌린지는 사용자별 영화 평점 데이터를 활용하여 추천시스템의 성능을 향상 시키는 것

## 잠재 인수 모형

- 잠재 인수 모형(Latent Factor Model)의 핵심은 사용자와 상품을 벡터로 표현하는 것
    - UV decomposition, SVD 로 불리기도 한다
- 잠재 인수 모형에서는 고정된 인수 대신 **효과적인 인수를 학습**하는 것을 목표로 한다
    - 학습한 인수를 잠재 인수(Latent Factor)라고 부른다
    - 추천을 가장 정확하게 할수 있는 차원으로 줄여서 그 차원으로 구성된 임베딩 공간에 배치한다
    - 임베딩 공간의 축들을 잠재 인수라고 부른다
- 사용자와 상품의 임베딩의 내적(Inner Product)이 평점과 최대한 유사하도록 하는 것
    - 사용자 $x$의 임베딩을 $p_x$, 상품 $i$의 임베딩을 $q_i$, 사용자 $x$의 상품 $i$에 대한 평점을 $r_{xi}$라고 할 때,
    - 임베딩의 목표는 $p_x^{\top}q_i$이 $r_{xi}$와 유사하도록 하는 것
- 행렬 차원에서 살펴보면 다음과 같다

![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2013.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2013.png)

- 잠재 인수 모형은 다음 손실 함수를 최소화하는 $P$와 $Q$를 찾는 것

![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2014.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2014.png)

- 하지만 위 손실 함수는 과적합(Overfitting)이 발생할 수 있다
    - 과적합이란 기계학습 모형이 훈련 데이터의 잡음(Noise)까지 학습하여, 평가 성능이 감소하는 현상
- 과적합을 방지하기 위해 정규화 항을 손실 함수에 더해준다

![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2015.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2015.png)

- 정규화는 극단적인, 즉 절댓값이 너무 큰 임베딩을 방지하는 효과가 있다
- 손실함수를 최소화하는 $P$와 $Q$를 찾기 위해서는 (확률적)경사하강법을 사용한다

![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2016.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2016.png)

- 경사하강법은 손실함수를 안정적으로 하지만 느리게 감소시킨다
- 확률적 경사하강법은 손실함수를 불안정하지만 빠르게 감소시킨다
- 실제로는 확률적 경사하강법이 더 많이 사용됨

## 고급 잠재 인수 모형

### 사용자와 상품의 편향을 고려한 잠재 인수 모형

- 각 사용자의 편향은 **해당 사용자의 평점 평균과 전체 평점 평균의 차** 이다
- 각 상품의 편향은 **해당 상품에 대한 평점 평균과 전체 평점 평균의 차** 이다
- 개선된 잠재 인수 모형에서는 평점을 전체 평균, 사용자 편향, 상품 편향, 상호작용으로 분리한다

    ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2017.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2017.png)

- 개선된 잠재 인수 모형의 손실 함수는 아래와 같다

    ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2018.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2018.png)

- 확률적 경사하강법을 통해 손실 함수를 최소화하는 잠재 인수와 편향을 찾아낸다

### 시간적 편향을 고려한 잠재 인수 모형

- 넷플릭스 시스템의 변화로 평균 평점이 크게 상승하는 사건이 있었다
- 영화의 평점은 출시일 이후 시간이 지남에 따라 상승하는 경향을 갖는다
- 개선된 잠재 인수 모형에서는 이러한 시간적 편향을 고려한다
    - 구체적으로 사용자 편향과 상품 편향을 시간에 따른 함수로 가정한다

        ![Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2019.png](Day%2024%20Node%20Embedding,%20Recommendation%20system%20949b1c4b29964539a6e64d6589269d18/Untitled%2019.png)

### 넷플릭스 챌린지의 결과

- 수많은 알고리즘을 앙상블(Ensemble)하는 방법으로 우승했다