# Day 17 RNN

## Recurrent Neural Network

- How to calculate the hidden state of RNNs
    - $h_t = f_W(h_{t-1}, x_t)$
        - $h_{t-1}$: old hidden-state vector
        - $x_t$ : input vector at some time step
        - $h_t$ : new hidden-state vector
        - $f_W$ : RNN function with parameters $W$
        - $y_t$ : output vector at time step $t$
            - 출력 형태에 따라 매 time step마다 계산 할 수도, 마지막에만 계산 할 수도 있다
    - 모든 time step 에서 파라미터를 공유함
    - $h_t = \mathrm{tanh}(W_{hh}h_{t-1}+W_{xh}x_t)$
    - $y_t = W_{hy}h_t$

    ![Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled.png](Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled.png)

    ## Types of RNNs

    - One-to-one
        - Standard Neural Network
    - One-to-many
        - Image Captioning
        - 같은 사이즈의 0으로 채워진 입력을 rnn에 주게 됨
    - Many-to-one
        - Sentiment Classification
    - Sequence-to-sequence
        - Machine translation
            - 입력이 끝나면서 출력을 내기 시작함
        - Video classification on frame level
            - 입력이 주어질 때마다 출력하는 태스크

    ![Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled%201.png](Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled%201.png)

## Character-level Language Model

- 글자 단위로 사전을 생성
- 각 글자는 사전 길이 만큼의 크기의 원핫벡터로 표현 가능
- 입력 글자가 주어질때 다음에 나올 글자를 예측하는 태스크를 RNN으로 계산
- $h_t = \mathrm{tanh}(W_{hh}h_{t-1}+W_{xh}x_t+b)$
- $h_0$ 는 defalut로 0으로 줌
- $\mathrm{Logit} = W_{hy}h_t+b$
- multi classification 이므로 Logit 벡터에 소프트 맥스를 취해줌
- test에서 inference를 수행할때 첫번째 입력만 주면, 첫번쨰 예측값을 다음 time step의 입력으로 넣는다
- 동일한 모델로 긴 시간의 예측이 가능하다

![Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled%202.png](Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled%202.png)

- 문단 단위의 긴 글을 학습 할 때에도, 공백이나 특수문자를 하나의 캐릭터 입력으로 학습하면 가능하다
- 모든 시퀀스에 대해 학습 시키는 것은 컴퓨팅 자원이 부족하므로 truncation(잘라서) 학습시킨다
- 다양한 상황에 대한 정보는 hidden state vector에 저장됨
- hidden state vector의 차원 하나를 고정하는 방법으로 RNN의 특성을 분석 할 수 있다
- Vanilla RNN → 동일한 matrix을 매 time step마다 곱하기 때문에 backpropagation에서 gradient vanishing or exploding이 발생

## Long Short-Term Memory

- Pass cell state information straightly without any transformation
    - Solving long-term dependency problem
- $\{c_t,h_t\} = \mathrm{LSTM}(x_t,c_{t-1},h_{t-1})$
- cell state가 핵심 정보를 담고 있다. hidden state vector는 cell state를 한번 더 가공하여 현재 time step의 다음 레이어의 입력 벡터로 사용됨
- i : Input gate, Whether to write to cell
- f : Forget gate, Whether to erase cell
- o : Output gate, How much to reveal cell
- g : Gate gate, How much to write to cell

![Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled%203.png](Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled%203.png)

- Forget gate
    - $f_t = \sigma(W_f\cdot[ h_{t-1},x_t] + b_f)$
    - 이전 time step에서 넘어온 정보 중 일부를 forget함
- Input gate
    - $i_t = \sigma(W_i\cdot[h_{t-1}, x_t] + b_i)$
- Gate gate
    - $\tilde{C_t} = \mathrm{tanh}(W_C \cdot[h_{t-1}, x_t] + b_C)$
- $C_t = f_t\cdot C_{t-1}+  i_t\cdot\tilde{C_t}$
- Output gate
    - $o_t = \sigma(W_o\cdot[h_{t-1}, x_t] + b_o)$
- $h_t = o_t\cdot\mathrm{tanh}(C_t)$
    - Cell state의 많은 정보 중 지금 당장 필요한 정보만을 필터링한 것으로 생각 할 수 있다

![Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled%204.png](Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled%204.png)

## Gated Recurrent Unit (GRU)

- $z_t = \sigma(W_z\cdot [h_{t-1}, x_t])$
- $r_t = \sigma(W_r\cdot [h_{t-1}, x_t])$
- $\tilde{h_t}=\mathrm{tanh}(W\cdot [r_t\cdot h_{t-1},x_t])$
- $h_t = (1-z_t)\cdot h_{t-1} + z_t\cdot \tilde{h_t}$

![Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled%205.png](Day%2017%20RNN%20f1e4f4068dfc4b378c7ae8ff47dd0eb1/Untitled%205.png)

- GRU는 LSTM의 cell state와 hidden state를 일원화 함
- LSTM의 2개의 gate의 연산을 1개의 gate로 줄임
- 계산량과 메모리 요구량을 줄인 경량화된 모델

## Summary on RNN/LSTM/GRU

- RNNs allow a lot of flexibility in architecture design
- Backward flow of gradients in RNN can explode or vanish
- LSTM,GRU는 Backpropagation에서 곱셈이 아닌 덧셈 연산을 하기 때문에 gradient vanishing, exploding문제가 해결