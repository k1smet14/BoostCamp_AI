# Day 1 파이썬/AI 개발환경 준비하기

# **BASIC COMPUTER CLASS FOR NEWBIES**

- Operating System
    - 우리의 프로그램이 동작할 수 있는 구동 환경
    - Software + Hardware
    - 프로그램은 OS에 dependent함. OS에 맞춰 개발 필요
- 파일시스템
    - OS에서 파일을 저장하는 트리구조 저장 체계
    - 파일 : 컴퓨터 등의 기기에서 의미 있는 정보를 담는 논리적인 단위
    - Directory & File
    - 경로는 컴퓨터 파일의 고유한 위치, 트리구조상 노드의 연결을 의미
    - 절대경로(루트 디렉토리부터 파일위치까지의 경로)와 상대경로(현재 있는 디렉토리부터 타깃 파일까지의 경로)
- 터미널
    - mouse가 아닌 키보드로 명령을 입력 프로그램 실행
    - Graphical User Interface   VS   Command Line Interface
    - Console = Terminal = CMD창
    - cmder 예쁘고 리눅스 커맨드 활용할 수 있다.
    - copy ../../abc.txt ./
    - Shell 명령어들 : cd clear cp rm ls

# **파이썬 개요**

- 플랫폼 독립적인 언어
    - OS에 상관없이 한번 프로그램을 작성하면 사용가능
- 인터프리터(interpreted) 언어
    - 소스코드를 바로 실행할 수 있게 지원하는 프로그램 실행 방법
- 컴파일러 vs 인터프리터
- 객체 지향적 언어
    - 실행 순서가 아닌 단위 모듈(객체) 중심으로 프로그램을 작성
    하나의 객체는 어떤 목적을 달성하기 위한 행동(method)와 속성(attribute)을 가지고 있음
- 동적 타이핑 언어
    - 프로그램이 실행하는 시점에 프로그램이 사용해야할 데이터에 대한 타입을 결정함
- 쉽고, 라이브러리가 다양하고, 널리 쓰이고 있다. Life is short. You need Python.

# **파이썬 코딩환경**

- 프로그램을 작성하고, 실행시키는 환경. <코딩 환경>이라고 부름.
- 개발 환경을 결정
    1. 운영체제
    2. Python Interpreter
    3. 코드 편집기(Editer)
- Python
    - Miniconda & VScode
- Data analysis + Deep learning
    - Jupyter Notebook & COLAB
- 환경설정을 해주어야 어디서든 사용 가능

# **번외.  리눅스 터미널 활용하기**

- 리눅스 명령어 나열하기
    - —help, man

        도움말 보기

    - touch

        빈 파일 생성. + 다른 명령

    - cp

        카피

    - ls, ls -l, ls -al

        현재 디렉토리의 파일 목록을 출력

    - pwd

        현재 위치하고 있는 디렉토리 출력

    - cd

        디렉토리 변경. 절대경로와 상대경로의 차이 이해할 필요가 있다.

    - rm

        파일삭제. rm 파일명. rm -r 디렉토리명

    - sudo

        super user do

    - apt, brew

        패키지 매니저

    - mv

        파일 이동

    - wget

        어드레스를 통해 다운로드 하기

    - git clone

        깃 복제하기

    - ;

        세미콜론으로 명령어 구분. 순차적 실행

    - cat

        cat 파일명. 파일 내용 출력

    - grep

        grep 검색어 파일명. 검색어가 포함되어 있는 행 출력

    - |

        파이프라인.  앞의 출력을 뒤의 입력으로 연결. ex)  ls —help  |  grep sort  |  grep linux

    - ps aux

        실행중인 프로그래 출력

    - >

        redirection. 모니터 출력(standard out)을 1> 뒤(파일)에 보냄.

        2> 는 standard error 를 redirection함. rm rename.txt 1> result.txt 2> error.log

    - <

        standard input을 redirection. cat < hello.txt

    - >>

        덮어쓰기가 아닌 append하기.

    - ./

        ./파일명. 프로그램 실행할때.

    쉘 스크립트 까지의 내용.