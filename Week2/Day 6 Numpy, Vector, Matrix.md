# Day 6 Numpy, Vector, Matrix

# Numpy

- Numerical Python
- 일반 List에 비해 빠르고, 메모리 효율적
- Numpy install

    ```python
    activate ml
    conda install numpuy
    ```

## ndarray

- numpy는  np.array 함수를 활용하여 배열을 생성
- numpy는 하나의 데이터 type만 배열에 넣을 수 있음
- numpy array는 어떤 메모리 공간에 차례대로 저장함

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled.png)

- shape : numpy array의 dimension 구성을 반환함 ( .shape)
- dtype : numpy array의 데이터 type을 반환함 ( .dtype)
- dimension 과 shape의 관계

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%201.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%201.png)

- ndarray의 single element가 가지는 data type : dtype

    `np.array([1,2,3], [4.5, "5", "6"]], dtype=np.float32)`

- nbytes : ndarray object의 메모리 크기를 반환함 ( .nbytes)

## Handling shape

- reshape : Array의 shape의 크기를 변경함. element의 갯수는 동일

```python
np.array(test_matrix).reshape(8, )
np.array(test_matrix).reshape(2, 4).shape
np.array(test_matrix).reshape(-1, 2) # -1 : size 기반으로 row 개수 선정
```

- flatten : 다차원 array를 1차원 array로 변환

    `np.array(test_matrix).flatten()`

## Indexing & Slicing

- list와 달리 이차원 배열에서 [0,0] 표기법을 제공함

```python
a = np.array([[1, 2, 3], [4.5, 5, 6]], int)
print(a)
print(a[0,0]) # Two dimensional array representation #1
print(a[0][0]) # Two dimensional array representation #2
a[0,0] = 12 # Matrix 0,0 에 12 할당
a[0][0] = 5 # Matrix 0,0 에 12 할당
```

- list와 달리 행과 열 부분을 나눠서 slicing이 가능함
- matrix의 부분 집합을 추출할 때 유용

```python
a = np.array([[1, 2, 3, 4, 5], [6, 7, 8, 9, 10]], int)
a[:,2:] # 전체 Row의 2열 이상
a[1,1:3] # 1 Row의 1열 ~ 2열
a[1:3] # 1 Row ~ 2Row의 전체
```

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%202.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%202.png)

## creation function

- arange : array의 범위를 지정하여, 값의 list를 생성하는 명령어

```python
np.arange(30) # List의 range와 같은 효과,  integer로 0부터 29까지 배열
np.arange(0, 5, 0.5) # (start, end, step) floating point도 표시 가능.
np.arange(30).reshape(5,6)
```

- np.zeros(shape, dtype, order), ones(shape, dtype, order)

```python
np.zeros(shape=(10,), dtype=np.int8) # 10 - zero vector 생성
np.zeros((2,5)) # 2 by 5 - zero matrix 생성

np.ones(shape=(10,), dtype=np.int8)
np.ones((2,5))
```

- np.empty : shape만 주어지고 비어있는 ndarray 생성
    - memory initialization 이 되지 않음

```python
np.empty(shape=(10,), dtype=np.int8)
np.empty((2,5))
```

- something_like
    - 기존 ndarray의 shape 크기 만큼 1,0 또는 empty array를 반환

```python
test_matrix = np.arange(30).reshape(5,6)
np.ones_like(test_matrix)
```

- np.identity : 단위 행렬(i 행렬)을 생성함

```python
np.identity(n=3, dtype=np.int8) # 3 by 3 단위 행렬
np.identity(5) # 5 by 5
```

- np.eye : 대각선 1인 행렬, k값의 시작 index의 변경이 가능

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%203.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%203.png)

- np.diag : 대각 행렬의 값을 추출함

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%204.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%204.png)

- random sampling : 데이터 분포에 따른 sampling으로 array 생성

```python
np.random.uniform(0, 1, 10).reshape(2,5) # 균등분포
np.random.normal(0, 1, 10).reshape(2,5) # 정규분포
```

## operation functions

- sum `array.sum(dtype=np.float)`
- axis : 모든 operation function을 실행할 때 기준이 되는 dimension 축

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%205.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%205.png)

- mean & std : ndarray의 element들 간의 평균 , 표준편차를 반환

```python
test_array.mean()
test_array.mean(axis=0)
test_array.std()
test_array.std(axis=0)
```

- 그 외에도 다양한 수학 연산자를 제공함

    `np.exp(test_array), np.sqrt(test_array) ....`

    ![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%206.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%206.png)

- concatenate
- numpy array를 합치는 함수

```python
np.vstack((a,b))
np.concatenate((a,b), axis=0)
np.hstack((a,b))
np.concatenate((a,b.T), axis=1)
b = b[np.newaxis, :] # 축 추가
```

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%207.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%207.png)

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%208.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%208.png)

## array operations

- numpy는 array간의 기본적인 사칙 연산을 지원함
- `matrix_a * matrix_b` Element-wise operations. ( array간 shape가 같을 때)
- Dot product : Matrix의 기본 연산, dot 함수 사용

```python
test_a = np.arange(1,7).reshape(2,3)
test_b = np.arange(7,13).reshape(3,2)
test_a.dot(test_b)
```

- transpose : transpose 또는 T attribute 사용

```python
test_a.transpose()
test_a.T
```

- broadcasting :  Shape이 다른 배열 간 연산을 지원하는 기능
    - matrix , scalar간 사칙연산이 가능함. // 연산자, **연산자도 가능
    - vector - matrix간의 연산도 지원. vector가 확장됨

- numpy performance
    - 일반적인 속도는 아래 순
        - for loop < list comprehension < numpy

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%209.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%209.png)

## comparisons

- All & Any : array의 데이터 전부(and) 또는 일부(or)가 조건에 만족 여부 반환

```python
a = np.arange(10)
np.any(a>5), np.any(a<0)
# (True, False)
np.all(a>5), np.all(a<10)
# (False, True)
```

- 배열의 크기가 동일 할 때 element간 비교의 결과를 Boolean 으로 반환

```python
test_a > test_b
test_a == test_b
(test_a > test_b).any()
```

```python
np.logical_and(a>0, a<3)
np.logical_not(b)
np.logical_or(b,c)
```

- np.where : 조건을 만족하는 Index 값 반환

```python
np.where( a> 0, 3, 2)
np.where(a>5) # Index 값 반환
np.isnan(a) # Not a number
np.isfinite(a) # is finite number
```

- argmax &  argmin : array내 최대값 또는 최소값의 index를 반환
    - axis 기반의 반환도 가능

    ```python
    a.argsort() # 작은값부터 정렬한 index를 반환
    a.argsort()[::-1]
    np.argmax(a), np.argmin(a)
    np.argmax(a, axis=1), np.argmin(a, axis=0)
    ```

## boolean & fancy index

- boolean index : 특정 조건에 따른 값을 배열 형태로 추출

```python
test_array > 3
# [False, True, False .....]
test_array[test_array > 3] # 조건이 True인 index의 element만 추출

condition = test_array < 3
test_array[condition]
```

- fancy index : array를 index value로 사용해서 값 추출

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2010.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2010.png)

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2011.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2011.png)

## numpy data i/o

- loadtxt & savetxt : text type의 데이터를 읽고, 저장하는 기능

```python
a = np.loadtxt("./populations.txt") # 파일 호출
a_int = a.astype(int) # Int type 변환
np.savetxt('int_data.csv', a_int, fmt="%.2e", delimiter=",") # csv로 저장
```

- np.save(), np.load()

# Mathematics for Artificial Intelligence

## 벡터

- 벡터는 숫자를 원소로 가지는 리스트 또는 배열
- 벡터는 공간에서 한 점을 나타냄
- 벡터는 원점으로부터 상대적 위치를 표현
- 벡터끼리 같은 모양을 가지면 성분곱(Hadamard product)을 계산할 수 있다.
- 두 벡터의 덧셈은 다른 벡터로부터 상대적 위치 이동을 표현
- 벡터의 노름(norm)은 원점에서부터의 거리를 말함

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2012.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2012.png)

- 노름의 종류에 따라 기하학적 성질이 달라짐
- 머싱러닝에선 각 성징들이 필요할 때가 있으므로 둘다 사용함

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2013.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2013.png)

- 두 벡터 사이의 거리 : L1, L2-노름을 이용해 계산할 수 있다.
    - 벡터의 뺄셈을 이용 $||y-x|| = ||x-y||$
- 두 벡터 사이의 각도 :  L2-노름, 제2 코사인 법칙에 의해 계산 가능

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2014.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2014.png)

- 내적의 해석

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2015.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2015.png)

## 행렬

- 행렬은 벡터를 원소로 가지는 2차원 배열
- 벡터가 공간에서 한 점을 의미한다면 행렬은 여러 점들을 나타냄
- 행렬끼리 같은 모양을 가지면 덧셈, 뺄셈 가능
- 성분곱은 벡터와 같음 → 각 인덱스 위치끼리 곱
- 행렬곱셈(matrix multiplication)은 i번째 행벡터와  j번쨰 열벡터 사이의 내적을 성분으로 가지는 행렬.
- numpy에선 @연산자를 사용
- 넘파이의 np.inner는 i번쨰 행벡터와 j번째 행벡터 사이의 내적을 성분으로 가지는 행렬을 계산함.  → 수학에서 말하는 내적과는 다르므로 주의!
- 행렬은 벡터공간에서 사용되는 연산자로 이해한다

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2016.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2016.png)

- 행렬곱을 통해 벡터를 다른 차원의 공간으로 보낼 수 있음
- 행렬곱을 통해 패턴을 추출할 수 있고 데이터를 압축할 수도 있음
- 어떤 행렬A의 연산을 거꾸로 되돌리는 행렬을 역행렬(inverse matrix)이라 부름
- 역행렬은 행과 열 숫자가 같고, (determinant)이 0이 아닌 경우만 계산 가능

```python
np.linalg.inv(X)
X @ np.linalg.inv(X)
```

- 만일 역행렬을 계산할 수 없다면 유사역행렬(pseudo-inverse) 또는 무어-펜로즈 (Moore-Penrose) 역행렬 $A+$을 이용한다.

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2017.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2017.png)

```python
np.linalg.pinv(Y)
np.linalg.pinv(Y) @ Y
```

- np.linalg.pinv를 이용하면 연립방정식의 해를 구할 수 있다.

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2018.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2018.png)

- np.linalg.pinv를 이용하면 데이터를 선형모델(linear model)로 해석하는 선형회귀식을 찾을 수 있다.

![Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2019.png](Day%206%20Numpy,%20Vector,%20Matrix%203f190b28c80e424e88ab4e2e8a2701da/Untitled%2019.png)

- sklearn의 LinearRegression과 같은 결과를 가져올 수 있다