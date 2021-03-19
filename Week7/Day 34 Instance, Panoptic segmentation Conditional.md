# Day 34 Instance, Panoptic segmentation / Conditional Generative model

# Instance/ Panoptic segmentation

## Instance segmentation

![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled.png)

- Mask R-CNN
    - RoIAlign 을 제안
        - interpolation을 통해 소수점 픽셀 레벨의 풀링. 정교한 feature를 뽑을 수 있다
    - Mask branch 를 추가

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%201.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%201.png)

- YOLACT (You Only Look At CoefficienTs)
    - Real time으로 Instance segementaion이 가능한 single-stage network

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%202.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%202.png)

- YolactEdge
    - 키프레임에 해당하는 프레임의 feature를 다음 프레임에 전달해서 특징맵의 계산량을 줄임

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%203.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%203.png)

## Panoptic segmentation

![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%204.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%204.png)

- UPSNet

![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%205.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%205.png)

![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%206.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%206.png)

- VPSNet

![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%207.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%207.png)

## Landmark localization

- 특정 물체에 대해 중요하다고 생각되는 특징부분을 추정하고 추적하는 것

![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%208.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%208.png)

- Hourglass network
    - Receptive field를 크게 하면서 skip connection을 통해 정확한 위치를 특정한다
    - 스택을 쌓아, 전체적인 그림과 디테일을 함께 높인다

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%209.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%209.png)

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2010.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2010.png)

- DensePose

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2011.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2011.png)

    - UV map 표현을 이용한다

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2012.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2012.png)

    - 3D surface regression branch를 도입함

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2013.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2013.png)

- RetinaFace
    - 다양한 형태의 task를 해결 할 수 있게 한다
    - 다른 task에서 오는 정보가 backbone 네트워크를 더 잘 학습시킨다

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2014.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2014.png)

## Detecting objects as keypoints

- CornerNet
    - 바운딩 박스가 {Top-left, Bottom-right} corners로 정해짐

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2015.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2015.png)

- CenterNet
    - center point가 성능에 중요하다. 바운딩 박스가 {Top-left, Bottom-right, Center}

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2016.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2016.png)

- CenterNet(2)
    - 바운딩 박스가 {Width, Height, Center}면 특정할 수 있다

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2017.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2017.png)

---

# Conditional generative model

- 주어진 condition에서 생성하는 모델

![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2018.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2018.png)

![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2019.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2019.png)

- Audio super resolution, machine translation, article generation with the title 등 사례가 있다
- image-to-image translation : Style transfer, Super resolution, Colorization ...
- Super resolution
    - 입력으로 저해상도 이미지가 주어졌을 때 고해상도 이미지를 생성하는 것

        ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2020.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2020.png)

        ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2021.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2021.png)

    - MSE-based image가 blurry한 이유 : 평균 영상을 만드는 것이 에러가 작아지는 방향이다
    - GAN은 real data와 구분을 못하게 하는 것이 목적이므로 해결이 가능하다

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2022.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2022.png)

## Image translation GANs

![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2023.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2023.png)

- Pix2Pix
    - L1 loss를 쓰는 이유 : Supervised Learning. y와 비슷한 영상을 만들기 위해, 학습을 안정시키기 위해
    - GAN loss : 성능을 향상시키기 위해

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2024.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2024.png)

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2025.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2025.png)

    - pairwise data가 필요하다 → data 구하기가 힘들다 → unpaired data만으로 생성이 가능할까?

- CycleGAN
    - non-pariwise datasets으로 도메인 translation이 가능하다

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2026.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2026.png)

    - CycleGAN loss = GAN loss(in both direction) + Cycle-consistency loss

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2027.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2027.png)

    - GAN loss
        - 양방향의 GAN loss를 가진다

        ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2028.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2028.png)

        - GAN loss만 사용하면, Mode Collapse문제가 생긴다. 하나의 아웃풋만 생성하는 문제
    - Cycle-consistency loss
        - X를 Y로 translate하고, 다시 Y를 X로 translate 했을때, 원본과 같아야 하는 loss
        - Self-supervision 방법

        ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2029.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2029.png)

- GAN 은 학습하기가 어렵다.
- GAN이 아닌 high quality output을 낼 수 있는 방법이 없을까? → Perceptual loss

![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2030.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2030.png)

- Perceptual loss
    - pre-trained된 visual perception을 활용

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2031.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2031.png)

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2032.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2032.png)

    - VGG network는 fix되어 있고 Image transform net만 학습시킨다
    - Feature reconstruction loss
        - content target의 feature와 transformed image의 feature를 비슷하게 하는 loss
        - 원본의 형태를 유지하게 하는 loss

        ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2033.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2033.png)

    - Style reconstruction loss
        - Style target에 비슷하게 해주는 loss
        - Style을 닮게 하기 위해 Style이 뭔지 디자인이 필요함 → Gram matrices
            - 풀링을 하여 공간적인 정보를 없애, 이미지 전반적인 통계적인 특성을 닮도록 설계

        ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2034.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2034.png)

## Various GAN applications

- Deepfake
    - 사람의 얼굴, 목소리를 생성하는 것

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2035.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2035.png)

    - Ethical concerns about Deepfake → Deepfake detection challenge
- Face de-identification
    - 얼굴을 약간 변형시켜 컴퓨터가 인식하지 못하게 하는 것

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2036.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2036.png)

- Face anonymization with passcode
    - 얼굴을 암호화 하는 방법

    ![Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2037.png](Day%2034%20Instance,%20Panoptic%20segmentation%20Conditional%20ec1aa272975a403abe72a7d0791eeafc/Untitled%2037.png)

- Video translation
    - Pose transfer, Video-to-video translation, Video-to-game ...