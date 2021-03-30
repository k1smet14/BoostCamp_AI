# PStage1 Day1 Image classification & EDA

2021년 3월 29일 학습정리

# EDA(Exploratory Data Analysis)

- 탐색적 데이터 분석 → 데이터를 이해하기 위한 노력이다
- 내가 알고 싶은 내용, 궁금한 내용을 체크 하는 과정
- 도구는 도구일 뿐, 여러 방법으로 접근해보자
- 도중에도 떠오르는 아이디어 때문에 다시 확인하는 과정을 거칠 것이다
- 처음에는 아무렇게나 시작해 보자

# Image Classification

- Image는 시각적 인식을 표현한 인공물
- 이미지를 입력으로 받아 Class를 배정하는 과정
- 베이스라인 코드를 기준으로 점차 확장해 나가자

---

# PStage 1 (Image Classification)

## Overview

COVID-19의 확산으로 전세계적으로 많은 제약이 있다. COVID-19의 위험성은 강한 전염력으로 부터 나온다. 전염을 막기위해 사람들은 마스크를 착용해야 할 필요가 있으며, 무엇보다도 코와 입을 가릴 수 있도록 올바르게 착용해야 한다. 공공장소에서 모든 사람들의 올바른 마스크 착용 상태를 검사해야 할 필요성이 있다.

따라서, 우리는 카메라로 비춰진 사람 얼굴 이미지 만으로 이 사람이 마스크를 쓰고 있는지, 쓰지 않았는지, 정확히 쓴 것이 맞는지 자동으로 가려낼 수 있는 시스템을 개발한다.

## DATA

모든 데이터셋은 아시아인 남녀로 구성되어 있고 나이는 20대부터 70대까지 다양하게 분포

- 전체 사람 명 수 : 4,500
- 한 사람당 사진의 개수: 7 [마스크 착용 5장, 이상하게 착용(코스크, 턱스크) 1장, 미착용 1장]
- 이미지 크기: (384, 512)

60%의 Train set, 20%는 public test set, 그리고 나머지 20%는 private test set

입력값 → 마스크 착용 사진, 미착용 사진, 혹은 이상하게 착용한 사진(코스크, 턱스크 등)

결과값 → 총 18개의 class를 예측해야 한다. 0~17에 해당되는 숫자

**Class Description:** 마스크 착용여부, 성별, 나이를 기준으로 총 18개의 클래스

![PStage1%20Day1%20Image%20classification%20&%20EDA%20d492d8fd3deb43d3afdbfcc80e0431b4/Untitled.png](PStage1%20Day1%20Image%20classification%20&%20EDA%20d492d8fd3deb43d3afdbfcc80e0431b4/Untitled.png)

## DATA Analysis

```python
import seaborn as sns
sns.displot(df, x='age', hue='gender', stat='density', multiple='dodge')
```

![PStage1%20Day1%20Image%20classification%20&%20EDA%20d492d8fd3deb43d3afdbfcc80e0431b4/Untitled%201.png](PStage1%20Day1%20Image%20classification%20&%20EDA%20d492d8fd3deb43d3afdbfcc80e0431b4/Untitled%201.png)

![PStage1%20Day1%20Image%20classification%20&%20EDA%20d492d8fd3deb43d3afdbfcc80e0431b4/Untitled%202.png](PStage1%20Day1%20Image%20classification%20&%20EDA%20d492d8fd3deb43d3afdbfcc80e0431b4/Untitled%202.png)

- 데이터의 불균형이 심한 편이다
    - 여성의 비율이 높다
    - 60대 이상의 비율이 적다
    - 올바른 마스크 데이터가 많다 → 올바른 마스크로 고르면 정답률이 높아진다

    ⇒ 부족한 쪽의 데이터를 augmentation 해볼까?

- 대체로 중앙에 정면을 바라본 사진이 많다
    - 사진은 잘 찍혀져 있는 편

    ⇒ data flip은 안하는게 좋을듯

    ⇒ 중앙을 crop하는 방법을 생각해 볼 수 있을것 같다

    ⇒ 회전을 살짝 넣어줘도 될것 같다

    ⇒ 칼라 변환도 괜찮아 보인다

    ⇒ 밝기 정규화는 필요해 보인다

- 클래스가 연령, 성별, 마스크 착용 상태 세가지로 나눠져 있다

    ⇒ 한번에 클래스 분류를 하는 방법과

    ⇒ 각각의 태스크에 클래스를 분류하고 합치는 방법을 생각해 볼 수 있을것 같다

- 확장자가 3가지 종류가 있다
    - .jpg, .png, .jpeg