# Day 2 파이썬 기초 문법

# Variable & List

## Variable

- 프로그래밍에서는 변수는 값을 저장하는 장소
- 변수는 메모리 주소를 가지고 있고 변수에 들어가는 값은 메모리 주소에 할당됨
- 변수 이름 작명법
    - 알파벳, 숫자, 언더스코어로 선언 가능
    - 의미 있는 단어로 표기
    - 대소문자 구분
    - 예약어는 쓰지 않는다.

## Dynamic Typing

- 코드 실행시점에 데이터의 Type을 결정하는 방법
- 실수형에서 정수형으로 형 변환 시 소수점 이하 내림
- 문자간에도 + 연산이 가능 → concatenate

## List

- 인덱싱 indexing
- 슬라이싱 slicing
- 리스트 연산
- 추가 삭제
- 메모리 저장 방식
- 패킹과 언팩킹
- 이차원 리스트

# Function and Console I/O

## function

- 어떤 일을 수행하는 코드의 덩어리
- 코드를 논리적인 단위로 분리
- 함수 부분을 제외한 메인프로그램부터 시작
- parameter : 함수의 입력 값 인터페이스
- argument : 실제 parameter에 대입된 값

## print formatting

1. % string
2. format 함수
3. fstring
    - python 3.6이후 PEP498에 근거한 formatting 기법
    - print(f'{name:*<20}')
    - print(f'{name:*^20}')   →  중앙정렬

# Conditionals and Loops

## condition

- x == y    →   x와 y의 값이 같은지 검사
- x is y    →    x와 y의 메모리 주소가 같은지 검사
- 삼항 연산자(Ternary operators) → True if value % 2 == 0 else False

## loop

- 반복문은 반복 시작 조건, 종료 조건, 수행 명령으로 구성됨
- 반복의 제어 else
    - 반복 조건이 만족하지 않을 경우 반복 종료 시 1회 수행
    - break로 종료된 반복문은 else block이 수행되지 않음

## debugging

- 문법적 에러를 찾기 위한 에러 메시지 분석
- 논리적 에러를 찾기 위한 테스트 → 중간 중간 프린터 문을 찍어서 확인
- 모든 문제는 google + stack overflow로 해결 가능

# String and advanced function concept

## string

- UTF-8을 많이 씀
- 인덱싱
- 슬라이싱
- 문자열 함수

    len(a)

    a.upper()

    a.lower()

    a.capitalize()

    a.title()

    a.count('abc')

    a.find('abc')

    a.startswith('abc')

    a.endswith('abc')

    a.strip()

    a.split()

    a.isdigit()

    a.islower()

    a.isupper()

## call by object reference

- 함수에서 parameter를 전달하는 방식
    1. Call by Value
    2. Call by Reference
    3. Call by Object Reference 

         → 파이썬은 객체의주소가 함수로 전달되는 방식

## function type hints

- 파이썬의 가장 큰 특징 - dynamic typing
- python 3.5 이후 type hints 기능 제공

    def do_function(var_name: var_type) -> return_type:

    pass

- Type hints의 장점
    1. 인터페이스를 명확히 알려줄 수 있다.
    2. 함수의 문서화시 parameter에 대한 정보를 명확히 알 수 있다.
    3. 코드의 발생 가능한 오류를 사전에 확인
    4. 시스템 전체적인 안정성 확보

## docstring

- 3개의 따옴표로 docstring 영역 표시(함수명 아래)
- black 모듈을 활용하여(black codename.py) 간단하게 변환 가능

## How to write Good Code

- 사람의 이해를 돕기 위해 규칙이 필요함
- 그 규칙을 코딩 컨벤션이라고 함
- 중요한 건 일관성!
- 읽기 좋은 코드가 좋은 코드

# 피어 섹션

Call by Value와 Call by Reference에 관한 질문이 나왔다.

-5 + 256 의 자주 쓰는 숫자는 메모리에 고정 되어 있다.

파이썬의 메모리 관련 내용은 아래를 참조

[https://leemoney93.tistory.com/25](https://leemoney93.tistory.com/25)

# Tips. 단축키

## vscode

- ctrl + shift + p

    명령 파레트

- ctrl + n

    새 페이지

- ctrl + s

    저장

- ctrl + /

    주석

## jupyter notebook

- ctrl + shift + -

    코드 분리하기

- shift + m

    코드 병합하기