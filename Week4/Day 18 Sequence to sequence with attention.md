# Day 18 Sequence to sequence with attention

# Seq2Seq Model

![Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled.png](Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled.png)

- start토큰으로 <SoS>, end토큰으로 <END>
- RNN에서, 마지막 히든 스테이트 벡터에 시퀀스 전체 정보를 압축하여 저장하여야 한다
- LSTM라 하더라도 앞쪽에 나타난 정보가 뒤로 갈수록 옅어지는 현상이 발생함
- 입력 문장의 순서를 역순으로 바꿔서 초반 단어를 잘 생성 하도록 하는 테크닉도 있음

# Seq2Seq Model with Attention

- 각 타임 스텝의 입력에 의한 인코더 히든 스테이트 벡터 모두를 디코더가 적절히 참조하여 단어를 생성함

![Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%201.png](Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%201.png)

- 디코더에서 마지막 인코더 hidden state vector와 입력(start)으로 부터 디코더 히든 스테이트 벡터를 만들고,
- 주어진 인코더 히든 스테이트 벡터 각각과 디코더 히든 스테이트 벡터의 내적 값(유사도)을 구하고 소프트맥스를 취해주는 것으로 각각 인코더 히든 스테이트 벡터에 해당하는 확률값을 구할 수 있다
- 확률값을 인코더 히든 스테이트 벡터에 주어지는 가중치로 생각 할 수 있고 가중평균을 구할 수 있다
- 인코더 히든 스테이트 벡터의 가중평균된 벡터는 attention module의 아웃풋이 된다
- 디코더 히든 스테이트 벡터와 attention 아웃풋 벡터와 concat이 되어 아웃풋 레이어의 입력이 되고 다음 단어를 예측한다
- 다음 단어와 이전 디코더 히든 스테이트 벡터로부터 현재 디코더 히든 스테이트 벡터가 나오고 어텐션 모듈에 넣어준다
- 같은 방식으로 출력을 계속 뽑아낸다
- 합이 1인 형태의 가중치 벡터를 attention vector라 한다
- 디코더 히든스테이트 벡터는 다음 단어를 예측하는 역할과 인코더 스테이트 벡터중 어떤 정보를 중점적으로 선택해야 하는지를 고려하는 역할을 한다
- backpropagation은 역순으로 진행된다
- 전 단계에서 예측을 잘못 했더라도 그 다음 스텝의 입력으로 ground truth를 넣어 학습한다
    - Teacher forcing 방식, 학습이 빠르고 용이하다
    - 학습 초반엔 Teacher forcing을 적용하고, 어느정도 학습 한 후엔 적용하지 않는 방법도 있다
- inference단계(테스트 단계)에서는 실제 예측값을 다음 타임 스텝의 입력으로 넣어준다

## Different Attention Mechanisms

![Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%202.png](Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%202.png)

- generalized dot product
    - 내적에 더하여 학습 가능한 행렬을 추가하여 보다 일반화된 유사도를 구할 수 있도록 구성할 수 있다
- concat
    - 벡터를 concat하여 fully connected layer를 적용하여 하나의 scalar값을 구할 수 있다
        - 혹은 layer를 여러층 쌓을 수도 있다(그림의 예제는 2층)

## Attention is Great

- 성능이 많이 향상됨
- information bottleneck problem을 해결함. 긴 문장의 문제를 해결
- 학습의 관점에서 vanishing gradient problem을 해결함
    - RNN은 decoder의 출력에서, encoder 앞 단어 까지의 gradient를 전달하는 경로가 너무 길었지만,  attention module을 통과하여 바로 인코더 벡터에 전달 할 수 있다
- attention 패턴을 조사함으로써, 디코더가 어디 부분에 집중했는지 알 수 있다

## Greedy decoding

- 시퀀스의 전체적인 문장의 확률 값을 보는 것이 아니라, 근시안 적으로, 현재 타임 스텝에서 가장 좋아보이는 단어를 선택하는 방법
- 중간에 다른 단어를 예측 했을 경우 뒤로 돌아가지 못하는 문제가 있다
- 이상적으로는 동시사건 확률을 계산하여 확률값이 최대가 되는 값을 찾으면 된다
- 하지만 $O(V^t)$이기에 현실적으로 불가능
- greedy와 전탐색 방법중 사이에 있는 차선책이 beam search

## Beam search

- 디코더의 매 time step마다, k개의 candidate 중에서 가장 확률이 높은 것을 선택함
    - beam size는 5~10개로 함

![Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%203.png](Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%203.png)

- Beam size : k = 2

![Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%204.png](Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%204.png)

- 확률이 0~1 이기 때문에 log를 취해주면 -가 된다. 그리고 단조 증가 함수가 된다
    - log값이 가장 큰 값을 선택하면 된다
- greedy decoding은 <end>토큰을 생성할때까지 디코딩한다
- beam search decoding은 각각의 다른 hypotheses는 다른 타임스텝에서 <end>토큰을 만들게 된다
    - 정해진 타임스텝 T 혹은 정해진 hypotheses 길이 n이 되면 중단한다
- 각각의 hypotheses의 길이가 다를때, 상대적으로 긴 길이의 hyptheses의 동시사건확률이 낮은 값이 나오게 되는 문제가 있다(항상 -값을 더해주기 때문)
- Normalize by length를 통해 해결 가능

![Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%205.png](Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%205.png)

# BLEU score

- 문장에서 단어가 하나씩 밀린경우, 각 자리에서 유사도를 평가하면, 정확도가 0%가 되는 문제가 있음
- 새로운 지표가 필요함

    ![Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%206.png](Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%206.png)

    ![Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%207.png](Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%207.png)

    - correct words : 겹치는 단어 수
    - precision : 정밀도, 정보 검색에서 특정 키워드를 검색하여 나온 문서가 의도에 부합하는 문서인가
    - recall : 재현율, 키워드를 검색하여 실제로 나와야 할 문서들이 얼마나 나오는가
    - F-measure : precision과 recall의 조화평균
        - 조화평균은 기하평균, 산술평균보다 작으므로, 내분점이 작은쪽에 가깝게 찍힌다
    - 각각의 단어가 ground truth와 일치하면서 순서만 달라졌을때, precision, recall, f-measure가 100%가 되어 버리는 문제가 있다
    - 기계번역의 성능평가에 사용되는 BLEU스코어가 있다
- BiLingual Evaluation Understudy(BLEU)
    - N개의 연속된 단어, N-gram을 확인하여 평가하는 방법

    ![Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%208.png](Day%2018%20Sequence%20to%20sequence%20with%20attention%2007a35c3e8f2240ecaecf2d1cbe183137/Untitled%208.png)

    - BLEU스코어는 recall을 사용하지 않는다
        - 문장에서 수사 하나가 없더라도 좋은 번역이 될 수 있다
    - 1-gram ~ 4-gram의 precision을 곱하여 기하평균을 냄
    - 앞의 항을 Brevity penalty라고 함
        - 길이 만을 고려했을때 짧아진 비율 만큼 낮춰주는 것(recall의 요소가 내포되어 있음)

# Appendix

- seq2seq with attention

[https://katie0809.github.io/2020/02/23/ai-study7/](https://katie0809.github.io/2020/02/23/ai-study7/)

- bag of words

[https://wikidocs.net/22650](https://wikidocs.net/22650)

- GloVe

[https://ratsgo.github.io/from frequency to semantics/2017/04/09/glove/](https://ratsgo.github.io/from%20frequency%20to%20semantics/2017/04/09/glove/)

- seq2seq

[https://wikidocs.net/24996](https://wikidocs.net/24996)

- 단어 유사도

[https://sy-programmingstudy.tistory.com/13](https://sy-programmingstudy.tistory.com/13)