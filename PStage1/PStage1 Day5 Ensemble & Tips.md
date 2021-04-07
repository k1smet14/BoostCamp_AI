# PStage1 Day5 Ensemble & Tips

## Ensemble

- 싱글 모델보다 더 나은 성능을 위해 서로 다른 여러 학습 모델을 사용하는 것
- Model Averaging(Voting)
    - 여러 모델의 결과를 voting해서 하나의 결과를 얻어내는 것

    ![PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled.png](PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled.png)

- Cross Validation
    - 검증 셋을 학습에 활용하는 방법
    - Stratified K-Fold Cross Validation
        - 가능한 경우를 모두 고려하면서 Split시에 Class 분포까지 고려하는 방법

        ![PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%201.png](PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%201.png)

- TTA(Test Time Augmentation)
    - 테스트 이미지를 Augmentation 후 모델 추론, 출력된 여러가지 결과를 앙상블

    ![PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%202.png](PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%202.png)

- 앙상블 효과는 확실히 있지만 그 만큼 학습, 추론 시간이 배로 소모됨

## Hyperparameter Optimization

- 시스템의 매커니즘에 영향을 주는 주요한 파라미터
    - Learning rate
    - Batch size
    - Hidden Layer 갯수
    - Loss 파라미터
    - Optimizer 파라미터
    - k-fold
    - Dropout
    - Regularization
- Optuna
    - 파라미터 범위를 주고 그 범위 안에서 trials 만큼 시행

    ```python
    import optuna

    def objective(trial):
    		x = trial.suggest_uniform('x', -10, 10)
    		return (x - 2) ** 2

    study = optuna.create_study()
    study.optimize(objective, n_trials=100)

    study.best_params
    ```

## Training Visualization

- Tensorboard
    - 학습 과정을 기록하고 트래킹 하는 것도 중요

    ![PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%203.png](PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%203.png)

    ![PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%204.png](PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%204.png)

    - 사용법

        ![PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%205.png](PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%205.png)

- Weight and Bias(wandb)
    - 딥러닝 로그의 깃허브 같은 느낌
    - wandb login

        ![PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%206.png](PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%206.png)

    - wandb init, log 설정

        ![PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%207.png](PStage1%20Day5%20Ensemble%20&%20Tips%20fdff5d6ec2ce475bb427204cb0f4ef35/Untitled%207.png)

## Machine Learning Project

- Jupyter Notebook
    - 코드를 아주 빠르게 cell 단위로 실행해 볼 수 있는 것이 장점
    - 보통 EDA를 할 때 사용하면 매우 편리
    - 학습 진행 도중 노트북 창이 꺼지면 못 돌아감
- Python IDLE
    - 구현은 한번만, 사용은 언제든, 간편한 코드 재사용
    - 강력한 디버깅 기능
    - 자유로운 실험 핸들링

## Tips

- 분석 코드 보다는 설명글을 유심히 보자 → 필자가 생각하고 있는 흐름을 읽을 수 있다
- 코드를 볼 때는 디테일한 부분까지 → 언제든 활용할 수 있을 정도로
- Paper with Codes 사이트 → 최신 논문과 그 코드까지
- 공유하는 것을 주저하지 말자 → 새로운 배움의 기회가 될 수 있다

---

# PStage 1 (Image Classification)

- 마지막즈음에 여러 모델들의 앙상블을 해볼 생각이다. 최소 5개 이상은 되어야 하지 않을까?
- Soft voting이 좋아보인다
- K-fold cross validation을 적용해야 겠다. 5-fold를 사용할 예정이다
- TTA도 적용해볼만 하다
- Tensorboard, wandb의 사용법을 숙지하고 적용할 예정
- 이제부터 py파일로 모듈화해서 VScode 상에서 작업할 예정