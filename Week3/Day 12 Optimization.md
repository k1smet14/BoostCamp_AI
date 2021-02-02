# Day 12 Optimization

# Optimization

### Generalization

- Generalization gap = Test error와 Training error의 차이

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled.png)

### Underfitting vs. Overfitting

### Cross-validation

- k-fold validation : k개로 학습 데이터를 파티션하고 한 부분을 validation으로, 나머지는 train 함. validation을 바꿔가며 학습함
- cross-validation을 통해 최적의 hyper parameter(lr, loss, 네트워크 크기 등)를 학습함. 다음에 hyper parameter를 고정하고 모든 train data를 사용해 모델을 학습
- test data는 학습에는 절대 사용하지 말아야.

### Bias and Variance

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%201.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%201.png)

- Bais and Variance Tradeoff
    - 학습 데이터에 노이즈가 껴 있을 때 bias, variance, noise 세가지를 minimize 해야함. 하지만 하나가 커

### Bootstrapping

- Bootstrapping is any test or metric that uses random sampling with replacement
- 학습 데이터의 일부, 일부를 이용해 여러 모델을 만들어 해결 하겠다.

### Bagging(Bootstrapping aggregating)

- bootstrapping으로 여러 모델을 만들고, 여러 모델들을 가지고 아웃풋을 voting, 평균 등 하겠다. ~ 앙상블

### Boosting

- 간단한 모델을 만들고, 이 모델에 잘 안되는 데이터에 대해서만 새로 모델을 만듦. 이렇게 여러 모델들을 모아 하나의 strong 모델을 만들어 해결

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%202.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%202.png)

# Practical Gradient Descent Methods

- Stochastic gradient descent -  single sample
- Mini-batch gradient descent -  subset of data
- Batch gradient descent - whole data

### Batch-size Matters

- large batch 를 사용하면 sharp minimizers에 도달
- small batch 를 사용하면 flat minimizers
- flat minimizer는 testing function이 training function에서 조금 멀어져도 generalization performance가 높다

# Gradient Descent Methods

### Gradient Descent

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%203.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%203.png)

### Momentum 관성

- 이전 배치에서의 그라디언트 정보를 활용(관성)

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%204.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%204.png)

### NAG(Nesterov Accelerated Gradient)

- 현재 정보로 한번 이동해 보고 그 곳에서 그라디언트를 계산해서 진행

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%205.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%205.png)

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%206.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%206.png)

- local minimum에서 수속하지 못하는 momentum의 단점을 보완

### Adagrad

- 많이 변한 parameter에 대해서는 적게 변화 시키고, 적게 변한 parameter에 대해서는 크게 변화 시키는 방법

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%207.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%207.png)

- G가 계속 커져서 뒤로 갈수록 학습이 멈춤

### Adadelta

- 현재 time step t에 대해서 현재 윈도우 사이즈의 gradient squares의 변화를 본다

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%208.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%208.png)

- G의 데이터가 커지는 것을 막기 위해 Exponential Moving Average를 사용 $\gamma$
- learning rate가 없다

### RMSprop

- $\eta$ 라는 stepsize를 추가

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%209.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%209.png)

### Adam (Adaptive Moment Estimation)

- 일반적으로 가장 잘 된다고 함
- EMA of gradient squares(Adaptive), Momentum을 섞은 것

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2010.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2010.png)

- Momentum, gradient squares, $\eta$, $\epsilon$.   4가지의 파라미터

# Regularization

- 학습에 반대되는 규제를 거는 것
- 학습 데이터에만 잘 동작하는 것이 아니라 테스트 데이터에도 잘 동작하기 위한 수법
- 도구로써 뉴럴네트워크에 하나씩 적용해 보는 것

### Early Stopping

- Validation error를 활용하여 모델을 평가해보고 loss가 커지기 시작할 가능성이 높은 곳에서 멈추는 것

### Parameter Norm Penalty

- 네트워크 파라미터가 너무 커지지 않게 하는 것. Weight의 크기가 작을수록 좋게
- 함수를 부드러운 함수 일수록 Generalization performance가 좋을것 이라는 가정하에.

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2011.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2011.png)

### Data Augmentation

- 데이터가 많을 수록 좋다
- 주어진 데이터를 조작하여 데이터를 늘리는 작업
- label이 바뀌지 않는 한도 내에서 augmentation 해야 함

### Noise Robustness

- 입력데이터와 네트워크의 weight에 noise를 넣음
- 왜 잘되는지는 의문

### Label Smoothing

- 학습 데이터 2개를 섞는 방법

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2012.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2012.png)

- 성능이 오르지만 왜 잘 되는 지는...
- 코드가 간단해서 들인 노력 대비 성능 향상이 좋다. 꼭 써 보길..

### Dropout

- 뉴런 일부를 꺼 주는것
- 각각의 Neuron들이 Robust 한 Feature을 잡아 낼 수 있다는 해석

### Batch Normalization

- 적용하고자 하는 layer의 statistics를 정규화 시키는것

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2013.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2013.png)

---

# CNN 첫걸음

## convolution 연산 이해하기

- 다층신경망(MLP)는 각 뉴런들이 선형모델과 활성함수로 모두 연결된(fully connected) 구조

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2014.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2014.png)

- Convolution 연산은 커널을 입력벡터 상에서 움직여가면서 선형모델과 합성함수가 적용되는 구조

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2015.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2015.png)

- Convolution 연산의 수학적 의미는 신호(signal)를 커널을 이용해 국소적으로 증폭 또는 감소시켜서 정보를 추출 또는 필터링 하는 것

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2016.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2016.png)

- Convolution 연산은 1차원 뿐만 아니라 다양한 차원에서 계산 가능

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2017.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2017.png)

- 3차원 Conv의 경우 2차원 Conv를 3번 적용한다고 생각하자
- 텐서를 직육면체 블록으로 이해하면 됨

## Convolution 연산의 역전파 이해하기

- Conv 연산은 커널이 모든 입력데이터에 공통으로 적용되기 때문에 역전파를 계산할 때도 conv 연산이 나오게 됨

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2018.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2018.png)

- $o_1$에는 $x_3$와 $w_3$가 곱해져서 $o_1$에 적용되었기 때문에,  $\delta_1$과 $w_3$가 곱해져서 $x_3$에 전달
- $o_2$의 경우 $\delta_2$와  $w_2$가 곱해져서 $x_3$에 전달
- $o_3$의 경우 $\delta_3$와  $w_1$가 곱해져서 $x_3$에 전달

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2019.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2019.png)

- $o_3$에서 $x_3$에 대해서  $w_1$을 통해 gradient를 전달했기 때문에,  $w_1$을 통해서 전달 된 gradient인 d3는 $x_3$을 곱해서 w1에 graident 가 전달됨
- $o_2$, $o_1$의 경우도 이하 그림과 같음

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2020.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2020.png)

- 각각의 커널들은 $x_3$ 뿐만 아니라 다른 입력에서도 적용됐기 때문에, 다른 입력에 대해서도 똑같이 gradient가 전달된다
- 각 커널에 들어오는 그레디언트를 더하면 convolution연산과 같다

![Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2021.png](Day%2012%20Optimization%205c7a1af3a7c145bf84bec4fdb45862b0/Untitled%2021.png)

# Appendix

- 이동평균

[https://angrypig7.github.io/misc/2018/07/04/이동평균(Moving-Average)/](https://angrypig7.github.io/misc/2018/07/04/%EC%9D%B4%EB%8F%99%ED%8F%89%EA%B7%A0(Moving-Average)/)

- 분산과 표준편차를 구할 때 n이 아닌 n-1로 나누는 이유는?

[https://bkshin.tistory.com/entry/ㅇ?category=1042793](https://bkshin.tistory.com/entry/%E3%85%87?category=1042793)

- 심슨의 역설

[https://brunch.co.kr/@leoyang99/4](https://brunch.co.kr/@leoyang99/4)

- 데카르트 곱, cartesian product

[https://velog.io/@davkim1030/Python-순열-조합-product-itertools](https://velog.io/@davkim1030/Python-%EC%88%9C%EC%97%B4-%EC%A1%B0%ED%95%A9-product-itertools)