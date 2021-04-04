# PStage1 Day2 Dataset & Data Generation

# Dataset

## Pre-processing

- Data Science는 80%의 Pre-processing과 20% Model, Etc..로 볼 수 있다
- 보통 경진대회용 데이터는 그 품질이 매우 양호한 편
- 데이터는 필요 이상으로 많은 정보를 가지고 있기도 한다 → Bounding box를 이용해 볼 수 있다
- 계산의 효율을 위해 적당한 크기로 Resize 한다
- 도메인, 데이터 형식에 따라 정말 다양한 case가 존재한다

## Generalization

- High Bias(Underfitting) or High Variance(Overfitting)이 일어날 수 있다
- Train Set중 일정 부분을 따로 분리, Validation Set으로 활용
- 주어진 데이터가 가질 수 있는 Case(경우), State(상태)의 다양성
    - 밤에 찍힌 사진이어서 어두운 경우, 폭우가 내리는 경우 등등
    - 문제가 만들어진 배경과 모델의 쓰임새를 살펴보면 힌트를 얻을 수 있다
- Torchvision.transforms
    - 종류는 많고, 사용은 간편하다

    ![PStage1%20Day2%20Dataset%20&%20Data%20Generation%20dcce02d2a46f4a39a0b74c09d73a6002/Untitled.png](PStage1%20Day2%20Dataset%20&%20Data%20Generation%20dcce02d2a46f4a39a0b74c09d73a6002/Untitled.png)

- Albumentations
    - 더 빠르고, 더 다양하다

- 이러한 함수들은 도구 가운데 하나일 뿐
- 무조건 적용 가능한 마스터키는 존재하지 않는다
- 앞서 정의한 problem을 깊이 관찰해서 어떤 기법을 적용하면 좋을지 가정하고 실험으로 증명해야 한다

# Data Generation

- Data feeding을 할때, 다음과 같은 경우를 고려하여 설계해야 한다

    ![PStage1%20Day2%20Dataset%20&%20Data%20Generation%20dcce02d2a46f4a39a0b74c09d73a6002/Untitled%201.png](PStage1%20Day2%20Dataset%20&%20Data%20Generation%20dcce02d2a46f4a39a0b74c09d73a6002/Untitled%201.png)

    - Model이 빠르더라도 데이터 생성이 느리면 최대 효율을 낼 수 없다
    - 예를 들어, Resize를 크게 하고 Rotation을 하는 경우와, Rotation을 하고 Resize를 크게 해주는 경우
        - 전자의 경우가 더 느리다 ← 계산량이 많아진다

## torch.utils.data

- Datasets
    - Vanilla Data를 Dataset으로 변환
    - Dataset의 구조

        ![PStage1%20Day2%20Dataset%20&%20Data%20Generation%20dcce02d2a46f4a39a0b74c09d73a6002/Untitled%202.png](PStage1%20Day2%20Dataset%20&%20Data%20Generation%20dcce02d2a46f4a39a0b74c09d73a6002/Untitled%202.png)

- DataLoader
    - 내가 만든 Dataset을 효율적으로 사용할 수 있도록 관련 기능 추가

    ![PStage1%20Day2%20Dataset%20&%20Data%20Generation%20dcce02d2a46f4a39a0b74c09d73a6002/Untitled%203.png](PStage1%20Day2%20Dataset%20&%20Data%20Generation%20dcce02d2a46f4a39a0b74c09d73a6002/Untitled%203.png)

    - 기능이 매우 많다

    ![PStage1%20Day2%20Dataset%20&%20Data%20Generation%20dcce02d2a46f4a39a0b74c09d73a6002/Untitled%204.png](PStage1%20Day2%20Dataset%20&%20Data%20Generation%20dcce02d2a46f4a39a0b74c09d73a6002/Untitled%204.png)

- Dataset과 DataLoader는 엄영히 하는 일이 다르므로 분리되는 것이 좋다

---

# PStage 1 (Image Classification)

- Data Augmentation
    - Horizontal Flip 을 하자
    - Vertical Flip은 좋지 않을것 같다
    - Crop → Random crop보다는 Center crop하는 것이 좋아보인다
    - 약간의 각도를 Rotation 해보자 → shift scale rotate
    - Brightenss, Contrast를 랜덤하게 변화시키자
    - Gaussian Noise를 첨가해보자
    - Albumentation을 적극 활용해보자
    - 데이터 불균형을 augmentation으로 채워보는 것도 괜찮을것 같다 → 구현이 조금 어려울지도?