# Day 19 Transformers

# Transformer

- No more RNN or CNN modules
- Attension module 로만 이루어져 있음

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled.png)

- 벡터들의 내적에 기반한 유사도를 구함
- 동일한 세트 내 벡터들 내에서 적용하므로 self-attention module이라고 함
- 자기자신과의 내적이 큰 값을 가지기 때문에 자신의 정보만을 크게 가지는 문제가 발생함
- 각 벡터들이 Query, key value 벡터의 역할을 함
- 주어진 벡터들중 어느 정보를 가져올지 중점이 되는 벡터가 Query 벡터
- Query 벡터와 내적이 되는 재료 벡터를 key 벡터 라고 함
- 유사도를 바탕으로 가중치가 적용되어 가중 평균이 구해지는 재료벡터를 value 벡터라 함
- 주어진 벡터가 세가지 각각의 역할을 하는 벡터로 변환하는 선형변환 매트릭스가 각각 존재함
    - $W^Q, W^K, W^V$가 됨
- query벡터와 key벡터를 내적한후 softmax를 통해 나온 확률 가중치를 value 벡터에 적용함
- key벡터와 value벡터의 개수는 동일 해야함. (1:1대응)
- value벡터의 가중평균의 결과로써 구해지는 최종적인 인코딩 벡터 $h_i$
- 인코딩 벡터는 모든 단어로 부터 정보를 적절히 결합한 값
    - 시퀀스 길이가 길더라도 거리와 상관없이 정보를 가져올 수 있다
        - Long-term dependency 문제를 해결함
- query 벡터와 key벡터는 같은 차원 $d_k$를 가져야 한다. value 벡터의 차원은 같지 않아도 된다 $d_v$

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%201.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%201.png)

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%202.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%202.png)

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%203.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%203.png)

- 각각의 row벡터에 따라 softmax 연산을 취해줌
- 실제 transformer 구현 상으론 동일한 shape로 mapping된 Q, K, V가 사용되어 각 matrix의 shape는 모두 동일하다

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%204.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%204.png)

- Scaled Dot-Product Attention
    - $d_k$가 커지면, $q^Tk$의 분산이 커지게 된다
    - softmax의 출력의 확률분포가 큰 값에 몰려서, 강한 피크를 보이게 된다
        - gradient vanishing 문제가 나타남
    - 내적값의 분산을 일정하게 하기 위해, $\sqrt{d_k}$로 나눠준다

# Transformor : Multi-Head Attention

- 병렬적으로 여러 버젼의 attention을 수행함

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%205.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%205.png)

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%206.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%206.png)

- 주어진 문장에서 서로 다른 측면에서의 정보를 병렬적으로 뽑고 그 정보를 합치는 과정
    - 서로 다른 head가 서로 보완적으로 정보를 추출함
- attention을 통해 나온 벡터들을 concat한 후 원래 차원으로 돌려주는 선형변환 layer를 통과시킴

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%207.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%207.png)

- gpu의 코어수가 무한대라고 생각할때, 행렬연산을 병렬적으로 계산하면 $O(1)$
- self-attention : 시퀀스 길이의 제곱에 비례하는 메모리가 필요함
- RNN은 순차적으로 계산해야 하기 때문에 병렬화가 불가능
- self-attention은 메모리 요구량은 RNN보다 크지만 학습속도는 빠르다

# Transformer: Block-Based Model

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%208.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%208.png)

- Each block has two sub-layers
    - Multi-head attention
    - Two-layer feed-forward NN (with ReLU)
- Each of these two steps also has
    - Residual connection
        - gradient vanishing 문제 해결 및 학습 안정화
    - layer normalization
    - $LayerNorm(x + sublayer(x))$

# Transformer : Layer Normalization

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%209.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%209.png)

- Nomarlization : 평균을 0, 분산을 1로 만들어 주고, affine transformation을 통해 원하는 평균, 분산을 주입해 주는것. (y = 2x + 3 변환을 해주면 평균은 3, 분산은 $2^2$으로 변환됨. 2와 3은 학습때 최적화 되는 값)

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2010.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2010.png)

- 각 word 벡터를 평균 0, 분산 1로 normalization함
- 각 노드별로 동일한 Affine transformation하여 원하는 평균, 분산을 주입함

# Transformer: Positional Encoding

- self-attention에서, 단어의 순서가 달라도 output 벡터가 동일하게 나타나는 문제가 있다
- 순서정보를 반영하는 추가적인 작업이 필요함
- 순서를 특정지어 줄 수 있는 특정한 상수 벡터를 각 순서의 워드 입력 벡터에 더해줌

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2011.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2011.png)

# Transformer: Warm-up Learning Rate Scheduler

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2012.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2012.png)

- iteration이 진행 됨에 따라 learning rate를 동적으로 바꿔 줌

# Transformer: High-Level View

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2013.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2013.png)

# Transformer: Decoder

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2014.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2014.png)

- decoder의 입력은 shifted right해줌 (<sos>, 나는, 집에)
- Masked decoder self-attention on previously generated output
- Encoder-Decoder attention
    - decoder에서 만들어 진 hidden state vector를 Query 벡터로, encoder의 최중 출력을 Key, Value 벡터로 가져옴

# Transformer: Masked Self-Attention

![Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2015.png](Day%2019%20Transformers%20b449345c3c784571aae53bd7e344903b/Untitled%2015.png)

- 예측 과정에서는 뒤에 나오는 단어로의 접근을 제외 해주어야 한다
- 대각선의 위쪽에 나오는 정보를 0으로 지워 준다
- 그 다음에 row별로 합이 1이 되도록 normalizaion을 해 준다