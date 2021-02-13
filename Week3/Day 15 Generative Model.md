# Day 15 Generative Model

## Generative model

- Generation(sampling)
- Density estimation(anomaly detection) : 엄밀히 분류 모델을 포함
    - 생성 + 분류 → explicit 모델 이라고 함
    - implicit 모델을 생성만 가능
- Unsupervised representation learning(feature learning)

## Basic Discrete Distributions

- Bernoulli distribution (coin flip)
    - 앞 또는 뒤가 나올 확률을 표현하는 파라미터는 1개가 필요
- Categorical distribution (m-sided dice)
    - m-1개의 파라미터가 필요
- 1개의 칼라 픽셀이 필요로 하는 파라미터 수 : 255 X 255 X 255
- 바이너리 이미지에서 픽셀수가 n일때 필요한 파라미터수 : $2^n-1$
- 파라미터 수를 줄이기 위해서는?
- 모든 픽셀이 independent 하다고 생각하자
- 파라미터 수는 n개가 된다
- 표현력이 떨어지기 때문에 중간을 생각 해보자

## Conditional Independence

- Chain rule
    - $p(x_1,...,x_n) = p(x_1)p(x_2|x_1)p(x_3|x_1,x_2)...p(x_n|x_1,...,x_{n-1})$
- Bayes' rule
    - $p(x|y) = \frac{p(x,y)}{p(y)} = \frac{p(y|x)p(x)}{p(y)}$
- Conditional independence(가정)
    - if $x\perp y|z$, then $p(x|y,z) = p(x|z)$
- 체인룰을 적용했을 때, 파라미터 수?  $2^n-1$, 똑같다. 아무것도 바뀐게 없기 때문
- Chain rule과 Conditional independence를 적절히 조합 해보자
- Markov assumption
    - $p(x_1,...,x_n) = p(x_1)p(x_2| x_1)p(x_3|x_2)\cdots p(x_n|x_{n-1})$
    - 파라미터 수?  $2n-1$
- Markov assumption을 이용해 exponential reduction을 얻었다
- conditional independecy를 활용한 방법을 Auto-regressive models라고 부른다

## Auto-regressive Model

- 변수의 순서를 정하는 것이 중요
- 이전 n개의 데이터를 고려하는 것을 AR-n 모델
- 이전 1개의 데이터를 고려하는 것을 AR-1 모델
- 변수의 순서, Conditional independence를 정하는 것에 따라 성능, 모델의 structure가 달라질 수 있다

## NADE : Neural Autoregressive Density Estimator

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled.png)

- i번째 pixel은 i-1개의 pixel에 의존하는 Auto-regressive 모델
- 입력 차원이 계속 바뀜. weight가 계속 커짐
- explicit 모델이다. 확률 계산이 가능
- 첫번째 확률이 주어지면 다음 확률값을 알 수 있기 때문

## Pixel RNN

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%201.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%201.png)

- RNN을 통해서 Generation 함
- 픽셀을 ordering하는 두 개의 다른 구조가 있음

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%202.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%202.png)

## Latent Variable Models

D. Kingma, "Variational Inference and Deep Learing: A New Synthesis", Ph.D. Thesis 강추

## Variational Auto-encoder

- Variational inference (VI)
    - posterior distribution을 잘 근사 할 수 있는 variational distribution을 찾는 것
    - 특히 KL divergence를 활용해서 posterior과 variatonal distribution의 차이를 최소화 하는것

    ![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%203.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%203.png)

- 어떻게 찾나?

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%204.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%204.png)

- Evidence Lower Bounce를 계산해서 이를 maximize 함으로써 Objective를 minimize함

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%205.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%205.png)

- x라는 입력을 잘 표현하는 z라는 Latent space를 찾고 싶다
- posterior를 모르니, 근사를 할 수 없다
- VI 테크닉 → ELBO를 maximize 하는 것이 KL divergence 거리를 줄여 주는 역할
- ELBO는 Reconstruction Term 과 Prior Fitting Term으로 나뉜다
- Key limitation
    - intractable model(hard to evaluate likelihood)
    - KL divergence 를 미분해야 하기 때문에 Gaussian을 사용해야 함

## Adversarial Auto-encoder

- 성능이 좋을 때가 많음

# Generative Adversarial Network

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%206.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%206.png)

- discriminator가 generator를 통해 더 좋아지기 때문에 generator도 좋아짐

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%207.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%207.png)

## GAN Objective

- A two player minimax game between generator and discriminator
- For discriinator:

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%208.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%208.png)

- where the optimal discriminator is

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%209.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%209.png)

- For generator:

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%2010.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%2010.png)

- Plugging in the optimal discriminator, we get

![Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%2011.png](Day%2015%20Generative%20Model%20931379389eac4bebbe4a648e5ba52474/Untitled%2011.png)

## DCGAN

- image 도메인에서 GAN을 활용함

## Info-GAN

## Text2Image

- 문장이 주어지면 이미지를 만듦
- DAL-E?

## Puzzle-GAN

- sub패치 들이 들어가면 원래 이미지를 복원

## CycleGAN

- image의 도메인을 바꿔주는 것
- Cycle-consistency loss

## Star-GAN

- image의 모드를 바꿔줄 수 있음

## Progessive-GAN

- 고차원의 이미지를 만들어 냄
- 작은 해상도의 이미지부터 고해상도까지 점차 늘려가면서 만들어 내는 것