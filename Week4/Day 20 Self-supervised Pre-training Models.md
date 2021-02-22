# Day 20 Self-supervised Pre-training Models

## Recent Trends

- self-attention block을 가진 transformer model은 여러 분야에서 활용됨
- 깊게 쌓은 transformer model을 self-supervised learning framework를 통해 학습 시킨 후 다양한 NLP task에 대해 transfer learning을 통해 해결함
- greedy decoding이라는 한계가 존재함

## GPT-1

![Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled.png](Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled.png)

- 다음 단어를 예측하는 Language modeling task를 통해 학습함
- 모델의 큰 변형 없이 여러 task에 대해 적용할 수 있는 framework를 제시함
- main task를 위한 마지막 layer를 추가해 줌으로써 다른 task에 활용 할 수 있음
- fine-tuning을 통해 전체 네트워크를 조금 학습하고 마지막 layer를 주로 학습한다

## BERT

- 문맥의 앞, 뒤 모두 고려해야 하는 필요성이 존재함
- Masked Language Model(MLM)
    - 토큰의 일부를 랜덤으로 mask로 치환하고, 그 단어를 예측하는 모델
    - 15% of the words to predict
        - 80% of the time, replace with [MASK]
        - 10% of the time, replace with a random word
        - 10% of the time, keep the sentence as same
- Next Sentence Prediction(NSP)
    - 2개의 문장이 연속된 문장인지, 랜덤인 문장인지 예측함

    ![Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%201.png](Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%201.png)

- BERT Summary
    - Model Architecture
        - BERT BASE:  L = 12, H = 768, A = 12 (A는 멀티 헤드 숫자)
        - BERT LARGE: L = 24, H = 1024, A = 16
    - Input Represetation
        - WordPiece embedding(30,000 WordPiece(의미 단위로 단위를 쪼갬))
        - Learned positional embedding (position embedding도 학습에 의해 결정됨)
        - [CLS] - Classification embedding
        - Packed sentence embedding [SEP]
        - Segment Embedding (문장 별로 다른 벡터를 더해 줌)
    - Pre-traning Tasks
        - masked LM
        - Next Sentence Prediction

![Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%202.png](Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%202.png)

![Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%203.png](Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%203.png)

## BERT: Fine-tuning Process

![Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%204.png](Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%204.png)

- model 사이즈를 키우고 파라미터 수를 늘리면 여러 다운스트림 task에서의 성능도 늘어난다

## GPT-2

- Just a really big transformer LM. 레이어를 늘림
- Trained on 40GB of text
- 모든 종류의 자연어 처리 task를 Question Answering으로 통합함
- 질의응답형 text를 학습함(outbound links from Reddit, a social media platform, WebText)
- Byte pair encoding(BPE)을 사용함
- Scaled the wieghts of residual layer at initialization by a factor of $\frac{1}{\sqrt{n}}$where n is the number of residual layer
- Use conversation question answering dataset(CoQA)

## GPT-3

- 모델을 크게 키움
- 96 attention layers, Batch size of 3.2M
- Language Models are Few-shot Learners

![Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%205.png](Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%205.png)

- text의 일부로써 예시를 주었을때 성능이 더 좋아진다
- Model 사이즈를 키울 수록 더 큰 차이를 보인다

## ALBERT: A Lite BERT

- 성능의 큰 하락없이 모델의 사이즈를 줄이고 학습 속도를 빠르게 함
- Factorized Embedding Parameterization
    - embedding layer의 dimension을 줄여 계산량을 줄임
        - 차원수를 맞춰주는 layer를 추가함

![Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%206.png](Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%206.png)

- Cross-layer Parameter Sharing
    - Shared-FFN : Only sharing feed-forward network parameters across layers
    - Shared-attention: Only sharing attention parameters across layers
    - All-shared: Both of them
- Sentence Order Prediction
    - Next Sentence Prediction pretraining task가 너무 쉬워서 실효성이 없다
    - 연속적으로 붙어있는 두 문장을 순서를 섞어, 두 문장이 정방향인지, 역방향인지 예측하는 task
        - 단어의 overlap 측면에서 차이가 없고, 문장의 논리적인 순서를 고려해야 함

## ELECTRA: Efficiently Learning an Encoder that Classifies Token Replacements Accurately

![Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%207.png](Day%2020%20Self-supervised%20Pre-training%20Models%20373edfae38fc4b0783916c1eafeb7e6f/Untitled%207.png)

- Generator는 기존 BERT로 생각 할 수 있다
- Discriminator는 각 단어를 원래 존재하는 단어인지, 잘못된 단어인지 예측
- Generative Adversarial Network의 아이디어를 가져옴
- Discriminator를 pre-trained model로 사용함

## Light-weight Models

- 경량화 모델. 큰 사이즈의 모델의 성능을 최대한 유지하면서 계산량을 줄임
- DistillBERT
    - teacher model과 student model이 있음
    - student model은 teacher model에 비해 작은 경량화 모델
    - student model은 teacher model의 output이나 feature를 잘 모사하도록 학습함
        - teacher model의 output의 확률분포를 student model의 ground truth로 학습시킴
- TinyBERT
    - DistillBERT의 학습 방법 뿐 아니라 Attention model의 hidden vector도 유사하게 학습함

## Fusing Knowledge Graph into Language Model

- BERT의 경우, 주어져 있는 문장에 포함되어 있지 않은 추가적인 정보가 필요할 때, 잘 활용하지 못함
- 외부 지식, 상식이 Knowledge Graph로 표현 됨
- ERNIE: Enhanced Language Representation with Informative Entities
- kagNET: Knowledge-Aware Graph Networks for Commonsense Reasoning