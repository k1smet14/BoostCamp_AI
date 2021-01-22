# Day 5 파이썬으로 데이터 다루기

# File / Exception / Log Handling

## Exception Handling

- try ~ except 문법

[Built-in Exception : 기본적으로 제공하는 예외](https://www.notion.so/793156427abf4bfe8c2b47d35ac497af)

- if문 - 로직적인 문제를 다룰때
- exception 사용자가 잘못된 입력을 넣었을때
- 파이썬에서는 try exception 구문을 권장함
- try ~ except ~ else ~ finally
    - else, finally는 별로..
- raise <Exception Type> (예외 정보)
    - 긴 작업을 할 때 오류가 있을 경우, 과감하게 에러를 발생 시킬 수 있다. 리소스 절약을 위해
- assert 예외 조건
    - 역시 리소스 절약을 위해 에러를 발생 시킴

## File Handling

- 기본적인 파일 종류로 text 파일과 binary 파일로 나눔
- 모든 text 파일도 실제로 binary 파일
- 파이썬은 파일 처리를 위해 "open"키워드를 사용함

    ```python
    # read() txt 파일 안에 있는 내용을 문자열로 반환
    f = open("i_have_a_dream.txt", "r")
    contents = f.read() # read() 통채로 읽어옴.
    print(contents)
    f.close()
    ```

    ```python
    # with 구문과 함께 사용하기
    with open("i_have_a_dream.txt", "r") as my_file:
    		contents = my_file.read()
    		print(type(contents), contents)
    ```

    ```python
    with open("i_have_a_dream.txt", "r") as my_file:
    		content_list = my_file.readlines() #파일 전체를 list로 반환
    		print(type(content_list)) #Type 확인
    		print(content_list) #리스트 값 출력
    # readlines() 한 줄씩 읽어서 전체를 리스트 형태로
    ```

    ```python
    with open("i_have_a_dream.txt", "r") as my_file:
    		i = 0
    		while True:
    				line = my_file.readline()
    				if not line:
    						break
    				print (str(i) + " === " + line.replace("\n",	""))
    				i = i + 1
    # readline() 실행 시 마다 한 줄씩 읽어 와서 메모리에 올림. 
    # 데이터 크기가 클 때
    ```

- open. 파일이 있는 곳의 주소를 연결함
- mode는 “w”, encoding=“utf8”

```python
f = open("count_log.txt", 'w', encoding="utf8")
for i in range(1, 11):
		data = "%d번째 줄입니다.\n" % i
		f.write(data)
f.close()
```

- mode는 “a”는 추가 모드
- os 모듈을 사용하여 directory 다루기

```python
if not os.path.isdir("log"):
os.mkdir("log")
```

- 최근에는 pathlib 모듈을 사용하여 path를 객체로 다룸
- os.path.exist('abc') → abc라는 폴더가 존재하는지

```python
# 디렉토리가 있는지, 파일이 있는지 확인 후
import os
if not os.path.isdir("log"):
		os.mkdir("log")
if not os.path.exists("log/count_log.txt"):
		f = open("log/count_log.txt", 'w', encoding="utf8")
		f.write("기록이 시작됩니다\n")
		f.close()
with open("log/count_log.txt", 'a', encoding="utf8") as f:
		import random, datetime
		for i in range(1, 11):
				stamp = str(datetime.datetime.now())
				value = random.random() * 1000000
				log_line = stamp + "\t" + str(value) +"값이 생성되었습니다" + "\n"
				f.write(log_line)
```

## Pickle

- 파이썬의 객체를 영속화하는 built-in 객체
- 데이터, object 등 실행중 정보를 저장 → 불러와서 사용
- 저장해야 하는 정보, 계산 결과 등 활용이 많음

```python
import pickle

f = open("list.pickle", "wb")
test = [1, 2, 3, 4, 5]
pickle.dump(test, f)
f.close()

f = open("list.pickle", "rb")
test_pickle = pickle.load(f)
print(test_pickle)
f.close()
```

## Logging Handling

- 레벨별(개발, 운영)로 기록을 남길 필요도 있음

```python
import logging
logger = logging.getLogger("main")
stream_hander = logging.StreamHandler()
logger.addHandler(stream_hander)

logger.setLevel(logging.DEBUG)
logger.debug("틀렸잖아!")
logger.info("확인해")
logger.warning("조심해!")
logger.error("에러났어!!!")
logger.critical("망했다...")
```

## configparser - 파일에

```python
import configparser
config = configparser.ConfigParser()
config.sections()
config.read('example.cfg')
config.sections()
for key in config['SectionOne']:
		print(key)
config['SectionOne']["status"]
```

## argparser - 실행시점에

```python
import argparse
parser = argparse.ArgumentParser(description='Sum two integers.')
parser.add_argument('-a', "--a_value", dest=”A_value", help="A integers", type=int)
parser.add_argument('-b', "--b_value", dest=”B_value", help="B integers", type=int)
args = parser.parse_args()
print(args)
print(args.a)
print(args.b)
print(args.a + args.b)
```

## Logging examples

```python
logger.info('Open file {0}'.format("customers.csv",))
try:
	with open("customers.csv", "r") as customer_data:
		customer_reader = csv.reader(customer_data, delimiter=',', quotechar='"')
		for customer in customer_reader:
			if customer[10].upper() == "USA": #customer 데이터의 offset 10번째 값
				logger.info('ID {0} added'.format(customer[0],))
				customer_USA_only_list.append(customer) #즉 country 필드가 “USA” 것만
except FileNotFoundError as e:
	logger.error('File NOT found {0}'.format(e,))
```

---

# Python data handling

## Comma Separate Value(CSV)

- CSV 객체 활용

```python
import csv
reader = csv.reader(f, delimiter=',', quotechar='"', quoting=csv.QUOTE_ALL)
# delimiter 글자를 나누는 기준
# lineterminator 줄 바꿈 기준
# quotechar  문자열을 둘러싸는 신호 문자
# quoting 데이터 나누는 기준이 quotechar에 의해 둘러싸인 레벨
```

## Web

- HTML(Hyper Text Markup Language)
    - 웹 상의 정보를 구조적으로 표현하기 위한 언어. Tag를 사용

## 정규식(Regular expression)

- 정규 표현식. 특정한 규칙을 가진 문자열의 집합을 추출
- 문법 자체는 매우 방대, 스스로 찾아서 하는 공부 필요
- 정규식 공부하자!!
- 정규식 연습장 ([http://www.regexr.com/](http://www.regexr.com/))활용
- import re

## XML(eXtensible Markup Language)

- 데이터의 구조와 의미를 설명하는 Tag(Markup)을 사용하여 표시하는 언어
- XML은 컴퓨터(예: PC - 스마트폰)간에 정보를 주고받기 매우 유용한 저장 방식으로 쓰이고 있음
- 정규표현식으로 Parsing이 가능함
- 그러나 좀더 손쉬운 도구(beautifulsoup)으로 파싱

### BeautifulSoup

- HTML, XML등 Markup 언어 Scraping을 위한 대표적인 도구
- 속도는 상대적으로 느리나 간편히 사용할 수 있음
- conda 가상 환경으로 lxml과 beautifulsoup 설치

activate python_mooc
conda install lxml
conda install -c anaconda beautifulsoup4=4.5.1

## JSON(JavaScript Object Notation)

- 간결성으로 기계/인간이 모두 이해하기 편함
- 데이터 용량이 적고, Code로의 전환이 쉬움
- XML의 대체제로 많이 활용
- json 모듈을 사용하여 손 쉽게 파싱 및 저장 가능
- JSON 파일의 구조를 확인 - 읽어온 후 - Dict Type처럼 처리

```python
# JSON Read
{"employees":[
{"firstName":"John", "lastName":"Doe"},
{"firstName":"Anna", "lastName":"Smith"},
{"firstName":"Peter", "lastName":"Jones"}
]}

import json
with open("json_example.json", "r", encoding="utf8") as f:
		contents = f.read()
		json_data = json.loads(contents)
		print(json_data["employees"])
```

```python
# JSON Write
import json
dict_data = {'Name': 'Zara', 'Age': 7, 'Class': 'First'}
with open("data.json", "w") as f:
	json.dump(dict_data, f)
```

# 마스터 클래스 최성철 교수

- 대학원을 가고 싶다면?
    - 수학 - 선대, 미적, 통계학, 수리통계, 해석학, 선대미적분
    - 수학보다 코딩 잘하는 사람 선호
    - Data Structure, OS, Data Base, 모델링, 쿼리 등
- 수학 능력?
    - 논문 읽으면서 내용이 감이 올정도
- 강화학습 적용?
    - offline RL 정형화 되어 있지 않은 환경에서 정형화 하기
- Data Scientist VS Machine Learning Engineer
    - 요즘은 이런 구분 잘 안함. 작은 기업일 수록 더 구분 안함
    - Full stack DL 하면 좋다
- 주식해라
    - 주식 공부하면 세상의 변화를 알 수 있고, 어떤 공부를 해야 하는지 배움.
- 코드 가독성?
    - 좋은 보고서 쓰듯이 해라
- 스파크는 기본적 소양. Hadoop, spark 등
- Django / Flask ?
    - 기초 소양. 서비스 형태로 보여 줄 필요가 있음.
- Auto ML ?
    - 엔지니어라면 파이프라인 잇기. 서비스화 하기 소양 필요
- 대학원?
    - 얼마나 재밌게 다닐 수 있는 곳
- fstring 주로 쓰자
- 대학원은 네트워크를 얻을 수 있는 것이 중요
- 취업을 위해 포트폴리오, 깃헙 준비
- AWS, google cloud? 능력도 중요. 도구사용 스킬은 좋다
- 자기를 믿는 힘이 중요하다!