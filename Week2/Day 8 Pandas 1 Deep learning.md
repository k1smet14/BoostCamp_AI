# Day 8 Pandas 1 / Deep learning

# Pandas

- 구조화된 데이터의 처리를 지원하는 python 라이브러리
- panel data → pandas
- pandas 데이터 용어
    - data table, sample
    - attribute, field, feature
    - instance, row, tuple
    - feature vector
    - data
- pandas 설치

    ```python
    conda create -n ml python=3.8
    activate ml
    conda install pandas
    ```

- 데이터 로딩

```python
import pandas as pd
data_url = "Data url" # url 주소
df_data = pd.read_csv(data_url, sep='\s+', header=None)#separate는 빈공간
```

- `df_data.head()` # 처음 다섯 줄 출력

## Series

- DataFrame : Data table 전체를 포함하는 Object
- Series :  DataFrame 중 하나의 Column에 해당하는 데이터의 모음 Object

![Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled.png](Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled.png)

```python
list_data = [1,2,3,4,5]
list_name = ['a','b','c','d','e']
example_obj = Series(data = list_data, index = list_name)
```

- index로 문자로 할 수도 있음
- numpy.ndarray의 subclass
- dict타입을 넣으면 key값이 index로, value값이 value로 생성됨

```python
dict_data = {"a": 1, "b": 2, "c": 3, "d": 4, "e": 5}
example_obj = Series(dict_data, dtype=np.float32, name="example_data")
example_objin
```

- `example_obj['a']` index값에 접근 방법
- `example_obj.values` 값 리스트만
- `example_obj.index` Index 리스트만
- index 값을 기준으로 series 생성

```python
dict_data_1 = {"a": 1, "b": 2, "c": 3, "d": 4, "e": 5}
indexes = ["a", "b", "c", "d", "e", "f", "g", "h"]
series_obj_1 = Series(dict_data_1, index=indexes)
# f NaN, g NaN, h NaN
```

## DataFrame

- Series를 모아서 만든 data table = 기본 2차원

![Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%201.png](Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%201.png)

- `df = DataFrame(raw_data, columns = ["age", "city"]` column 선택
- `df.first_name`  column 선택 - series  추출
- `df["first_name"]` column 선택 - series 추출
- `df.loc[:, ["last_name"]]` loc - index location 인덱스 이름
- `df["age"].iloc[1:]` iloc - index position 인덱스 번호
- `df.debt = df.age > 40` boolean타입 series를 생성해서 새로운 컬럼에 할당
- `df.T` transpose
- `df.values` 값 출력
- `df.to_csv()` csv 변환
- `del df["debt"]` column 을 삭제함
- `df.drop("debt", axis=1)` column 삭제

## selection & drop

- xlrd 모듈이 없을 경우
    - !conda install --y xlrd
- `df["account"]` 한개의  column 선택시. 결과값이 series
- `df[["account", "street", "state"]].head(3)` 1개 이상의 column 선택. 리스트를 넣을 시, 결과값이 dataframe.
- `df.head(2).T` 전치해서 보기
- `df["name"][:3]` column 이름과 함께 row index 사용 시. 해당 column 만
- `account_serires[list(range(0, 15, 2))]` 리스트 형태로 가져오기
- `account_serires[account_serires < 250000]` boolean index
- basic, loc, iloc selection

![Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%202.png](Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%202.png)

```python
df.index = list(range(0, 15)) # index 변경
df.reset_index(inplace=True) # inplace 옵션 True면 df 자체가 변화함 
# 보통 자기 자신이 변하지 않게 함
```

- 삭제 (drop)

```python
df.drop(1) # index number로 drop
# df.drop(1, inplace=True)
df.drop([0, 1, 2, 3]) # 한개 이상의 index number로 drop
df.drop("city", axis=1) # axis 지정으로 축을 기준으로 drop
```

## dataframe operations

```python
s1 = Series(range(1, 6), index=list("abced"))
s2 = Series(range(5, 11), index=list("bcedef"))
s1 + s2 # s1.add(s2)
# a Nan, f NaN
# a, f 값이 없기 때문에 index 기준으로 연산 수행 해서 NaN값 으로 반환
s1.add(s2, fill_value=0)
# 없는 값을 0으로 채움
```

- operation types : add, sub, div, mul 등
- Series 데이터, Dateframe도 연산 가능 `df.add(s2, axis=0)`
    - axis를 기준으로 broadcasting이 일어남

## lambda, map, apply

- pandas의 series type의 데이터도 map 함수 사용 가능
- function 대신 dict, sequence형 자료등으로 대체 가능

```python
s1.map(lambda x: x**2).head(5)
z = {1: "A", 2: "B", 3: "C"}
s1.map(z) # dict type 으로 데이터 교체
s2 = Series(np.arange(10, 30))
s1.map(s2) # 같은 위치의 데이터를 s2로 전환
```

- replace : Map 함수의 기능중 데이터 변환 기능만 담당

```python
df.sex.replace({"male": 0, "female": 1})
df.sex.replace(["male", "female"], [0, 1], inplace=True)
```

- apply : map과 달리, series 전체(column)에 해당 함수를 적용
- 입력 값이 series 데이터로 입력 받아 handling

```python
f = lambda x: np.mean(x)
df_info.apply(f)

df_info.apply(np.mean)
df_info.mean()

def f(x):
    return Series(
        [x.min(), x.max(), x.mean(), sum(x.isnull())],
        index=["min", "max", "mean", "null"],
    )
df_info.apply(f)
```

- applymap :  series 단위가 아닌 모든 element 단위로 함수를 적용함

```python
f = lambda x: x // 2
df_info.applymap(f).head(5)

f = lambda x: x ** 2
df_info["earn"].apply(f) # series 단위에 apply를 적용시킬 때와 같은 효과
```

## pandas built-in functions

- `df.describe()` Numeric type 데이터의 정보를 보여줌
- unique() : series data의 유일한 값을 list로 반환함

```python
key = df.race.unique()
value = range(len(df.race.unique()))
df["race"].replace(to_replace=key, value=value)
```

- sum() : 기본적인 column 또는 row값의 연산을 지원
- sub, mean, min, max, count, median, mad, var 등

```python
numueric_cols = ["earn", "height", "ed", "age"]
df[numueric_cols].sum(axis=1)

df.sum(axis=1)
```

- isnull() : column 또는 row 값의 NaN(null) 값의  index를 반환함

```python
df.isnull()

df.isnull().sum() / len(df)
```

- sort_values() : column 값을 기준으로 데이터를 sorting

```python
df.sort_values(["age", "earn"], ascending=True) # 오름차순
```

- corr() : 상관계수. cov() : 공분산. corrwith() : 나머지 값 전체와의 상관계수

```python
df.age.corr(df.earn)

df.age[(df.age < 45) & (df.age > 15)].corr(df.earn)

df.age.cov(df.earn)

df.corrwith(df.earn)
```

- df.corr() 모든 데이터의 상관관계를 보여줌
- `df.sex.value_counts(sort=True) / len(df)` 값 개수 측정
- pd.options.display.max_rows = 100  # 100개 보여줌

# 딥러닝 학습방법 이해하기

- 비선형모델인 신경망(neural network)를 보자

![Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%203.png](Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%203.png)

- Softmax
    - 모델의 출력을 확률로 해석할 수 있게 변환 해주는 연산
    - 분류 문제를 풀 때 선형모델과 소프트맥스 함수를 결합하여 예측

![Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%204.png](Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%204.png)

![Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%205.png](Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%205.png)

- 신경망은 선형모델과 활성함수(activation function)을 합성한 함수
- 활성 함수
    - 비선형(nonlinenar)함수로서 딥러닝에 중요한 개념
    - 활성함수를 쓰지 않으면 딥러닝은 선형모형과 차이가 없다
    - sigmoid 함수나 tanh 함수가 있으나 딥러닝에선 ReLU함수를 많이 쓴다

    ![Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%206.png](Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%206.png)

- 다층(multi-layer) 퍼셉트론(MLP)은 신경망이 여러층 합성된 함수

![Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%207.png](Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%207.png)

- 왜 층을 여러개 쌓나요?
    - 이론적으로는 2층 신경망으로도 임의의 연속함수를 근사 할 수 있다
    - 그러나 층이 깊을수록 목적함수를 근사하는데 필요한 뉴런(노드)의 숫자가 훨씬 빨리 줄어들어 좀 더 효율적으로 학습이 가능하다
- 역전파 알고리즘
    - 딥러닝은 역전파(backpropagation) 알고리즘을 이용하여 각 층에 사용단 패러미터$\{{\mathbf{W}^{(ℓ)}, \mathbf{b}^{(ℓ)}\}}^L_ℓ=1$를 학습한다.
    - 각 층 패러미터의 그레디언트 벡터는 윗층부터 역순으로 계산

    ![Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%208.png](Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%208.png)

- 2층 신경망

![Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%209.png](Day%208%20Pandas%201%20Deep%20learning%20cb7d987128cf4e0a9ad8bfce3cb29499/Untitled%209.png)