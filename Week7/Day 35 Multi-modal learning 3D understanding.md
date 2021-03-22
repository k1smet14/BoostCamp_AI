# Day 35 Multi-modal learning / 3D understanding

# Multi-modal learning

- 다양한 데이터 타입, 형태 혹은 다양한 데이터 특성을 학습하는 방법

![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled.png)

- 데이터 표현 방식이 달라 문제 해결이 어렵다
- 정보량도 unbalance하고 feature spaces도 다르다
- 특정 modality에 biased될 가능성이 높다 → 쉬운 데이터에 대해 주로 학습해버리는 문제

## Multi-modal tasks(1) - Visual data & Text

### Joint embedding

- Image tagging

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%201.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%201.png)

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%202.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%202.png)

    - 두 embedding의 차원이 같아야 함
    - matching되는 pair는 거리가 가깝게, non-matching pairs는 거리가 멀게 embedding 을 학습한다
    - Multi-modal analogy를 학습할 수 있다

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%203.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%203.png)

- Image & food recipe retrieval

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%204.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%204.png)

    - recipe는 순서가 있고 텍스트이다 → RNN
    - image는 CNN backbone 네트워크로.

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%205.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%205.png)

### Cross modal translation

- Image captioning

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%206.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%206.png)

    - Show and tell
        - image 는 CNN으로, sentence는 RNN(LSTM)으로

        ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%207.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%207.png)

    - Show, attend, and tell
        - image에서 중요한 부분을 reference로 문장을 생성하여 성능을 높이는 방법
        - 공간정보를 유지하는 feature map을 RNN에 넣어줌
        - word 를 생성할 때 마다, feature map을 참조함

        ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%208.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%208.png)

        - 중간 연산자는 weighted sum

        ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%209.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%209.png)

        ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2010.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2010.png)

- Text-to-image by generative model
    - one to many mapping model

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2011.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2011.png)

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2012.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2012.png)

    - 가우시안 랜덤 코드를 붙여줘서 다양한 output 결과가 나오게 해준다

### Cross modal reasoning

- Visual question answering
    - 영상이 주어지고, 질문이 주어졌을때, 답을 하는 task

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2013.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2013.png)

## Multi-modal tasks (2) - Visual data & Audio

### Sound representation

- waveform을 Acoustic feature로 변환을 해주고 사용한다

![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2014.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2014.png)

- Short-time Fourier transform(STFT)
    - 짧은 window 구간 내에서만 FT한다
    - Hamming window를 곱해줘서 edge에 생기는 아티팩트를 줄여줌

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2015.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2015.png)

- Spectrogram
    - 시간에 따라 spectrums를 쌓아준다

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2016.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2016.png)

    - Melspectogram, MFCC 가 있다

### Joint embedding

- Scene recogition by sound

![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2017.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2017.png)

- SoundNet
    - Video는 pre-trained network에, audio는 1D CNN에 넣어준다
    - Video branch net는 fix 되어 있고, Audio branch를 학습시킨다
        - teacher-student 방식

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2018.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2018.png)

    - target task에 응용할 때, pool5 feature를 뽑아서 사용한다
        - pool5가 좀더 generalize한 정보를 가지고 있다고 주장

### Cross modal translation

- Speech2Face
    - 음성으로부터 얼굴을 추정하는 task
    - Module networks를 사용함

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2019.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2019.png)

- Image-to-speech synthesis

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2020.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2020.png)

    - image에서 sub-word units을 추출한다
    - Unit-to-Speech model을 학습한다

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2021.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2021.png)

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2022.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2022.png)

### Cross modal reasoning

- Sound source localization
    - 소리 input과 image가 주어졌을때 소리가 나는 위치를 localization하는 task

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2023.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2023.png)

    - Fully supervised version, Unsupervised version, Semi-supervised version을 만들수 있다

- Speech separation
    - Clean spectogram과 enhanced spectogram 사이의 L2 loss를 사용한다
    - Training data는 두개의 clean speech videos를 concat하고 음성을 더해서 활용한다

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2024.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2024.png)

- Lip movements generation, Autopilot ...

---

# 3D understanding

- 참고도서 - Multiple View Geometry in computer vision (PDF available online)
- 3D data representation
    - 3D의 표현은 unique하지 않다

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2025.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2025.png)

- 3D datasets
    - ShapeNet
        - Large sale synthetic objects(51,300 3D models with 55 categories)
    - PartNet
        - Fine-grained dataset, useful for segmetation(573,585 part instances in 26,671 3D models)

        ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2026.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2026.png)

    - SceneNet
        - 5 million RGB-Depth synthetic indoor images

        ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2027.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2027.png)

    - ScanNet
        - RGB-Depth dataset with 2.5 million vies obtained from more than 1500 scans

        ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2028.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2028.png)

    - Outdoor 3D scene datasets (typically for autonomous vehicle applications)

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2029.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2029.png)

## 3D tasks

- 3D object recognition
    - 3D CNN을 사용하여 인식

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2030.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2030.png)

- 3D object detection

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2031.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2031.png)

- 3D semantic segmentation

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2032.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2032.png)

- Conditional 3D generation
    - Mesh R-CNN
        - 2d input image에서 3D meshs를 출력

        ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2033.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2033.png)

        ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2034.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2034.png)

- More complex 3D reconstruction models
    - multiple sub-problems로 decomposing하는 것

    ![Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2035.png](Day%2035%20Multi-modal%20learning%203D%20understanding%20e052c2133c124b0997f39549fc02b26a/Untitled%2035.png)

## 3D application example

- Photo refocusing
    - 하나의 이미지를 depth map을 이용하여 defocusing, refocusing 하는 task