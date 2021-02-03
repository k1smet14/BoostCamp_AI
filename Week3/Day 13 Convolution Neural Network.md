# Day 13 Convolution Neural Network

# Convolution Neural Network

- 2D Image convolution

$\begin{aligned} (I*K)(i,j) = \sum_{m}\sum_{n}I(m,n)K(i-m,j-n) = \sum_m\sum_nI(i-m,i-n)K(m,n)
\end{aligned}$

- Stack of Convolutions

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled.png)

- Convolution and pooling layers : feature extraction
- Fully connected layer : decision making (e.g., classification)
    - Fully connected layer를 없애는 추세
    - 모델의 학습해야 하는 파라미터의 숫자가 늘어날수록 학습이 어렵고 generalize performance가 떨어진다
    - 모델을 딥하게 하면서 파라미터 수를 줄이는 방향으로

## Stride

- stride = 2 → 필터를 두 픽셀마다 찍는 것

## padding

- 바운더리 정보가 버려지기 때문에, 가장자리에 어떤 값을 덧대어 주는 것
- 내가 원하는 출력에 맞추어 stride, padding을 하면 됨
- padding(1), stride(1), 3x3 kernel 일때 parameter의 수? → 3 X 3 X 128 X 64

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%201.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%201.png)

## 1x1 Convolution

- Dimension reduction → 채널을 줄임
- depth를 늘리면서 parameter수를 줄임
- e.g., bottlenect architecture

# Modern CNN

## AlexNet

- 네트워크가 2개로 나뉘어 있음 → GPU의 한계 때문에 2GPUs
- ReLU activation 사용
    - 선형 모델의 특성을 보존
    - Easy to optimize with gradient descent
    - Overcome the vanishing gradient problem
        - 기존 optimizer에선 gradient가 0으로 가까워 지는 문제
- Local response normalization, Overlapping pooling
- Data augmentation
- Dropout

## VGGNet

- 3 X 3 filter만 사용해서 더 깊게

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%202.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%202.png)

- 고려하는 영역은 같으면서 파라미터 수를 줄일 수 있다
- Dropout ( p = 0.5 )
- VGG16, VGG19

# GoogLeNet

- Network-in-network (NiN) 구조
- Inception blocks
    - 여러 response를 concatenation 함
    - 1x1 convolution을 통해 채널 방향으로 dimension을 줄임

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%203.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%203.png)

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%204.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%204.png)

- receptive field는 같으면서 parameter 수를 줄임

## ResNet

- 네트워크가 깊어질 수록 학습이 어렵다
    - 오버피팅 문제는 아님
- Add an identity map (skip connection)
    - x를 네트워크의 출력값에 더해주는 것

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%205.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%205.png)

- Batch normalization after convolutions
- Bottlenect architecture → 1x1 convolution을 더해주는 것

## DenseNet

- DenseNet uses concatenation instead of addition
- concat 할 수록 채널이 커지기 때문에 1x1 conv를 통해 줄여줌

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%206.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%206.png)

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%207.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%207.png)

## Summary

- key takeaways
    - VGG : repeated 3x3 blocks
    - GoogLeNet : 1x1 convolution
    - ResNet : skip-connection
    - DenseNet : concatenation

# Computer Vision Applications

## Semantic Segmentation

- 어떤 이미지가 있을때 이미지를 픽셀마다 분류하는 것

## Fully Convolutional Network

- Dense 레이어를 없애는 것 → convolutionalization

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%208.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%208.png)

- parameter 숫자는 바뀌지 않는다
- input dimension에 independent 하다
- input image가 커져도 동일한 conv 필터를 동일하게 작용하기 때문에 나오는 spacial dimension이 커진다? 줄어든다?
- spacial dimension(output dimension)이 줄어들어, 원래의 모양으로 늘려 줘야 함
- 그 방법이 Deconvolution

## Deconvolution (conv transpose)

- Convolution의 역 연산 (엄밀히는 conv의 역 연산은 존재 하지 않음)

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%209.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%209.png)

- Spatial dimension을 늘려줌

# Detection

- 바운딩 박스를 찾는 법

## R-CNN

1. takes an input image
2. extracts around 2,000 region proposals (using Selective search)
3. compute features for each proposal (using AlexNet)
4. classifies with linear SVMs
- 네트워크를 2000번 돌려야 해서 비효율적

## SPPNet

- 이미지 안에서 CNN을 한번만 돌림
- 바운딩 박스를 뽑고, 이미지 전체에 대해 convolutional feature map을 만들고, 뽑힌 바운딩 박스 위치에 해당하는 convolutional feature map의 sub tensor만 뜯어 오자

## Fast R-CNN

- For each region, get a fixed length feature from ROI pooling
- Two outputs: class and bounding-box regressor

## Faster R-CNN

- 바운딩 박스를 뽑아내는 Region Proposal도 학습하자

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%2010.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%2010.png)

- Region Proposal Network
    - 이미지에서 특정 패치가 바운딩 박스로서 의미가 있을지(물체가 있을지)를 정해줌
    - k anchor boxes
        - 미리 정해놓은 바운딩 박스의 크기.
        - 어떤 크기의 물체가 있을거 같다고 미리 알고 있는것
        - k개의 템플릿을 고정 시킴?

## YOLO

- YOLO is an extremely fast object detection algorithm
- 바운딩 박스와 클래스 분류를 한번에 함
- 이미지를 S x S grid로 나눔
    - If the center of an object falls into the grid cell, that grid cell is responsible for detection
- Each cell predicts B bounding boxes(B=5)
- Each cell predicts C class probabilities

![Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%2011.png](Day%2013%20Convolution%20Neural%20Network%202543bd5dd9264948a9a471d16b8e6fc3/Untitled%2011.png)

# Appendix

- google images download 설치하기

[https://github.com/BoostcampAITech/lecture-note-python-basics-for-ai/blob/main/codes/pytorch/00_utils/google images download.md](https://github.com/BoostcampAITech/lecture-note-python-basics-for-ai/blob/main/codes/pytorch/00_utils/google%20images%20download.md)

- Batch Normalization

[https://gaussian37.github.io/dl-concept-batchnorm/](https://gaussian37.github.io/dl-concept-batchnorm/)

- Gradient descent optimization

[http://shuuki4.github.io/deep learning/2016/05/20/Gradient-Descent-Algorithm-Overview.html](http://shuuki4.github.io/deep%20learning/2016/05/20/Gradient-Descent-Algorithm-Overview.html)

- 합성곱 역전파

[https://www.philgineer.com/2021/02/cnn-5.html](https://www.philgineer.com/2021/02/cnn-5.html)