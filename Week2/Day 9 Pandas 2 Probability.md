# Day 9 Pandas 2 / Probability

# Groupby

- `df.groupby("Team")["Points"].sum()` Team을 기준으로 Points를 Sum
- `df.groupby(["Team", "Year"])["Points"].sum` 한개 이상의 column을 묶음
- Groupby의 결과물도 결국 dataframe
- 두 개의 column으로 groupby를 할 경우, index가 두개 생성

```python
h_index.unstack() # Group으로 묶여진 데이터를 matrix 형태로 전환해줌
h_index.swaplevel() # Index level을 변경할 수 있음
h_index.swaplevel().sortlevel(0)
h_index.sum(level=0) # Index level을 기준으로 기본 연산 수행 가능
h_index.sum(level=1)
```

- Groupby에 의해 Split된 상태를 추출 가능함

```python
grouped = df.groupby("Team")
for name, group in grouped:
		print(name)
		print(group)

grouped.get_group("Devils") # 특정 key 값을 가진 그룹의 정보만 추출
```

- 추출 된 group 정보에는 세 가지 유형의 apply가 가능함
- Aggregation : 요약된 통계 정보를 추출해 줌
- Transformation : 해당 정보를 변환해줌
- Filtration : 특정 정보를 제거 하여 보여주는 필터링 기능

```python
grouped.agg(sum)
grouped.agg(np.mean)
grouped['Points'].agg([np.sum, np.mean, np.std]) 
#특정 컬럼에 여러개의 function을 apply 할 수도 있음

score = lambda x: (x.max())
grouped.transform(score)
score = lambda x: (x - x.mean()) / x.std()
grouped.transform(score)
# 개별 데이터의 변환을 지원함

df.groupby("Team").filter(lambda x: len(x) >= 3)
df.groupby("Team").filter(lambda x: x["Points"].sum() > 1000)
# 특정 조건으로 데이터를 검색할 때 사용

```

## Pivot table

- Index 축은 groupby와 동일함
- Column에 추가로  labeling 값을 추가하여,
- Value에 numeric type 값을 aggregation 하는 형태

```python
df_phone.pivot_table(["duration"],
											index=[df_phone.month, df_phone.item],
											columns=df_phone.network, aggfunc="sum", fill_value=0)
```

## Crosstab

- 두 칼럼의 교차 빈도, 비율, 덧셋 등을 구할 때 사용
- Pivot table의 특수한 형태
- User-Item Rating Matrix 등을 만들 때 사용 가능

```python
pd.crosstab(
    index=df_movie.critic,
    columns=df_movie.title,
    values=df_movie.rating,
    aggfunc="first",
).fillna(0)

df_movie.groupby(["critic", "title"]).agg({"rating": "first"})
```

## Merge

- 두 개의 데이터를 하나로 합침

```python
pd.merge(df_a, df_b, on="subject_id")
pd.merge(df_a, df_b, left_on="subject_id", right_on="subject_id")
# 두 dataframe이 column이름이 다를 때

pd.merge(df_a, df_b, on="subject_id", how="left")
pd.merge(df_a, df_b, on="subject_id", how="right")
pd.merge(df_a, df_b, on="subject_id", how="outer")
pd.merge(df_a, df_b, on="subject_id", how="inner")
pd.merge(df_a, df_b, right_index=True, left_index=True)# index based join
```

![Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled.png](Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled.png)

## Concat

- 같은 형태의 데이터를 붙이는 연산작업

```python
df_new = pd.concat([df_a, df_b])
df_new.reset_index()

df_a.append(df_b)

df_new = pd.concat([df_a, df_b], axis=1)
```

---

# 확률론 맛보기

- 딥러닝은 확률론 기반의 기계학습 이론에 바탕을 두고 있다.
- 손실 함수들의 작동 원리는 데이터 공간을 통계적으로 해석해서 유도
- 회귀 분석에서 손실함수로 사용되는 $L_2$-노름은 예측오차의 분산을 가장 최소화하는 방향으로 학습하도록 유도
- 분류 문제에서 사용되는 교차엔트로피는 모델 예측의 불확실성을 최소화하는 방향으로 학습하도록 유도
- 분산 및 불확실성을 최소화하기 위해서는 측정하는 방법을 알아야.

### 이산확률변수 vs 연속확률변수

- 확률변수는 확률분포에 따라 discrete과 continuous확률변수로 구분
- 이산형 확률변수는 확률변수가 가질 수 잇ㅅ는 경우의 수를 모두 고려하여 확률을 더해서 모델링

![Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%201.png](Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%201.png)

- 연속형 확률변수는 데이터 공간에 정의된 확률변수의 밀도(density)위에
서의 적분을 통해 모델링한다

![Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%202.png](Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%202.png)

- 조건부확률분포 $P(\mathbf{x}|y)$는 데이터 공간에서 입력 $\mathbf{x}$와 출력 y 사이의 관계를 모델링 → $P(\mathbf{x}|y)$는 특정 클래스가 주어진 조건에서 데이터의 확률분포를 보여줌

### 조건부확률과 기계학습

- 조건부확률 $P(y|\mathbf{x})$는 입력 변수 $\mathbf{x}$ 에 대해 정답이 $y$ 일 확률을 의미
- 로지스틱 회귀에서 사용했던 선형모델과 소프트맥스 함수의 결합은 데이터에서 추출된 패턴을 기반으로 확률을 해석하는데 사용
- 분류 문제에서 softmax$(\mathbf{W}\phi + \mathbf{b})$ 은 데이터 $\mathbf{x}$ 로부터 추출된 특징패턴 $\phi(\mathbf{x})$과 가중치행렬 $\mathbf{W}$을 통해 조건부확률 $P(y|\mathbf{x})$을 계산
- 회귀 문제의 경우 조건부 기대값 E$[y|\mathbf{x}]$을 추정
- 딥러닝은 다층신경망을 사용하여 데이터로부터 특징패턴 $\phi$을 추출

## 기대값

- 확률분포가 주어지면 데이터를 분석하는데 사용 가능한 여러 종류의 통계적 범함수를 계산 할 수 있다.
- 기대값(expectation)은 데이터를 대표하는 통계량이면서 동시에 확률분포를 통해 다른 통계적 범함수를 계산하는데 사용됨

![Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%203.png](Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%203.png)

- 기대값을 이용해 분산, 첨도, 공분산 등 여러 통계량을 계산 할 수 있음

![Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%204.png](Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%204.png)

## 몬테카를로 샘플링

- 기계학습의 많은 문제들은 확률분포를 명시적으로 모를 때가 대부분
- 확률분포를 모를 때 데이터를 이용하여 기대값을 계산하려면 몬테카를로
(MonteCarlo) 샘플링 방법을 사용해야 함

![Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%205.png](Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%205.png)

- 몬테카를로 샘플링은 독립추출만 보장된다면 대수의 법칙(law of large number)에 의해 수렴성을 보장한다

![Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%206.png](Day%209%20Pandas%202%20Probability%203ae6dd85681a43e1b452af25a898794b/Untitled%206.png)

 

# 번외.

pandas 치트 시트

[[https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf](https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf)]

기울기 벡터(Gradient vector)의 기하학적 의미

[[https://blog.naver.com/mindo1103/90153612595](https://blog.naver.com/mindo1103/90153612595)]

Latex 문법

[https://ko.wikipedia.org/wiki/위키백과:TeX_문법](https://ko.wikipedia.org/wiki/%EC%9C%84%ED%82%A4%EB%B0%B1%EA%B3%BC:TeX_%EB%AC%B8%EB%B2%95)

[https://librewiki.net/wiki/수학_기호](https://librewiki.net/wiki/%EC%88%98%ED%95%99_%EA%B8%B0%ED%98%B8)

선형대수학

[https://darkpgmr.tistory.com/108](https://darkpgmr.tistory.com/108)