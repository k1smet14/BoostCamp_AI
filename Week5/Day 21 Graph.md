# Day 21 Graph

## 그래프

- 그래프(graph)는 정점 집합과 간선 집합으로 이루어진 수학적 구조
- 하나의 간선은 두 개의 정점을 연결
- 모든 정점 쌍이 반드시 간선으로 직접 연결되는 것은 아니다
- 그래프는 네트워크(Network)로도 불린다
- 정점(Vertex)은 노드(node)로 간선은 엣지(Edge) 혹은 링크(Link)로도 불린다
- 우리 주변에는 많은 복잡계(Complex System)이 있다
- 그래프는 복잡계를 표현하고 분석하기 위한 언어
- 복잡계는 구성 요소들 간의 상호작용으로 이루어지고, 그 상호작용을 표현하기 위한 수단으로 그래프를 사용
- 그래프를 공부함으로써 복잡계가 등장하는 수많은 분야에 활용할 수 있다

## 그래프 관련 인공지능 문제

- 정점 분류(Node Classification) 문제
- 연결 예측(Link Prediction) 문제
- 추천(Recommendation) 문제
- 군집 분석(Community Detection) 문제
    - 연결 관계로부터 사회적 무리(Social Circle)을 찾아낼 수 있을까?
- 랭킹(Ranking) 및 정보 검색(Information Retrieval) 문제
- 정보 전파(Information Cascading) 및 바이럴 마케팅(Viral Marketing) 문제

## 그래프 관련 기초 개념

- 방향이 없는 그래프(Undirected Graph) vs 방향이 있는 그래프(Directed Graph)
- 가중치가 없는 그래프(Unweighted Graph) vs 가중치가 있는 그래프(Weighted Graph)
- 동종 그래프(Unpartite Graph) vs 이종 그래프(Bipartite Graph)
- 보통 정점들의 집합을 $V$, 간선들의 집합을 $E$, 그래프를 $G = (V,E)$로 적는다
- 정점의 이웃(Neighbor)은 그 정점과 연결된 다른 정점을 의미
    - 정점 v의 이웃들의 집합을 보통 $N(v)$ 혹은 $N_v$로 적는다
- 방향성이 있는 그래프에서는 나가는 이웃과 들어오는 이웃을 구분
    - 정점 v에서 간선이 나가는 이웃(Out-Neighbor)의 집합을 $N_{out}(v)$
    - 정점 v로 간선이 들어오는 이웃(In-Neighbor)의 집합을 $N_{in}(v)$

## 그래프의 표현 및 저장

- 그래프를 다루기 위해 파이썬 라이브러리인 NetworkX를 사용
- NetworkX를 이용하여, 그래프를 생성, 변경, 시각화할 수 있다

```python
# 필요한 library를 임포트
import networkx as nx
import numpy as np
import matplotlib.pyplot as plt
```

```python
G = nx.Graph() # 방향성이 없는 그래프 초기화
DiGraph = nx.DiGraph() # 방향성이 있는 그래프 초기화
G.add_node(1) # 정점 1 추가
for i in range(1, 11): # 정점 2 ~ 10 추가
		G.add_node(i)

G.add_edge(1, 2) #정점 1과 2 사이에 간선 추가
for i in range(2, 11):
		G.add_edge(1, i)

# 그래프를 시각화
pos = nx.spring_layout(G) # 정점의 위치 결정
im = nx.draw_networkx_nodes(G, pos, node_color="red", node_size=100) # 정점의 색과 크기를 지정하여 출력
nx.draw_networkx_edges(G, pos) # 간선 출력
nx.draw_networkx_labels(G, pos, font_size=10, font_color="black") # 각 정점이 라벨을 출력
plt.show()
```

- 간선 리스트(Edge List): 그래프를 간선들의 리스트로 저장
    - 각 간선은 해당 간선이 연결하는 두 정점들의 순서쌍(Pair)로 저장
    - 방향성이 있는 경우에는(출발점, 도착점) 순서로 저장
- 인접 리스트(Adjacent List) - 방향성이 없는 경우:
    - 각 정점의 이웃들을 리스트로 저장
- 인접 리스트(Adjacent List) - 방향성이 있는 경우:
    - 각 정점의 나가는 이웃들과 들어오는 이웃들을 각각 리스트로 저장
- 인접 행렬(Adjacency Matrix) - 방향성이 없는 경우
    - 정점 수 X 정점 수 크기의 행렬
    - 정점 i와 j 사이에 간선이 있는 경우, 행렬의 i행 j열(그리고 j행 i열) 원소가 1 (간선이 없는 경우 0)
    - 대각선을 기준으로 대칭인 행렬
- 인접 행렬(Adjacency Matrix) - 방향성이 있는 경우
    - 정점 i에서 j로의 간선이 있는 경우, 행렬의 i행 j열 원소가 1 (간선이 없는 경우 0)

```python
nx.to_dict_of_lists(G) # 그래프를 인접 리스트로 저장
nx.to_edgelist(G) # 그래프를 간선 리스트로 저장
nx.to_numpy_array(G) # 그래프를 인접 행렬(일반 행렬)로 저장
nx.to_scipy_sparse_matrix(G) # 그래프를 인접 행렬(희소 행렬)로 저장
```

- 일반 행렬은 전체 원소를 저장하므로 정점 수의 제곱에 비례하는 저장 공간을 사용
- 희소 행렬은 0이 아닌 원소만을 저장하므로 간선의 수에 비례하는 저장 공간을 사용
- 행렬의 대부분이 0이 아닐 때에는 일반 행렬을 쓰는게 속도가 빠르다\

## 실제 그래프 vs 랜덤 그래프

- 실제 그래프(Real Graph)란 다양한 복잡계로 부터 얻어진 그래프를 의미
- 랜덤 그래프(Random Graph)는 확률적 과정을 통해 생성한 그래프를 의미
    - 에르되스-레니 랜덤 그래프(Erods-Renyi Random Graph)
        - 임의의 두 정점 사이에 간선이 존재하는지 여부는 동일한 확률 분포에 의해 결정됨
        - n개의 정점을 가지고, 임의의 두 개의 정점 사이에 간선이 존재할 확률은 p
        - $G(n,p)$로 표현되고, 정점 간의 연결은 서로 독립적(Independent)임

## 작은 세상 효과

- 정점 $u$와 $v$의 사이의 경로(Path)는 아래 조건을 만족하는 정점들의 순열(Sequence)
    1. $u$에서 시작해서 $v$에서 끝나야 함
    2. 순열에서 연속된 정점은 간선으로 연결되어 있어야 함
- 경로의 길이는 해당 경로 상에 놓이는 간선의 수로 정의됨
- 정점 $u$와 $v$의 사이의 거리(Distance)는 $u$와 $v$ 사이의 최단 경로의 길이
- 그래프의 지름(Diameter)은 정점 간 거리의 최댓값
- 임의의 두사람을 골랐을때, 몇 단계의 지인을 거쳐 연결되어 있을까?
    - 1960년 실험에서 평균 6단계, MSN 메신저 그래프에서는 평균 7단계
    - 이러한 현상을 작은 세상 효과(Small-world Effect)라고 부른다
- 작은 세상 효과는 높은 확률로 랜덤 그래프에도 존재함
- 하지만 모든 그래프에서 작은 세상 효과가 존재하는 것은 아니다
    - 예를 들어 체인(Chain), 사이클(Cycle), 격자(Grid) 그래프에서는 작은 세상 효과가 존재하지 않음

## 연결성의 두터운 꼬리 분포

- 정점의 연결성(Degree)은 그 정점과 연결된 간선의 수를 의미
- 정점 v의 연결성은 해당 정점의 이웃들의 수와 같다
    - 보통 정점 v의 연결성은 $d(v), d_v$혹은 $\left|N(v)\right|$ 로 쓴다
- 정점의 나가는 연결성(Out Degree)는 $d_{out}(v)$ 혹은 $|N_{out}(v)|$로 표시
- 정점의 들어오는 연결성(In Degree)는 $d_{in}(v)$ 혹은 $|N_{in}(v)|$로 표시
- 실제 그래프의 연결성 분포는 두터운 꼬리(Heavy Tail)를 갖는다
    - 즉, 연결성이 매우 높은 허브(Hub) 정점이 존재함을 의미
- 랜덤 그래프의 연결성 분포는 높은 확률로 정규 분포와 유사함
    - 연결성이 매우 높은 허브(Hub) 정점이 존재할 가능성은 0에 가깝다

![Day%2021%20Graph%2062ecc752e63f48ce94dd4c6bfc4d8ab1/Untitled.png](Day%2021%20Graph%2062ecc752e63f48ce94dd4c6bfc4d8ab1/Untitled.png)

## 거대 연결 요소

- 연결 요소(Connected Component)는 다음 조건들을 만족하는 정점들의 집합을 의미
    1. 연결 요소에 속하는 정점들은 경로로 연결될 수 있다
    2. 1.의 조건을 만족하면서 정점을 추가할 수 없다
- 실제 그래프에는 거대 연결 요소(Giant Connected Component)가 존재한다
- 거대 연결 요소는 대다수의 정점을 포함함
- 랜덤 그래프에도 높은 확률로 거대 연결 요소(Giant Connected Component)가 존재함
    - 단, 정점들의 평균 연결성이 1보다 충분히 커야 한다
    - 자세한 이유는 Random Graph Theory를 참고

## 군집 구조

- 군집(Community)란 다음 조건들을 만족하는 정점들의 집합
    1. 집합에 속하는 정점 사이에는 많은 간선이 존재한다
    2. 집합에 속하는 정점과 그렇지 않은 정점 사이에는 적은 수의 간선이 존재한다
    - 수학적으로 엄밀한 정의는 아님
- 지역적 군집 계수(Local Clustering Coefficient)는 한 정점에서 군집의 형성 정도를 측정함
    - 정점 i의 지역적 군집 계수는 정점 i의 이웃 쌍 중 간선으로 직접 연결된 것의 비율을 의미
    - 정점 i의 지역적 군집 계수를 $C_i$로 표현
    - 연결성이 0인 정점에서는 지역적 군집 계수가 정의되지 않음
- 정점 i의 지역적 군집 계수가 매우 높다면, 정점 i와 그 이웃들은 높은 확률로 군집을 형성함
- 전역 군집 계수(Global Clustering Coefficient)는 전체 그래프에서 군집의 형성 정도를 측정함
    - 그래프 G의 전역 군집 계수는 각 정점에서의 지역적 군집 계수의 평균
    - 단, 지역적 군집 계수가 정의되지 않는 정점은 제외
- 실제 그래프에서는 군집 계수가 높다. 즉 많은 군집이 존재한다
    - 동질성(Homophily): 서로 유사한 정점끼리 간선으로 연결될 가능성이 높다
    - 전이성(Transitivity): 공통 이웃이 있는 경우, 공통 이웃이 매개 역할을 해줄 수 있다
- 랜덤 그래프에서는 지역적 혹은 전역 군집 계수가 높지 않다
    - 구체적으로 랜덤 그래프 $G(n,p)$에서의 군집 계수는 $p$이다

## 실습: 군집 계수 및 지름 분석

- 실습에서는 다음 세 종류의 그래프의 구조를 분석한다

![Day%2021%20Graph%2062ecc752e63f48ce94dd4c6bfc4d8ab1/Untitled%201.png](Day%2021%20Graph%2062ecc752e63f48ce94dd4c6bfc4d8ab1/Untitled%201.png)

- 작은 세상 그래프는 균일 그래프의 일부 간선을 임의로 선택한 간선으로 대체한 그래프
- 구체적인 코드는 colab notebook 참조

# Appendix

- NetworkX Documentetaion

[https://networkx.org/documentation/stable/index.html](https://networkx.org/documentation/stable/index.html)

- [Snap.py](http://snap.py)

[https://snap.stanford.edu/snappy/](https://snap.stanford.edu/snappy/)