# Day 3 파이썬 기초 문법2
---

# Python data structure

## Stack

- Last In First Out(LIFO)
- a.pop() → return 값을 가지면서 리스트a를 변화시키는 함수
- 리스트를 사용하여 스택 구조를 구현 가능

## Queue

- First In First Out(FIFO)
- Stack과 반대되는 개념

## Tuple

- 값의 변경이 불가능한 리스트
- 선언 시 ( ) 를 사용
- 함수의 반환 값 등 사용자의 실수에 의한 에러를 사전에 방지

## Set

- 값을 순서없이 저장, 중복 불허 하는 자료형
- set 객체 선언을 이용하여 객체 생성
- union( | ), intersection( & ), difference( - ) 집합 연산이 가능

## Dict

- Key 값을 활용하여, 데이터 값(Value)를 관리함
- {key1:value1, key2:value2, ...} 형태
- 다른 언어에서는 Hash Table 이라는 용어를 사용

# Collections

## deque

- Stack과 Queue를 지원하는 모듈
- List에 비해 효율적인=빠른 자료 저장 방식을 지원
- rotate, reverse등 Linked List의 특성을 지원
- 기존 list 형태의 함수를 모두 지원하면서 보다 효율적인 자료구조

## OderedDict

- python 3.6부터 dict도 입력한 순서를 보장하여 출력함

## defaultdict

- dict type의 값에 기본 값을 지정, 신규값 생성시 사용하는 방법

```python
from collections import defaultdict
d = defaultdict(object) # Default dictionary를 생성
d = defaultdict(lambda: 0) # Default 값을 0으로 설정함
print(d["first"])
```

## Counter

- Sequence type의 data element들의 갯수를 dict 형태로 반환

```python
from collections import Counter
c = Counter()
c = Counter('gallahad')
print(c)  # Counter({'a': 3, 'l': 2, 'g': 1, 'd': 1, 'h': 1})
```

- Dict type, keyword parameter 등도 모두 처리 가능

```python
c = Counter({'red': 4, 'blue': 2})
c = Counter(cats=4, dogs=4)
```

- Set의 연산들을 지원함

## namedtuple

- tuple 형태로 data 구조체를 저장하는 방법
- 저장되는 data의 variable을 사전에 지정해저 저장함

---

 

# Pythonic code

- 파이썬 특유의 문법을 활요하여 효율적으로 코딩하는 기법
- 많은 언어들이 서로의 장점을 채용하는 추세
- 남 코드에 대한 이해도가 올라가고 효율적이다

## Split & join

- split() → string type의 값을 기준값으로 나눠서 list형태로 변환
- join() → String으로 구성된 list를 합쳐 하나의 string으로 반환

## List comprehension

- 기존 List를 사용하여 간단히 다른 List를 만드는 기법
- 일반적으로 for + append보다 속도가 빠름

```python
result = [i+j for i in case_1 for j in case_2 if not(i==j)]
result = [i+j if not(i==j) else i fr i in case_1 for j in case_2]
# filter : i와 j가 같다면 list에 추가하지 않음

result = [i+j for i in case_1 for j in case_2]
# for i in case_1:
#   for j in case_2:
#     i+j
result = [[i+j for i in case_1] for j in case_2]
# case_1 의 for문이 먼저 돌아감
```

## enumerate & zip

- enumerate : list의 element를 추출할 때 번호를 붙여서 추출
- zip : 두 개의 list값을 병렬적으로 추출

```python
for i, (a,b) in enumerate(zip(alist, blist)):
		print(i, a, b)
# enumerate & zip 동시 사용
```

## lambda & map & reduce

- lambda
    - 함수 이름 없이, 함수처럼 쓸 수 있는 익명함수
    - Python 3부터는 권장하지는 않으나 여전히 많이 쓰임
- map
    - 두 개 이상의 list에 적용. if filter도 가능
    - iteration을 생성하기 때문에 list 붙여줘야 함
- reduce
    - list에 똑같은함수를 적용해서 통합

## iterable object

- 내부적 구현으로 __iter__ 와 __next__가 사용됨
- iter()와 next() 함수로 iterable객체를 iterator object로 사용

## generator

- element가 사용되는 시점에 값을 메모리에 반환
    - yield를 사용해 한번에 하나의 element만 반환함
- list comprehension과 유사한 형태로 generator형태의 list 생성
    - generator expression 이라고도 부름
    - []대신 ()를 사용하여 표현

    ```python
    gen_ex = (n*n for n in range(500))
    ```

---

- list 타입의 데이터를 반환해주는 함수는 generator로 만들어라
    - 읽기 쉬운 장점, 중간 과정에서 loop이 중단 될 수 있을 때
- 큰 데이터를 처리할 때는 generator expression을 고려
- 파일 데이터를 처리할 때도 generator를 쓰자

## arguments

1. Keyword arguments
2. Default arguments
3. Variable-length arguments

## variable-length & asterisk

- variable-length
    - **개수가 정해지지 않은 변수**를 함수의 parameter로 사용하는 법
    - 입력된 값은 tuple type으로 사용할 수 있음
- keyword variable-length
    - parameter 이름을 따로 지정하지 않고 입력하는 방법
    - 입력된 값은 dict type으로 사용할 수 있음
- asterisk
    - tuple, dict등 자료혀에들어가 있는 값을 unpacking
    - 함수의 입력값, zip 등에 유용하게 사용가능

# Tips. 단축키

## vscode

- ctrl + shift + v

## Jupyter notebook

- a
- b
- dd
- %timeit 함수
- pprint

## Windows

- ctrl + 좌우 화살표

    단어 단위 이동

- ctrl + shift + 좌우 화살표
- ctrl + shift + 위아래 화살표

## Notion

[https://dololak.tistory.com/603](https://dololak.tistory.com/603)