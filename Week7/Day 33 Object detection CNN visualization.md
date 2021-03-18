# Day 33 Object detection / CNN visualization

# Object detection

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled.png)

- Instance, Panoptic segmentation은 서로 다른 객체들을 구분한다
- Object detection = Classification + Box localization
- Bounding Box는 센터점과 넓이, 높이 혹은 왼쪽 위와 오른쪽 아래점 좌표로 표현 가능

## Two-stage detector

- Traditional methods
    - Gradient-based detector(e.g., HOG) → 사람의 직관을 바탕으로한 테크닉
    - Selective search
        - 영상을 비슷한 색끼리 분리하고, 비슷한 영역끼리 반복해서 합쳐준다
        - 큰 segmentation을 포함하는 바운딩 박스를 제안한다

        ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%201.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%201.png)

### R-CNN

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%202.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%202.png)

- 마지막에 classifier로서 SVM을 학습한다
- 속도가 느리고 selective search를 사용하여 성능의 한계

### Fast R-CNN

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%203.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%203.png)

- Conv feature map을 뽑아 놓는다
- RoI pooling layer를 통해 Roi feature를 뽑는다
- 각각의 RoI에 대해 class와 바운딩 박스를 예측한다

### Faster R-CNN

- 최초의 End-to-end Object detector
- IOU : 두 영역의 오버랩을 측정하는 metric

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%204.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%204.png)

- Anchor boxes : 각 위치에서 발생할 것 같은 box들을 미리 정해 둔 후보군

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%205.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%205.png)

- Region Proposal Network(RPN)을 제안해, Time-consuming selective search를 대체함

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%206.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%206.png)

- 미리 정해둔 대표적 k개의 anchor boxes
- 더 정교한 위치는 regression 문제로 다시 푼다
- cls 분류를 위한 2k scores, 위치 regression을 위한 4k coordinates

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%207.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%207.png)

- Non-Maximu Suppresion(NMS)
    - 많은 바운딩 박스 중 주요한 박스만 남기고 필터링 하는 표준적인 알고리즘

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%208.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%208.png)

## Single-stage detector

- 정확도를 조금 포기하더라도 빠른 수행시간을 통한 리얼타임 디텍팅을 목표로 함

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%209.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%209.png)

### You only look once (YOLO)

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2010.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2010.png)

- 각 그리드에 대해 B개의 bounding box score, confidence score, class score를 예측

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2011.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2011.png)

- B = 2, C = 20이어서 각 그리드 마다 30차원의 결과가 나옴

### Single Shot MultiBox Detector(SSD)

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2012.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2012.png)

- 각 피쳐맵 마다 해상도에 적절한 바운딩 박스 크기를 예측할 수 있도록 멀티스케일을 고려함

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/_2021-03-17__1.14.01.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/_2021-03-17__1.14.01.png)

## Two-stage detector vs. one-stage detector

- Class imbalance problem
    - one-stage detector에서는 배경에 대해 바운딩박스를 계산하여 문제가 된다
- Focal loss
    - 잘 맞추었을 때는 더 낮은 로스를 주는것, 맞추지 못할 경우 더 샤프한 gradient를 주는것

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2013.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2013.png)

- RetinaNet

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2014.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2014.png)

## Detection with Transformer

- 최근의 트렌드
    - ViT by Google
    - DeiT by Facebook
    - DETR by Facebook
- DETR(DEtection TRansformer)
    - Feature들을 토큰으로 넣어줌
    - 물체에 대한 질의를 queries로 넣어줌

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2015.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2015.png)

---

# CNN Visualization

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2016.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2016.png)

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2017.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2017.png)

- 뒤쪽 레이어는 차원수가 높아서 사람이 직관적으로 해석하기 어렵다

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2018.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2018.png)

## Analysis of model behaviors

- Nearest Neighbors(NN) in a feature space

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2019.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2019.png)

- Dimensionality reduction
    - 고차원 분포를 차원축소를 통해 저차원 분포로 바꿔주는 것
    - t-distributed stochastic neighbor embedding (t-SNE)
- Layer activation
    - hidden node의 activation map을 추출

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2020.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2020.png)

- Maximally activating patches
    - hidden layer의 큰 값을 나타내는 부근을 뜯어내 나열하는 것
    - 특정 layer의 acitivation map중 큰 값을 나타내는 부분에 대응되는 원본이미지의 receptive field를 계산하여 표시해 준다

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2021.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2021.png)

- Class visualization

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2022.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2022.png)

    - 최적화를 통해 영상을 생성함 (Gradient ascent)
    - loss 함수는 다음과 같다

        ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2023.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2023.png)

    - backpropagation을 통해 입력이미지를 업데이트 해 나간다

## Model decision explanation

- Occlusion map
    - 위치별로 Occlusion 패치로 가렸을 때, 확률 스코어를 계산하여 히트맵으로 만드는 것

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2024.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2024.png)

- via Backpropagation
    - 특정 이미지를 classification 해보고, 결정적으로 영향을 미친 부분이 어느 곳인지 히트맵 형태로 표현

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2025.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2025.png)

    - backpropagation을 입력이미지 까지 하고, gradient를 제곱이나 절대값을 취하고, visualization한다
- Rectified unit(backward pass)

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2026.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2026.png)

- Guided backpropagation

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2027.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2027.png)

![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2028.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2028.png)

- Class activation mapping (CAM)

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2029.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2029.png)

    - 최종 Feature map을 Global average pooling(GAP)을 한 후 FC layer에 통과시킨다

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2030.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2030.png)

    - GAP을 하기 전 이기 때문에 공간 정보를 가지고 있음
    - 마지막 모델 구조를 바꿔야 하고 재학습 해야 하는 단점이 있다
    - ResNet and GoogLeNet은 GAP를 가지고 있어서 CAM을 활용하기에 용이하다
- Grad-CAM
    - 재학습 없이, 구조를 바꾸지 않고 미리학습된 네트워크에서 CAM을 뽑을 수 있는 방법
    - backbone이 CNN이기만 하면 사용할 수 있음
    - 관심 있는 activation map까지만 backprop를 함

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2031.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2031.png)

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2032.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2032.png)

    - 여러가지 task에 활용할 수 있는 일반화된 툴
    - Guided Backprop와 곱하여 결합 할 수도 있다

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2033.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2033.png)

- SCOUTER
    - 왜 그러한 결론을 내리고, 다른것은 왜 아닌지 비교대조 가능한 방법

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2034.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2034.png)

- GAN dissection

    ![Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2035.png](Day%2033%20Object%20detection%20CNN%20visualization%20ca0a9df21cf641da9ba349dbf489d47f/Untitled%2035.png)

    - 유저가 컨트롤 할 수 있는 GAN을 유도 할 수 있다