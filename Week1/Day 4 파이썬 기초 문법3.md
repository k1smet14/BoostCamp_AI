# Day 4 파이썬 기초 문법 3

---

 

# Python Object-Oriented Programming

## Object-Oriented Programming

- 객체 : 속성(Attribute)와 행동(Action)을 가짐
- OOP는 속성은 변수(variable), 행동은 함수(method)로 표현됨
- 설계도에 해당하는 클래스(class)와 실제 구현체인 인스턴스(instance)

## Objects in Python

- Python naming rule
    - snake_case : 파이썬 함수/변수명에 사용
    - CamelCase : 파이썬 Class명에 사용
- Attribute 추가는 __init__, self와 함께!
- __는 특수한 예약 함수나 변수 그리고 함수명 변경(맨글링)으로 사용
    - ex) `__main__ , __add__ , __str__ , __eq__`
- method 는 반드시 **self** 를 추가해야만 class함수로 인정됨
- self → 생성된 instance 자신

## OOP characteristics

1. Inheritance 상속
    - 부모클래스로 부터 속성과 method를 물려받은 자식 클래스 생성
2. Polymorphism 다형성
    - 같은 이름 메소드의 내부 로직을 다르게 작성
    - Dynamic Typing 특성으로 인해 같은 부모클래스의 상속에서 발생
3. Visibility 가시성
    - 객체의 정보를 볼 수 있는 레벨을 조절하는 것
    - 누구나 객체 안에 모든 변수를 볼 필요가 없음
- Encapsulation
    - 캡슐화 또는 정보 은닉(Information Hiding)
    - Class를 설계할 때, 클래스 간 간섭/정보공유의 최소화
    - 캡슐을 던지듯, 인터페이스만 알아서 써야함
- Visibility Example 1

```python
class Product(object):
		pass
class Inventory(object):
		def __init__(self):
				self.__items = []
		def add_new_item(self, product):
				if type(product) == Product:
						self.__items.append(product)
						print("new item added")
				else:
						raise ValueError("Invalid Item")
		def get_number_of_items(self):
				return len(self.__items)

my_inventory = Inventory()
my_inventory.add_new_item(Product())
my_inventory.add_new_item(Product())
print(my_inventory.get_number_of_items())
print(my_inventory.__items)
my_inventory.add_new_item(object)
```

`self.__items` Private 변수로 선언. 타객체가 접근 못함

- Visibility Example 2

```python
class Inventory(object):
		def __init__(self):
				self.__items = []

		@property # property decorator. 숨겨진 변수를 반환하게 해줌
		def items(self):
				return self.__items

my_inventory = Inventory()
my_inventory.add_new_item(Product())
my_inventory.add_new_item(Product())
print(my_inventory.get_number_of_items())

items = my_inventory.items # decorator로 함수를 변수처럼 호출
items.append(Product())
print(my_inventory.get_number_of_items())
```

숨겨진 items에 데코레이터를 이용하여 내부에서 접근하고 반환함.

하지만 이렇게 해도 내부 정보를 변화시킬 수 있기 떄문에 정보를 카피해서 반환하는게 좋다.

## decorate

- first-class objects
    - 변수나 데이터 구조에 할당이 가능한 객체

    ```python
    def square(x):
    		return x * x
    f = square  # 함수를 변수로 사용
    f(5) 
    ```

    - 파라메터로 전달이 가능 + 리턴 값으로 사용

    ```python
    def formula(method, argument_list): # method로 함수를 받음
    		return [method(value) for value in argument_list]
    ```

    - 파이썬의 함수는 일급함수
- inner function
    - 함수 내에 또 다른 함수가 존재

    ```python
    def print_msg(msg):
    		def printer():
    				print(msg)
    		printer()
    print_msg("Hello, Python")
    ```

    - closures : inner function을 return값으로 반환

    ```python
    def print_msg(msg):
    		def printer():
    				print(msg)
    		return printer

    another = print_msg("Hello, Python")
    another()
    ```

    - closure example

    ```python
    def tag_func(tag, text):
    		text = text
    		tag = tag

    		def inner_func():
    				return '<{0}>{1}<{0}>'.format(tag, text)
    		return inner_func

    h1_func = tag_func('title', "This is Python Class")
    p_func = tag_func('p', "Data Academy")
    ```

- decorator
    - 복잡한 클로져 함수를 간단하게!

    ```python
    def star(func):
    		def inner(*args, **kwargs):
    				print("*" * 30)
    				func(*args, **kwargs)
    				print("*" * 30)
    		return inner

    @star
    def printer(msg): # printer함수가 func로. msg가 args로.
    		print(msg)
    printer("Hello")
    ```

    - 좀 더 복잡한 데코레이터

    ```python
    def star(func):
    		def inner(*args, **kwargs):
    				print("*" * 30)
    				func(*args, **kwargs)
    				print("*" * 30)
    		return inner
    def percent(func):
    		def inner(*args, **kwargs):
    				print("%" * 30)
    				func(*args, **kwargs)
    				print("%" * 30)
    		return inner

    @star
    @percent
    def printer(msg):
    print(msg)
    printer("Hello")

    >>>>
    **************************************
    %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
    Hello
    %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
    **************************************
    ```

    - 좀 더 더 복잡한 데코레이터. 데코레이터에 argument가 있을 떄

    ```python
    def generate_power(exponent):
    		def wrapper(f):
    				def inner(*args):
    						result = f(*args)
    						return exponent**result
    				return inner
    		return wrapper

    @generate_power(2)  # 2가 exponent
    def raise_two(n):   # raise_two 함수가 f 로. n이 args로.
    		return n**2

    print(raise_two(7))
    ```

          

          

---

# Module and Package

## 모듈과 패키지

- 모듈들을 모아서 하나의 큰 프로그램을 개발함
- 패키지 : 모듈을 모아놓은 단위, 하나의 프로그램
- 파이썬의 Module == py 파일을 의미

## 모듈

- 모듈 만들기
    1. 같은 폴더에 Module인 .py 파일과 사용하는 .py을 저장한후
    2. import 문을 사용해서 module을 호출
- import 하면 모든 코드가 메모리에 로딩
- pycache → 인터프리터가 기계어로 번역해서 저장해 둠

## namespace

- 모듈 안에는 함수와 클래스 등이 존재 가능
- 필요한 내용만 골라서 호출 할 수 있음
- from과 import 키워드를 사용

```python
# Alias 설정하기 – 모듈명을 별칭으로
import fah_converter as fah 
print(fah.covert_c_to_f(41.6))

# 모듈에서 특정 함수 또는 클래스만 호출하기
from fah_converter import covert_c_to_f
print(covert_c_to_f(41.6)) covert_c_to_f 함수만 호출함

# 모듈에서 모든 함수 또는 클래스를 호출하기
from fah_converter import *
print(covert_c_to_f(41.6)) 전체 호출
```

# Built-in Modules

- 파이썬이 기본 제공하는 라이브러리
- 문자처리, 웹, 수학 등 다양한 모듈이 제공

```python
#난수
import random
print (random.randint (0,100)) # 0~100사이의 정수 난수를 생성
print (random.random()) # 일반적인 난수 생성

#시간
import time
print(time.localtime()) # 현재 시간 출력

#웹
import urllib.request
response = urllib.request.urlopen("http://thetemlab.io")
print(response.read())
```

## 패키지

- 하나의 대형 프로젝트를 만드는 코드의 묶음
- 다양한 모듈들의 합, 폴더로 연결됨
- __init__ , __main__ 등 키워드 파일명이 사용됨
- 다양한 오픈 소스들이 모두 패키지로 관리됨

### 패키지 만들기

1. 기능들을 세부적으로 나눠 폴더로 만듦
2. 각 폴더별로 필요한 모듈을 구현함
3. 폴더별로 __init__.py 구성하기
    - 현재 폴더가 패키지임을 알리는 초기화 스크립트
    - 하위 폴더와 py파일을 모두 포함함
    - import 와 __all__ keyword 사용
4. __main__.py파일 만들기
    - Package내에서 다른 폴더의 모듈을 부를 때

        `from game.graphic.render import render_test()`  절대 참조
        `from .render import render_test()` . 현재 디렉토리 기준
        `from ..sound.echo import echo_test()` .. 부모 디렉토리 기준

5. 실행하기 - 패키지 이름만으로 호출하기

## 가상환경 설정하기

- 프로젝트 진행 시 필요한 패키지만 설치하는 환경
- 기본 인터프리터 + 프로젝트 종류별 패키지 설치
- 다양한 패키지 관리 도구를 사용함
- 대표적으로 virtualenv와 conda가 있음

### conda 가상환경

- `conda create -n my project python=3.8` 가상환경 새로 만들기
- `conda activate my project` 가상환경 호출
- `conda deactivate` 가상환경 해제
- `conda install <패키지명>`

### conda 가상환경 예시

- matplotlib 활용한 그래프 표시
    - 대표적인 파이썬 그래프 관리 패키지
    - 엑셀과 같은 그래프들을 화면에 표시함
    - 다양한 데이터 분석 도구들과 함께 사용됨

    ```python
    import matplotlib.pyplot as plt
    plt.plot([1,2,3,4])
    plt.ylabel('some numbers')
    plt.show()
    ```

- tqdm
    - 프로그램 실행시간, 과정 표시

    ```python
    from tqdm import tqdm
    import time
    for i in tqdm(range(100000)):
    		if i % 1000 == 0:
    				time.sleep(1)
    ```