# PStage1 Day6 Introduction to Visualization

- 데이터 시각화란 데이터를 그래픽 요소로 매핑하여 시각적으로 표현하는 것
- 시각화는 다양한 요소가 포함된 Task
    - 목적
    - 독자
    - 데이터
    - 스토리
    - 방법
    - 디자인
- 시각화는 정답이 없다
- 하지만 지금까지 연구되고 사용된 시각화 모범 사례를 통해 좋은 시각화를 만들 수 있다
- 시각화 관련 참고 자료
    - Visualization Analysis & Design
    - Fundamentals of Data Visualization

## 데이터 이해하기

- 데이터 시각화를 위해서는 데이터가 우선적으로 필요
- 시각화를 진행할 데이터
    1. 데이터셋 관점(global)
    2. 개별 데이터의 관점(local)
- 데이터셋의 종류
    - 정형 데이터
        - 테이블 형태로 제공되는 데이터, 일반적으로 csv,, tsv 파일로 제공
        - Row가 데이터 1개 item
        - Column은 attribute(feature)
        - 가장 쉽게 시각화할 수 있는 데이터셋
            - 통계적 특성과 feature 사이 관계, 데이터 간 관계, 데이터 간 비교 등
    - 시계열 데이터
        - 시간 흐름에 따른 데이터를 Time-Series라고 한다
        - 기온, 주가 등 정형데이터와 음성, 비디오와 같은 비정형 데이터가 존재한다
        - 시간 흐름에 따른 추세(Trend), 계절성(Seasonality), 주기성(Cycle) 등을 살펴본다
    - 지리 데이터
        - 지도 정보와 보고자 하는 정보 간의 조화가 중요함
        - 지도 정보를 단순화 시키는 경우도 존재
        - 거리, 경로, 분포 등 다양한 실사용
    - 관계형(네트워크) 데이터
        - 객체와 객체 간의 관계를 시각화
            - Graph visualization / Network Visualization
        - 객체는 Node로, 관계는 Link로 표현
        - 크기, 색, 수 등으로 객체와 관계의 가중치를 표현
        - 휴리스틱하게 노드 배치를 구성
    - 계층적 데이터
        - 관계 중에서도 포함관계가 분명한 데이터
            - 네트워크 시각화로도 표현 가능
        - Tree, Treemap, Sunburst 등이 대표적
    - 다양한 비정형 데이터
- 데이터의 종류
    - 데이터의 종류는 다양하게 분류 가능
    - 대표적으로 4가지로 분류
        - 수치형(numerical)
            - 연속형(continuous) : 길이, 무게, 온도 등
            - 이산형(discrete) : 주사위 눈금, 사람 수 등
        - 범주형(categorical)
            - 명목형(norminal) : 혈액형, 종교 등
            - 순서형(ordinal) : 학년, 별점, 등급 등

## 시각화 이해하기

- 마크와 채널
    - mark : 점, 선, 면으로 이루어진 데이터 시각화

        ![PStage1%20Day6%20Introduction%20to%20Visualization%20dd56e41be06249a78cf3c2f05bc239f2/Untitled.png](PStage1%20Day6%20Introduction%20to%20Visualization%20dd56e41be06249a78cf3c2f05bc239f2/Untitled.png)

    - channel : 각 마크를 변경할 수 있는 요소들

        ![PStage1%20Day6%20Introduction%20to%20Visualization%20dd56e41be06249a78cf3c2f05bc239f2/Untitled%201.png](PStage1%20Day6%20Introduction%20to%20Visualization%20dd56e41be06249a78cf3c2f05bc239f2/Untitled%201.png)

- 전주의적 속성
    - Pre-attentive Attribute
    - 주의를 주지 않아도 인지하게 되는 요소
        - 시각적으로 다양한 전주의적 속성이 존재
    - 동시에 사용하면 인지하기 어려움
        - 적절하게 사용할 때, 시각적 분리(visual popout)

    ![PStage1%20Day6%20Introduction%20to%20Visualization%20dd56e41be06249a78cf3c2f05bc239f2/Untitled%202.png](PStage1%20Day6%20Introduction%20to%20Visualization%20dd56e41be06249a78cf3c2f05bc239f2/Untitled%202.png)

---

# PStage 1 (Image Classification)

- label smoothing이랑 distillation을 같이하면 성능이 안좋다는 논문
- cutmix가 기본으로 자주 사용된다
- RAS 성능 차이가 크다
- k-fold 구현을 하였다
- Dataset을 건드리면 성능이 불안정하다
- 58세 이상은 60세 이상으로 분류하도록 age filter를 걸었다 → acc는 떨어졌지만 f1 score가 올라갔다