# Day 14 Recurrent Neural Network

# RNN 첫걸음

## 시퀀스 데이터 이해하기

- 시퀀스 데이터는 독립동등분포(Independent and identically distributed(i.i.d.))  가정을 잘 위배하기 때문에 순서를 바꾸거나 과거 정보에 손실이 발생하면 데이터의 확률분포도 바뀌게 된다
- 이전 시퀀스의 정보를 가지고 앞으로 발생할 데이터의 확률분포를 다루기 위해 조건부확률을 이용할 수 있다
    - $\begin{aligned}
    P(X_1,\cdots,X_t) &= P(X_t\mid X_1,\cdots,X_{t-1}P(X_1,\cdots,X_{t-1}) \\
    &=\prod^t_{s=1}P(X_s\mid X_{s-1},\cdots,X_1)
    \end{aligned}$
    - $X_t \sim P(X_t\mid X_{t-1},\cdots,X_1)$
    - $X_{t+1} \sim P(X{t+1} \mid X_t, X_{t-1}, \cdots, X_1)$
- 시퀀스 데이터를 다루기 위해선 길이가 가변적인 데이터를 다룰 수 있는 모델이 필요
- 과거 몇개의 정보만 필요할 경우, 고정된 길이 $\tau$만큼의 시퀀스만 사용하는 방법을 AR($\tau$) 자기회귀모델(Autoregerssive Model)이라고 부름
    - $\tau$ 를 정해줘야 하는 하이퍼 파라미터 이기 떄문에 사전지식이 필요, 문제에 따라서 $\tau$ 가 변해야 할 수도 있다
- Latent autoregressive model : 잠재자기회귀모델
    - 직전정보와 그 이전의 정보를 각각 $X_{t-1}$와 잠재변수$H_{t}$로 묶어, 두가지 데이터만 가지고 미래시점을 예측
    - 가변 길이를 고정된 길이로 바꿔 모델링 할 수 있게 된다.

        ![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled.png)

    - 과거의 변수들의 잠재변수들을 어떻게 인코딩할지? → RNN으로

![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%201.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%201.png)

## RNN 이해하기

- 가장 기본적인 RNN모형은 MLP와 유사하다

![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%202.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%202.png)

- RNN은 이전 순서의 잠재변수와 현재의 입력을 활용하여 모델링
- 잠재변수인 $H_t$를 복제해서 다음 순서의 잠재변수를 인코딩하는데 사용
- t에 따라 변하는 것은 잠재변수와 입력데이터이고, 3개의 가중치 행렬은 변하지 않는다
- RNN의 역전파는 잠재변수의 연결그래프에 따라 순차적으로 계산

![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%203.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%203.png)

- 다음 시점의 잠재변수에서 들어오는 gradient 벡터와 출력에서 들어오는 gradient 벡터를 입력과 그 전 잠재변수에 전달하게 된다

## BPTT 이해하기

- BPTT를 통해 RNN의 가중치 행렬의 미분을 계산하면 아래와 같이 미분의 곱으로 이루어진 항이 계산된다

![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%204.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%204.png)

- BPTT를 모든 항에 적용해주면 graident가 불안정해진다
    - 1보다 큰 값이면 매우 커지게 되고
    - 1보다 작은 값이면 매우 작아지게 된다
- 특히 주의해야 하는 것은 gradient가 0으로 주는 것(vanishing)
    - 과거 시점으로 갈수록 정보를 유실할 확률이 높다
- 이 문제를 해결 하기 위해 길이를 끊어 주는 방법을 truncated BTPP

![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%205.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%205.png)

- Vainilla RNN 길이가 긴 시퀀스를 처리하는데 문제가 있기 떄문에 오늘날에는 쓰지 않는다
- 이를 해결하기 위해 등장한 RNN네트워크가 GRU와 LSTM

![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%206.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%206.png)

# Sequential Model

- 받아들여야 하는 입력의 차원이 몇 개인지 모르기 때문에 어렵다
- 입력의 차원과 상관없이 모델이 동작 할 수 있어야 함
- Markov model (first-order autoregressive model)
    - 나의 현재는 바로 직전 과거에만 dependent하다
    - joint distribution을 표현하기 굉장히 쉽지만, 과거 많은 정보를 버리게 된다
- Latent autoregressive model
    - Hidden state가 과거의 정보를 요약하고 있고, 다음 time step은 하나의 hidden state에 의존

## Recurrent Neural Network

- Short-term dependencies
    - 짧은 과거 정보는 잘 동작하지만, 긴 과거 정보는 잘 기억하지 못한다

    ![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%207.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%207.png)

    - $\phi$가 sigmoid라면 0~1사이로 n번 곱하게 되어서 Vanishing gradient이 일어남
    - $\phi$가 ReLU라면 n번 곱하면서 값이 커지게 되어서 exploding graident가 일어남

## Long Short Term Memory

![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%208.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%208.png)

![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%209.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%209.png)

- cell state는 내부에서만 흐르고 밖으로 나가지 않는다
- cell state는 지금까지의 time step의 정보를 다 취합해서 summerize 하는 정보
- Forget gate
    - 어떤 정보를 버릴지 결정한다

    ![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%2010.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%2010.png)

- Input gate
    - 현재 들어온 정보 중 어떤 정보를 cell state에 올릴지 결정

    ![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%2011.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%2011.png)

- Update cell
    - cell state를 업데이트 함

    ![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%2012.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%2012.png)

- Output Gate
    - 어떤 정보를 output으로 내보낼지 결정

    ![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%2013.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%2013.png)

## Gated Recurrent Unit

- gate가 2개밖에 없다
- cell state가 없다 hidden state가 곧 output이고 다음번 cell에 들어감

![Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%2014.png](Day%2014%20Recurrent%20Neural%20Network%20d7ace39cfd0c47aaaf5ddc8486c88db5/Untitled%2014.png)

- 요즘에는 Transformer 때문에 LSTM, GRU 다 잘 안쓰임

# Transformer

내용이 많고 복잡하기 때문에 자세한 설명은 아래 블로그에서.

[https://nlpinkorean.github.io/illustrated-transformer/](https://nlpinkorean.github.io/illustrated-transformer/)

# Appendix

- Fully Convolutional Networks for Semantic Segmentation

[https://modulabs-biomedical.github.io/FCN](https://modulabs-biomedical.github.io/FCN)