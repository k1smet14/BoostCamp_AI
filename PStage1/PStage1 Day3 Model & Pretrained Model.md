# PStage1 Day3 Model & Pretrained Model

# Design Model with Pytorch

- Pytoch는 Low-level, Pythonic, Flexibility하다
- Pytorch 모델의 모든 레이어는 nn.Module 클래스를 따른다
- __init__에서 정의한 또다른 nn.Modules

![PStage1%20Day3%20Model%20&%20Pretrained%20Model%204527b08a68314ae5a0618e82e98381f3/Untitled.png](PStage1%20Day3%20Model%20&%20Pretrained%20Model%204527b08a68314ae5a0618e82e98381f3/Untitled.png)

- 모델(모듈)이 호출 되었을 때 forward 함수가 실행된다
- 모든 nn.Module은 forward() 함수를 가진다
    - 내가 정의한 모델의 forward()를 한번만 실행한 것으로 그 모델의 forward에 정의된 모듈 각각의 forward()가 실행된다
- 모델에 정의되어 있는 modules가 가지고 있는 계산에 쓰일 Parameter
    - `sd = a.state_dict()`
    - `params = list(a.parameters())`
- 각 모델 파라미터 들은 data, grad, requires_grad 변수 등을 가지고 있다

![PStage1%20Day3%20Model%20&%20Pretrained%20Model%204527b08a68314ae5a0618e82e98381f3/Untitled%201.png](PStage1%20Day3%20Model%20&%20Pretrained%20Model%204527b08a68314ae5a0618e82e98381f3/Untitled%201.png)

- Pythonic하다는 것의 장점
    - `sd = a.state_dict()` → Python의 Dictionary
    - 이러한 형식과 구조를 미리 알고 있다면, 응용이 가능하고, 에러들도 행들링 할 수 있다

# Pretrained Model

- ImageNet의 등장으로 Computer Vision의 어마어마한 발전이 이루어졌다
- 모델 일반화를 위해 매번 수 많은 이미지를 학습시키는 것은 까다롭고 비효율적
    - 좋은 품질, 대용량의 데이터로 미리 학습한 모델 → 이 모델을 바탕으로 내 목적에 맞게 다듬어서 사용
    - 미리 학습된 좋은 성능이 검증되어 있는 모델을 사용하면 매우 효율적
    - 쉽게 모델 구조와 Pretrained Weight를 다운로드 할 수 있다

## Transfer Learning

- 심플한 구조는 다음과 같다

![PStage1%20Day3%20Model%20&%20Pretrained%20Model%204527b08a68314ae5a0618e82e98381f3/Untitled%202.png](PStage1%20Day3%20Model%20&%20Pretrained%20Model%204527b08a68314ae5a0618e82e98381f3/Untitled%202.png)

- Pretraining 할 때 설정했던 문제와 현재 문제와의 유사성을 고려해야 한다
- 문제를 해결하기 위한 학습 데이터가 충분한지, 유사성이 충분한지에 따라 학습을 변경할 수 있다

![PStage1%20Day3%20Model%20&%20Pretrained%20Model%204527b08a68314ae5a0618e82e98381f3/Untitled%203.png](PStage1%20Day3%20Model%20&%20Pretrained%20Model%204527b08a68314ae5a0618e82e98381f3/Untitled%203.png)

---

# PStage 1 (Image Classification)

- 적절한 Backbone 네트워크를 생각해 보자
- VGGface2로 pretrained된 InceptionResnetV1을 사용해 보았다
    - 얼굴 사진을 train 하였기 때문에 성별, 연령 분류에 도움이 될 것 같다는 생각을 하였다
- 마지막 classifier를 mask dataset의 class인 18개로 분류하는 것으로 바꾸었다
- 학습데이터의 크기가 크지 않기 때문에 네트워크 전체를 학습시키는 finetuning을 하였다
- 배치사이즈는 32로 하였다. ← 64의 경우 메모리가 감당하지 못함
- optimizer와 hyper parameter는 pretraining에 사용한 것을 그대로 사용하였다
- 약 32epoch를 돌린 결과 (1epoch당 약 6분의 시간이 소요되었다. 총 3시간 정도)
    - val acc : 0.9367
    - test acc : 0.72
    - f1 score : 0.57
- 수렴까지 약 20epochs면 충분할 것 같다
- 다들 val acc 보다 20%정도 낮게 나온다고 한다
- Overfitting이 심하게 된것 같다 → test dataset의 질이 편차가 심하다고 한다
- train set도 라벨이 잘못 정해진 경우도 존재한다고 한다 → train set을 고쳐야 할 수도?
- pretrained 데이터 셋은 서양인이고 컴피티션 데이터셋은 모두 아시아인이다
    - 나이대를 예측하는데에 pretrained 데이터가 별로 유용하지 못한것 같다
- 나이대를 예측하는 task가 이번 컴피티션의 큰 격차가 나는 부분인것 같다
- efficientnet을 활용 해야겠다
- 현재 모델은 나중에 앙상블할때 사용해 보아야겠다