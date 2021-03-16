# Day 32 Image classification 2 / Semantic segmentation

# Image classification 2

## Problems with deeper layers

- 깊은 네트워크는 많은 features를 학습한다
    - 큰 recptive fields
    - more capacity and non-linearity
- 그러나 깊은 네트워크 일수록 optimize하기 힘들다
    - Gradient vanishing / exploding
    - Computationally complex
    - Degradation problem

## GoogLeNet

- Inception module을 사용한다
    - 하나의 레이어에서 다양한 크기의 convolution filter를 사용함
    - 채널축으로 concatenation함
    - 계산량이 많아지는 문제가 있다
    - 1x1 convolution을 이용해 채널 수를 줄인다

    ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled.png)

- 1x1 convolution

![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%201.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%201.png)

- Overall architecture
    - Stem network: vanilla convolution networks
    - Stacked inception modules
    - Auxiliary classifiers
    - Classifier output(a single FC layer)

    ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%202.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%202.png)

- Auxiliary classifier
    - backpropagation시 gradient vanishing문제가 있어 중간 결과로 부터 loss를 전달할 수 있다
    - 학습시에만 사용하고, 테스트때는 사용하지 않는다

        ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/_2021-03-10__1.10.41.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/_2021-03-10__1.10.41.png)

## ResNet

- Revolutions of depth

![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%203.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%203.png)

- Degradation problem
    - 네트워크 깊이가 깊어질수록, accuracry gets saturated → degrade rapidly
    - 오버피팅 문제가 아니다. 문제는 optimization

    ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%204.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%204.png)

- Hypothesis
    - Plain layer : 레이어가 깊을수록, H(x)를 바로 학습하는것은 어렵다
    - Residual block: 대신, residual를 학습한다
        - Target function    : $H(x) = F(x) + x$
        - Residual function : $F(x) = H(x) - x$

        ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%205.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%205.png)

    - A solution : Shortcut connection
    - 학습시, gradient는 주로 shorter paths에 관련이 있다
    - $2^n$가지의 path가 있고, 다양한 경로를 통해 복잡한 mapping을 학습할 수 있다
- Overall architecture
    - He initialization
    - Additional conv layer at the beginning
    - Stack residual blocks(every block has two 3x3 conv layers)
    - Batch norm after every conv layer
    - 블럭을 나눠 단계마다 공간해상도를 줄이고 채널 수를 2배로 늘린다
    - 최종 출력은 하나의 FC layer

    ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%206.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%206.png)

## DenseNet

- 각 layer의 모든 output을 채널 축으로 concatenation
    - Alleviate vanishing gradient problem
    - Strengthen feature propagation
    - Encourage the reuse of features

    ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%207.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%207.png)

## SENet

- Attention across channels
- Squeeze and excitation operations
    - Squeeze : global average pooling을 통해 각채널의 공간정보를 없애고 채널축의 분포를 구함
    - Excitation : FC layer를 통해 채널간 연관성을 고려하여 attention score를 구함

    ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%208.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%208.png)

- 입력 attention과 weight를 가지고 중요도가 낮은것은 없애고 높은것은 더 크게 scaling

## EfficientNet

- Building deep, wide, and high resolution networks in an efficient way
    - 3개의 factor를 잘 조절하여 성능을 높이는 방법을 제시

![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%209.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%209.png)

## Deformable convolution

![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2010.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2010.png)

- 사람이나 동물의 요소들의 상대적인 위치가 바뀌는 것을 고려하기 위해
- 2D spatial offset prediction for irregular convolution
- Irregular grid sampling with 2D spatial offests
- Implemented by standard CNN and grid sampling with 2D offsets

## CNN backbones

- GoogLeNet은 효율적인 모델이지만, 학습과 구현이 난해하다
- VGGNet과 ResNet이 backbone 네트워크로 많이 쓰인다
    - Constructed with simple 3x3 conv layers

---

# Semantic segmentation

- pixel단위로 classification 하는 것
- Don't care about instances. Only care about semantic category
    - instance segmentation이 따로 있다

## Fully Convolutional Networks(FCN)

- The first end-to-end architecture for semantic segmentation
- 인풋 사이즈에 대응하는 segmentation map을 아웃풋으로 얻는다

![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2011.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2011.png)

- Fully connected vs Fully convolutional
    - Fully connected layer : 공간정보를 고려하지 않고 fixed dimensional vector로 출력해준다
    - Fully convolutional layer : spatial coordinates를 유지한 채로 classification map형태인 아웃풋

        ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/_2021-03-10__1.52.31.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/_2021-03-10__1.52.31.png)

- flattening을 하면 공간정보가 사라진다
- 채널축으로 flattening을 하면 공간정보를 유지할 수 있다
- 그것이 1x1 convolution과 같은 연산

![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2012.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2012.png)

- 넓은 receptive field를 고려하기 위해 풀링 레이어가 적용되고 그 결과 해상도가 매우 작아지게 된다
- 이를 해결하기 위해 upsampling을 사용한다
- Upsampling
    - Transposed convolution
        - Transposed convolutions work by swapping the forward and backward passes of convolution
        - overlap 되는 부분 때문에 Checkerboard artifact가 발생한다

        ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2013.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2013.png)

    - Upsample and convolution
        - transposed covolution의 overlap issue를 피하는 방법
        - Decompose into spatial upsampling and feature convolution
            - {Nearest-neighbor(NN), Bilinear} interpolation followed by convolution
                - interpolation을 통해 해상도를 키우고, 학습을 위한 convolution을 적용함

            ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2014.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2014.png)

- 높은 레이어에 있는 activation map을 upsampling을 통해 해상도를 키우고, 중간층의 map들을 upsampling해서 가져와 concatenation

![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2015.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2015.png)

![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2016.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2016.png)

## Hypercolumns for object segmentation

- Very similar to FCN
- Difference : Apply to each bounding box

![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2017.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2017.png)

## U-Net

- Built upon "fully convolutional networks"
- Predict a dense map by concatenating feature maps from contracting path
- Overall architecture

    ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2018.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2018.png)

    - Contracting path는 일반적인 CNN
    - Expanding path
        - 단계별로 activation map의 해상도를 늘려주고 채널 사이즈를 줄여 감
        - Repeatedly appliyng 2x2 convolutions
        - Concatenating the corresponding feature maps from the contracting path
    - Concatenation of feature maps provides localized information
        - 경계선이나 공간적으로 중요한 정보를 뒤 쪽 layer에 바로 전달함
- feature map의 spatial size가 홀수이면?
    - upsampling에서 정보손실이 발생함
    - 중간에 어떤 layer에서도 홀수 해상도의 activation map이 나오지 않도록 유의해야 함

## DeepLab

- Conditional Random Fields(CRFs)
    - 영상처리 관련한 후처리 방법이다
    - 픽셀과 픽셀을 연결하여 그래프로 보는 방법
    - rough한 score map과 이미지의 edge를 활용하여 score map이 확산이 일어나게 하는 알고리즘
- Dilated convolution
    - Atrous convolution
    - 더 넓은 영역을 고려하면서, 파라미터 수는 늘어나지 않는다
    - receptive field가 exponential 하게 증가함

    ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2019.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2019.png)

- Depthwise separable convolution

    ![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2020.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2020.png)

    - No. parameters
        - Standard conv : $D^2_KMND^2_F$
        - Depthwise separable conv : $D_K^2MD_F^2+MND_F^2$
- Deeplab v3+

![Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2021.png](Day%2032%20Image%20classification%202%20Semantic%20segmentatio%203a0b8a3c9e974c8e8b013106cf7c21ae/Untitled%2021.png)