# Day 10 Visualization / Statistics

# Matplotlib

- 다양한 graph 지원, Pandas 연동
- pyplot 객체를 사용하여 데이터를 표시
- pyplot 객체에 그래프들을 쌓은 다음 flush
- 최대단점 argument를 kwargs로 받음. shift + tab 으로 확인 어려움.
- graph는 원래 figure 객체에 생성됨
- pyplot 객체 사용시, 기본 figure에 그래프가 그려짐
- Matplotlib는 Figure 안에 Axes로 구성. Figure위에 여러 개의 Axes를 생성

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled.png)

- subplot의 순서를 grid로 작성
- color 속성 사용. `plt.plot(x1, y1, color="#eeefff")` or `color="r"`
- ls 또는 linestyle 속성 `plt.plot(x1, y1, linestyle="dashed")` or `ls="dotted"`
- title함수 사용, figure의 subplot별 입력 가능 `plt.title("two lines")`
- latex 타입의 표현도 가능 `plt.title('$y = \\frac{ax + b}{test}$')`
- legend함수로 범례를 표시함, loc위치 등 속성 지정

    `plt.legend(shadow=True, fancybox=True, loc="lower right")`

- Graph 보조선을 긋는 grid와  xy축 범위 한계를 지정

    `plt.grid(True, lw=0.4, ls="__", c=".90")`

    `plt.xlim(-100, 200)`

- scatter 함수 사용, marker : scatter 모양 지정

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%201.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%201.png)

- s : 데이터의 크기를 지정, 데이터의 크기 비교 가능

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%202.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%202.png)

- bar 함수 사용 : 막대 그래프
- histogram :  `plt.hist(X, bins=100)`
- boxplot : `plt.boxplot(data)`

# Seaborn

- matplotlib를 더 쉽게. 기존 matplotlib에 기본 설정을 추가
- 복잡한 그래프를 간단하게 만들 수 있는 wrapper
- 손쉬운 설정으로 데이터 산출
- lineplot, scatterplot, countplot 등

```python
sns.lineplot(x="timepoint", y="signal", hue="event", data=fmri) #선그래프
sns.scatterplot(x="total_bill", y="tip", data=tips) # scatter 그래프
sns.countplot(x="smoker", hue="time", data=tips) # 카운트한 그래프
sns.barplot(x="day", y="tip", data=tips) # 막대 그래프
sns.set(style="darkgrid")
sns.distplot(tips["total_bill"]) # 분포 그래프
```

- violinplot - boxplot에 distribution을 함께표현
- stripplot - scatter와 category 정보를 함께 표현
- swarmplot - 분포와 함께 scatter를 함께 표현
- pointplot - category별로 numeric의 평균, 신뢰구간 표시
- regplot - scatter + 선형함수를 함께 표시
- 한 개 이상의 도표를 하나의 plot에 작성.  Axes를 사용해서 grid를 나눔

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%203.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%203.png)

- replot - Numeric 데이터 중심의분포 /  선형 표시
- catplot - category 데이터 중심의 표시
- FacetGrid - 특정 조건에 따른 다양한 plot을 grid로 표시
- pairplot - 데이터 간의 상관관계 표시
- Implot - regression 모델과 category 데이터를 함께 표시

---

# Statistics

## 모수란?

- 통계적 모델링은 적절한 가정 위에서 확률분포를 추정(inference)하는 것이 목표.
- 유한한 개수의 데이터만 관찰해서 모집단의 분포를 정확하게 알아낸 다는 것은 불가능하므로, 근사적으로 확률분포를 추정 할 수 밖에 없다
- 데이터가 특정 확률분포를 따른다고 선험적으로(a prior) 가정한 후 그 분포를 결정하는 모수(parameter)를 추정하는 방법을 모수적(parametric) 방법론이라고 함
- 특정 확률분포를 가정하지 않고 데이터에 따라 모델의 구조 및 모수의 개수가 유연하게 바뀌면 비모수(nonparametric) 방법론이라 부름

## 확률분포 가정하기 : 히스토그램을 통해 모양을 관찰

- 데이터가 2개의 값(0 또는 1)만 가지는 경우 → 베르누이 분포
- 데이터가 n 개의 이산적인 값을 가지는 경우→ 카테고리 분포
- 데이터가 [0,1] 사이에서 값을 가지는 경우→ 베타분포
- 데이터가 0 이상의 값을 가지는 경우→ 감마분포, 로그정규분포 등
- 데이터가 ℝ 전체에서 값을 가지는 경우→정규분포, 라플라스분포 등
- 기계적으로 확률분포를 가정해서는 안 되며, 데이터를 생성하는 원리를 먼저 고려하는 것이 원칙

## 데이터로 모수를 추정해보자

- 데이터의 확률분포를 가정했다면 모수를 추정해 볼 수 있다
- 정규분포의 모수는 평균 $\mu$ 과 분산 $\sigma^2$으로, 이를 추정하는 통계량(statistic)은 :

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%204.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%204.png)

- 통계량의 확률분포를 표집분포(sampling distribution)라 부르며, 특히 표본평균의 표집분포는 $N$이 커질수록 정규분포 $\mathcal{N}(\mu, \sigma^2/N)$를 따른다.
    - 이를 중심극한정리(Central Limit Theorem)이라 부르며, 모집단의 분포가 정규분포를 따르지 않아도 성립

## 최대가능도 추정법

- 표본평균이나 표본분산은 중요한 통계량이지만 확률분포마다 사용하는 모
수가 다르므로 적절한 통계량이 달라지게 됩니다
- 이론적으로 가장 가능성이 높은 모수를 추정하는 방법 중 하나는 최대가능도 
추정법(maximum likelihood estimation, MLE)이다

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%205.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%205.png)

- 데이터 집합 X가 독립적으로 추출되었을 경우 로그가능도를 최적화 함

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%206.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%206.png)

- 로그가능도를 최적화하는 모수 $\theta$는가능도를 최적화하는 MLE가 된다
- 데이터의 숫자가 수억 단위라면 컴퓨터의 정확도로 가능도를 계산하는 것은 불가능
- 데이터가 독립일 경우, 로그를 사용하면 가능도의 곱셉을 로그가능도의 덧셈으로 바꿀 수 있기 때문에 컴퓨터로 연산이 가능
- 경사하강법으로 가능도를 최적화할 때 미분 연산을 사용하게 되는데, 로그 가능도를 사용하면 연산량을 $O(n^2)$에서 $O(n)$ 으로 줄여줌
- 대게의 손실함수의 경우 경사하강법을 사용하므로 음의 로그가능도(negative log-likelihood)를 최적화 함

## 최대가능도 추정법 예제: 정규분포

- 정규분포를 따르는 확률변수 $X$로부터 독립적인 표본 $\{\mathbf{x}_1,...,\mathbf{x}_n\}$을 얻었을 때 최대가능도 추정법을 이용하여 모수를 추정하면?

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%207.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%207.png)

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%208.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%208.png)

## 최대가능도 추정법 예제 : 카테고리 분포

- 카테고리 분포 Multinoulli $(\mathbf{x};p_1,...,p_d)$를 따르는 확률변수 $X$로부터 독립적인 표본 $\{\mathbf{x_1},...,\mathbf{x}_n\}$ 을 얻었을 때 최대가능도 추정법을 이용하여 모수를 추정?

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%209.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%209.png)

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%2010.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%2010.png)

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%2011.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%2011.png)

## 딥러닝에서 최대가능도 추정법

- 딥러닝 모델의 가중치를 $\theta = (\mathbf{W}^{(1)},...,\mathbf{W}^{(L)})$ 라 표기했을 때 분류 문제에서 소프트맥스 벡터는 카테고리분포의 모수 $(p_1,...,p_k)$를 모델링 함
- 원핫벡터로 표현한 정답레이블 $\mathbf{y} = (y_1,...,y_k)$를 관찰데이터로 이용해 확률분포인 소프트맥스 벡터의 로그가능도를 최적화할 수 있다.

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%2012.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%2012.png)

## 확률분포의 거리를 구해보자

- 기계학습에서 사용되는 손실함수들은 모델이 학습하는 확률분포와 데이터에서 관찰되는 확률분포의 거리를 통해 유도
- 데이터공간에 두 개의 확률분포 $P(\mathbf{x}), Q(\mathbf{x})$가 있을 경우 두 확률분포 사이의 거리(distance)를 계산할 때 다음과 같은 함수들을 이용
    - 총변동 거리 (Total Variation Distance,TV)
    - 쿨백-라이블러발산 (Kullback-Leibler Divergence, KL)
    - 바슈타인거리 (Wasserstein Distance)

## 쿨백-라이블러 발산

- 쿨백-라이블러 발산(KL Divergence)은 다음과 같이 정의

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%2013.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%2013.png)

- 쿨백-라이블러는 다음과 같이 분해할 수 있음

![Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%2014.png](Day%2010%20Visualization%20Statistics%20f46d59e614e9428a913b531ad3ed6c32/Untitled%2014.png)

- 분류 문제에서 정답레이블을 $P$, 모델 예측을 $Q$라 두면 최대가능도 추정법은 쿨백-라이블러 발산을 최소화하는 것과 같다

# Appendix

- Dive into Deep Learning

[https://d2l.ai/index.html](https://d2l.ai/index.html)

- Dive into Deep Learning 한글판

[https://ko.d2l.ai/chapter_appendix/how-to-contribute.html](https://ko.d2l.ai/chapter_appendix/how-to-contribute.html)

- Mathmatics for Machine Learning

[https://mml-book.github.io/book/mml-book.pdf](https://mml-book.github.io/book/mml-book.pdf)