# PStage1 Day4 Training & Inference

- 학습 프로세스에 필요한 요소를 크게 Loss, Optimizer, Metric으로 나눌 수 있다

## Loss

- Loss 함수 = Cost 함수 = Error 함수
- Loss도 사실 nn.Module 이다  nn.Loss

![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled.png)

- loss.backward()가 실행되면 모델의 파라미터의 grad 값이 업데이트 된다
    - required_grad=False 는 제외
- Focal Loss
    - Class Imbalance 문제가 있는 경우, 맞춘 확률이 높은 Class는 조금의 loss를, 맞춘 확률이 낮은 Class는 Loss를 훨씬 높게 부여한다
- Label Smoothing Loss
    - Class target label을 Onehot 표현으로 사용하기 보다는 Soft하게 표현해서 일반화 성능을 높인다
    - ex) [0.025, 0.9, 0.025, 0.025, ...]

## Optimizer

- 영리하게 움직일 수록 수렴은 빨라진다

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%201.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%201.png)

- 학습 시에 Learning rate를 동적으로 조절할 수는 없을까? → LR scheduler
- StepLR
    - 특정 Step마다 LR 감소

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%202.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%202.png)

- CosineAnnealingLR
    - Cosine 함수 형태처럼 LR을 급격히 변경

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%203.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%203.png)

- ReduceLROnPlateau
    - 더 이상 성능 향상이 없을 때 LR 감소

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%204.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%204.png)

     

## Metric

- 모델의 평가
    - 학습된 모델을 객관적으로 평가할 수 있는 지표가 필요
    - Classification : Accuracy, F1-score, precision, recall ...
    - Regression : MAE, MSE
    - Ranking : MRR, NDCG, MAP
- 데이터 상태에 따라 적절한 Metric을 선택하는 것이 필요
    - Class 별로 밸런스가 적절히 분포 → Accuracy
    - Class 별 밸런스가 좋지 않아서 각 클래스 별로 성능을 잘 낼 수 있는지 확인 필요 → F1-Score

## Training Process

- model.train()

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%205.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%205.png)

- optimizer.zero_grad()

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%206.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%206.png)

- loss = criterion(outputs, labels)

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%207.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%207.png)

- loss를 마지막으로 chain 생성한다

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%208.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%208.png)

- loss의 grad_fn chain → loss.backward()

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%209.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%209.png)

- optimizer.step()

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2010.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2010.png)

- 응용 : Gradient Accumulation

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2011.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2011.png)

## Inference Process

- model.eval()

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2012.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2012.png)

- with torch.no_grad():

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2013.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2013.png)

- 추론 과정에 validation set이 들어가면 그게 검증이다

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2014.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2014.png)

- 체크포인트는 아래와 같이 짤 수 있다

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2015.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2015.png)

- 최종 Submission 스펙을 확인 후 변환하여 제출하면 된다

    ![PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2016.png](PStage1%20Day4%20Training%20&%20Inference%2083ed7c1130814caebbc59d579683fe15/Untitled%2016.png)

## Appendix : Pytorch Lightning

- Keras처럼 코드를 간단화 해주는 것
- 계속 발전 중이다
- 하지만 공부는 Pytorch로 하는 것이 좋다 → 자세한 이해가 가능

---

# PStage 1 (Image Classification)

- batch size는 최대 32인것 같다
- validation set 의 비율은 0.2로 했다
- Loss는 Focal Loss와 Label Smoothing Loss를 적용해봐야 겠다
- F1 Score를 계산해 베스트를 저장하는 방법을 해야겠다
- optimizer 는 Adam을 썼다
    - AdamP가 좋다는 정보가 있다
- Scheduler를 적용해야겠다. → CosineAnnealingLR을 생각중이다
- Metric은 F1 Score를 중점적으로 보려고 한다
- 체크포인트를 저장
- Early Stopping과 Gradient Accumulation을 적용해봐야겠다