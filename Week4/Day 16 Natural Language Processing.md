# Day 16 Natural Language Processing

## Natural language processing

- Low-level parsing
    - Tokenization, stemming(다양한 변형형에서 단어의 어근을 추출)
- Word and phrase level
    - Named entity recognition(고유 명사 인식)
    - part-of-speech tagging(문장에서 단어의 성분이 무엇인지 알아내는 것)
- Sentence level
    - Sentiment analysis, machine translation
- Multi-sentence and paragraph level
    - Entailment prediction(두 문장간의 논리적 내포 혹은 모순 관계를 예측)
    - question answering(독해 기반의 질의응답)
    - dialog systems(대화 기능)
    - summarization(요약)

## Text mining

- Extract useful information and insights from text and document data
- Document clustering (e.g., topic modeling)
- Highly related to computational social science(사회 현상)

## Information retrieval

- 정보 검색 시스템, 추천시스템

## Trends of NLP

- text data를 단어로 구분하고 벡터로 표현하는 과정을 거침
    - word embedding (Word2Vec, GloVe)
- RNN-family models(LSTMs and GRUs)
- attention modules and Transformer models(RNNs with self-attention)
- Transformer model - 자연어 처리 여러 분야 뿐 아니라 다른 분야에도 사용됨
- self-supervised 모듈을 크게 쌓는 경향
    - 라벨이 없는 자가 지도 학습 e.g., BERT, GPT-3 ...
        - 범용 인공지능 기술로서 발전함
- 하지만 엄청난 컴퓨터 자원이 필요하다

## Bag-of-Words

- text data set에서 unique한 단어를 모아 vocabulary를 만든다
- unique words를 one-hot vectors로 인코딩한다
    - 어떤 단어 쌍의 유클리디언 거리가 모두 루트2, cosine similarity는 0
- 문장의 각 단어들의 원핫 벡터를 더해줌으로써 문장을 표현할 수 있다

## NaiveBayes Classifier

- For a document $d$ and a class $c$

![Day%2016%20Natural%20Language%20Processing%20960c245a0ca94201a4d2bedf100dcfce/Untitled.png](Day%2016%20Natural%20Language%20Processing%20960c245a0ca94201a4d2bedf100dcfce/Untitled.png)

- $P(d|c)P(c)= P(w_1,w_2,...,w_n|c)P(c) \rightarrow P(c)\prod_{w_i\in W}P(w_i|c)$                                     (by conditional independence assumption)

## Word Embedding

- Express a word as a vector
- 비슷한 의미의 단어는 비슷한 벡터로 표현된다 → short distance
- 다른 의미의 단어는 다른 벡터로 표현 → far distance

## Word2Vec

- Assumption : 인접한 단어가 인접한 의미를 갖는다
- 주변에 등장하는 단어를 통해 단어의 의미를 알 수 있다는 방법론

![Day%2016%20Natural%20Language%20Processing%20960c245a0ca94201a4d2bedf100dcfce/Untitled%201.png](Day%2016%20Natural%20Language%20Processing%20960c245a0ca94201a4d2bedf100dcfce/Untitled%201.png)

- window를 움직여가며 단어 쌍을 만듦
- hidden layer의 차원은 하이퍼 파라미터. 여기서는 2차원으로 가정
- $x$가 원핫벡터 이기 때문에 $W_1$의 컬럼을 추출하는 형태. 코딩때도 행렬곱이 아닌 index를 뽑아오는 방식으로
- softmax를 통과한 확률 분포가 ground truth와 가깝도록 학습함
- 내적 연산을 벡터들간의 유사도를 나타내는 과정으로 생각하면, ground truth에 해당하는 벡터와의 내적값은 커지도록 파라미터를 학습한다

![Day%2016%20Natural%20Language%20Processing%20960c245a0ca94201a4d2bedf100dcfce/Untitled%202.png](Day%2016%20Natural%20Language%20Processing%20960c245a0ca94201a4d2bedf100dcfce/Untitled%202.png)

- juice의 입력단어 벡터와 drink의 출력단어 벡터와 유사한 것을 알 수 있다
- 최종적으로 아웃풋 임베딩 벡터는 input vector를 사용한다
- 같은 벡터는 같은 관계성을 표현한다
    - vec[queen] - vec[king] = vec[woman] - vec[man]
- Word intrusion detection : 여러 단어가 있을 때 관계성이 가장 떨어지는 단어를 찾는 것
    - 단어 간의 유클리디언 거리를 평균 내서 평균값이 가장 큰 것이 가장 의미가 상이한 단어
- Word2Vec은 NLP의 거의 모든 분야에서 사용됨

## GloVe : Global vectors for Word Representation

- $J(\theta) = \frac{1}{2}\sum^W_{i,j=1}f(P_{ij})(u_i^Tv_j -logP_{ij})^2$
- Fast training
- Works well even with a small corpus
- [https://nlp.stanford.edu/projects/glove/](https://nlp.stanford.edu/projects/glove/)

## Appendix

- 자연어 처리
    - [https://sy-programmingstudy.tistory.com/13](https://sy-programmingstudy.tistory.com/13)
- Bag of words
    - [https://wikidocs.net/22650](https://wikidocs.net/22650)
- GloVe
    - [https://ratsgo.github.io/from frequency to semantics/2017/04/09/glove/](https://ratsgo.github.io/from%20frequency%20to%20semantics/2017/04/09/glove/)