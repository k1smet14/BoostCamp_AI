# Day 23 Community, Recommendation system

# 군집

- 군집(Community)이란 다음 조건들을 만족하는 정점들의 집합
    1. 집합에 속하는 정점 사이에는 많은 간선이 존재한다
    2. 집합에 속하는 정점과 그렇지 앟은 정점 사이에는 적은 수의 간선이 존재한다
    - 수학적으로 엄밀한 정의는 아님
- 온라인 소셜 네트워크의 군집들은 사회적 무리(Social Circle)을 의미하는 경우가 많다
- 그래프를 여러 군집으로 잘 나누는 문제를 군집 탐색(Community Detection)문제 라고 한다

# 군집 구조의 통계적 유의성과 군집성

- 배치 모형(Configuration Model)
    1. 각 정점의 연결성(Degree)을 보존한 상태에서
    2. 간선들을 무작위로 재배치하여서 얻은 그래프를 의미
    - 배치 모형에서 임의의 두 정점 i와 j 사이의 간선이 존재할 확률은 두 정점의 연결성에 비례

![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled.png)

- 군집성(Modularity)
    - 그래프와 군집들의 집합 S가 주어졌을 때, 각 군집 $s\in S$ 가 군집의 성질을 잘 만족하는 지를 살펴보기 위해, 군집 내부의 간선의 수를 그래프와 배치 모형에서 비교한다
    - 구체적으로, 군집성은 다음 수식으로 계산된다

    ![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%201.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%201.png)

    - 배치 모형과 비교했을 때, 그래프에서 군집 내부 간선의 수가 월등히 많을 수록 성공한 군집 탐색이다
    - 즉, 군집성은 무작위로 연결된 배치 모형과의 비교를 통해 통계적 유의성을 판단한다
    - 군집성은 항상 -1과 +1 사이의 값을 가지며, 보통 0.3~0.7정도의 값을 가질 때, 그래프에 존재하는 통계적으로 유의미한 군집들을 찾아냈다고 할 수 있다

# 군집 탐색 알고리즘

### Girvan-Newman 알고리즘

- Girvan-Newman 알고리즘은 대표적인 하향식(Top-Down) 군집 탐색 알고리즘이다
- 전체 그래프에서 탐색을 시작하고, 군집들이 서로 분리되도록, 간선을 순차적으로 제거
- 서로 다른 군집을 연결하는 다리(Bridge) 역할의 간선을 제거해야 군집들이 분리된다
- 매개 중심성(Betweeness Centrality)
    - 간선의 매개 중심성(Betweeness Centrality)을 사용하여 다리 역할의 간선을 찾을 수 있다
    - 이는 해당 간선이 정점 간의 최단 경로에 놓이는 횟수를 의미
    - 정점 i로 부터 j로의 최단 경로 수를 $\sigma_{i,j}$라고 하고 그 중 간선(x,y)를 포함한 것을 $\sigma_{i,j}(x,y)$라고 할 때, 간선(x,y)의 매개 중심성은 다음 수식으로 계산 됨
    - $\sum_{i<j}\frac{\sigma_{i,j}(x,y)}{\sigma_{i,j}}$
- Girvan-Newman 알고리즘은 매개 중심성이 높은 간선을 순차적으로 제거한다
    - 간선이 제거될 때마다, 매개 중심성을 다시 계산하여 갱신한다
    - 간선이 모두 제거될 때까지 반복한다

![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%202.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%202.png)

- 간선의 제거 정도에 따라서 다른 입도(Granularity)의 군집 구조가 나타난다
- 간선을 어느 정도 제거하는 것이 적합할까?
    - 군집성이 최대가 되는 지점까지 간선을 제거한다
    - 단, 현재의 연결 요소들을 군집으로 가정하되, 입력 그래프에서 군집성을 계산한다

![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%203.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%203.png)

- Girvan-Newman 알고리즘을 정리하면 다음과 같다
    1. 전체 그래프에서 시작한다
    2. 매개 중심성이 높은 순서로 간선을 제거하면서, 군집성의 변화를 기록한다
    3. 군집성이 가장 커지는 상황을 복원한다
    4. 이 때, 서로 연결된 정점들, 즉 연결 요소를 하나의 군집으로 간주한다
    - 즉 전체 그래프에서 시작해서 점점 작은 단위를 검색하는 하향식(Top-Down) 방법

### Louvain 알고리즘

- Louvain 알고리즘은 대표적인 상향식(Bottom-Up) 군집 탐색 알고리즘이다
- 개별 정점에서 시작해서 점점 큰 군집을 형성

![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%204.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%204.png)

- 구체적인 Louvain 알고리즘의 동작 과정은 다음과 같다
    1. Louvain 알고리즘은 개별 정점으로 구성된 크기 1의 군집들로부터 시작한다
    2. 각 정점 u를 기존 혹은 새로운 군집으로 이동한다, 이 때, 군집성이 최대화되도록 군집을 결정한다
    3. 더 이상 군집성이 증가하지 않을 때까지 2.를 반복한다
    4. 각 군집을 하나의 정점으로 하는 군집 레벨의 그래프를 얻은 뒤 3.을 수행한다
    5. 한 개의 정점이 남을 때까지 4.를 반복한다

# 중첩이 있는 군집 탐색

- 실제 그래프의 군집들은 중첨되어 있는 경우가 많다
- Girvan-Newman 알고리즘, Louvain 알고리즘은 군집 간의 중첩이 없다고 가정한다

![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%205.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%205.png)

- 중첩 군집 모형
    1. 각 정점은 여러 개의 군집에 속할 수 있다
    2. 각 군집 A에 대하여, 같은 군집에 속하는 두 정점은 $P_A$ 확률로 간선으로 직접 연결된다
    3. 두 정점이 여러 군집에 동시에 속할 경우 간선 연결 확률은 독립적이다
        - 예를 들어, 두 정점이 군집 A와 B에 동시에 속할 경우, 두 정점이 간선으로 직접 연결될 확률은      $1-(1-P_A)(1-P_B)$이다
    4. 어느 군집에도 함께 속하지 않는 두 정점은 낮은 확률 $\epsilon$으로 직접 연결된다
- 중첩 군집 모형이 주어지면, 주어진 그래프의 확률을 계산할 수 있다.
    - 그래프의 확률은 다음 확률들의 곱이다
        1. 그래프의 각 간선의 두 정점이 (모형에 의해) 직접 연결될 확률
        2. 그래프에서 직접 연결되지 않은 각 정점 쌍이 (모형에 의해) 직접 연결되지 않을 확률

        ![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%206.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%206.png)

- 중첩 군집 탐색은 주어진 그래프의 확률을 최대화하는 중첩 군집 모형을 찾는 과정
    - 최우도 추정치(Maximum Likelihood Estimate)를 찾는 과정이다

    ![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%207.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%207.png)

    - 중첩 군집 탐색을 용이하게 하기 위하여 완화된 중첩 군집 모형을 사용한다
    - 완화된 중첩 군집 모형에서는 각 정점이 각 군집에 속해 있는 정도를 실숫값으로 표현한다
    - 최적화 관점에서는, 모형의 매개변수들이 실수 값을 가지기 때문에, 익숙한 최적화 도구(경사하강법 등)을 사용하여 모형을 탐색할 수 있다는 장점이 있다

# 추천 시스템

- 추천 시스템은 사용자 각각이 구매할 만한 혹은 선호할 만한 상품을 추천하는 것
- 구매 기록이라는 암시적(Implicit)인 선호, 혹은 평점이라는 명시적(Explicit)인 선호가 있는 경우도 있다
- 추천 시스템의 핵심은 사용자별 구매를 예측하거나 선호를 추정하는 것
- 그래프 관점에서 추천 시스템은 "미래의 간선을 예측하는 문제" 혹은 "누락된 간선의 가중치를 추정하는 문제"로 해석할 수 있다

# 내용 기반 추천 시스템

- 내용 기반(Content-based) 추천은 각 사용자가 구매/만족했던 상품과 유사한 것을 추천하는 방법
- 내용 기반 추천은 다음 네 가지 단계로 이루어진다

![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%208.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%208.png)

- 첫 단계는 사용자가 선호했던 상품들의 상품 프로필(Item Profile)을 수집하는 단계
    - 어떤 상품의 상품 프로필이란 해당 상품의 특성을 나열한 벡터
    - 영화의 경우 감독, 장르, 배우 등의 원-핫 인코딩이 상품 프로필이 될 수 있다
- 다음 단계는 사용자 프로필(User Profile)을 구성하는 단계
    - 사용자 프로필은 선호한 상품의 상품 프로필을, 선호도를 사용항여 가중 평균하여 계산한다

![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%209.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%209.png)

- 다음 단계는 사용자 프로필과 다른 상품들의 상품 프로필을 매칭하는 단계
    - 사용자 프로필 벡터 $\vec u$와 상품 프로필 벡터 $\vec v$가 주어졌을 때, 코사인 유사도 $\frac{\vec u\cdot\vec v}{\lVert\vec u\rVert \lVert\vec v\rVert}$를 계산한다
        - 즉, 두 벡터의 사이각의 코사인 값을 계산
    - 코사인 유사도가 높을 수록, 해당 사용자가 과거 선호했던 상품들과 해당 상품이 유사함을 의미
- 마지막 단계는 사용자에게 상품을 추천하는 단계
    - 계싼한 코사인 유사도가 높은 상품들을 추천한다
- 내용 기반 추천시스템의 장점
    1. 다른 사용자의 구매 기록이 필요하지 않다
    2. 독특한 취향의 사용자에게도 추천이 가능하다
    3. 새 상품에 대해서도 추천이 가능하다
    4. 추천의 이유를 제공할 수 있다
- 내용 기반 추천시스템의 단점
    1. 상품에 대한 부가 정보가 없는 경우에는 사용할 수 없다
    2. 구매 기록이 없는 사용자에게는 사용할 수 없다
    3. 과적합(Overfitting)으로 지나치게 협소한 추천을 할 위험이 있다

# 협업 필터링 추천시스템

- 사용자-사용자 협업 필터링은 다음 세 단계로 이루어진다
    1. 추천의 대상 사용자를 $x$라고 할 때, $x$와 유사한 취향의 사용자들을 찾는다
    2. 유사한 취향의 사용자들이 선호한 상품을 찾는다
    3. 이 상품들을 $x$에게 추천한다

![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2010.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2010.png)

- 사용자-사용자 협업 필터링의 핵심은 유사한 취향의 사용자를 찾는것
    - 취향의 유사도는 어떻게 계산할까?
- 취향의 유사성은 상관 계수(Correlation Coefficient)를 통해 측정한다

    ![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2011.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2011.png)

    - 사용자 $x$의 상품 $s$에 대한 평점을 $r_{xs}$
    - 사용자 $x$가 매긴 평균 평점을 $\bar{r_x}$
    - 사용자 $x$와 $y$가 공동 구매한 상품들을 $S_{xy}$
- 구체적으로 취향의 유사도를 가중치로 사용한 평점의 가중 평균을 통해 평점을 추정한다
    - 사용자 $x$의 상품 $s$에 대한 평점을 $r_{xs}$를 추정하는 경우를 생각해보자
    - 상관 계수를 이용하여 상품 $s$를 구매한 사용자 중에 $x$와 취향이 가장 유사한 $k$명의 사용자 $N(x;s)$를 뽑는다
    - 평점 $r_{xs}$는 아래의 수식을 이용해 추정한다

![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2012.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2012.png)

- 추정한 평점이 가장 높은 상품들을 $x$에게 추천한다
- 협업 필터링의 장점
    - 상품에 대한 부가 정보가 없는 경우에도 사용할 수 있다
- 협업 필터링의 단점
    - 충분한 수의 평점 데이터가 누정되어야 효과적
    - 새 상품, 새로운 사용자에 대한 추천이 불가능
    - 독특한 취향의 사용자에게 추천이 어렵다

# 추천 시스템의 평가

- 데이터를 훈련(Training)데이터와 평가(Test)데이터로 분리한다
- 평가 데이터는 주어지지 않았다고 가정한다

![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2013.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2013.png)

- 훈련 데이터를 이용해서 가리워진 평가 데이터의 평점을 추정한다
- 추정한 평점과 실제 평가 데이터를 비교하여 오차를 측정한다
    - 오차를 측정하는 지표로는 평균 제곱 오차(Mean Squared Error, MSE)가 많이 사용된다

    ![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2014.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2014.png)

    - 평가 데이터 내의 평점들의 집합을 $T$라고 함
    - 평균 제곱근 오차(Root Mean Squared Error, RMSE)도 많이 사용됨

    ![Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2015.png](Day%2023%20Community,%20Recommendation%20system%20aae6f544aa0d4960a82587b0ea7a225a/Untitled%2015.png)

- 이 밖에도, 다양한 지표가 사용된다
    - 추정한 평점으로 순위를 매긴 후, 실제 평점으로 매긴 순위와의 상관 계수를 계산
    - 추천한 상품 중 실제 구매로 이루어진 것의 비율을 측정
    - 추천의 순서 혹은 다영성까지 고려하는 지표들도 사용된다