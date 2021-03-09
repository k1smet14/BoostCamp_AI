# Day 31 Image Classification1 / Annotation data efficient learning

# What is computer vision?

- Computer Graphics(Rendering)  VS  Computer Vision(Inverse rendering)
- Visual perception & intelligence
    - input : visual data (image or video)
- also, computer vision includes understanding human visual perception capability
- 기존 machine learning에서는 feature extraction을 사람이 했다
    - 딥러닝 에서는 feature extraction도 학습해서 사람의 오류를 최소화

# Image Classification

- 모든 데이터를 가지고 있다면, classification 문제는 k Nearest Neighbors(k-NN)으로 풀 수 있다
    - 하지만 Time complexity, Memory complexity가 O(n)으로 한계가 있다
    - Neural Networks로 Compress 하자
- 레이어가 1층인 심플한 모델을 생각해보자(perceptron)
    - 단순 모델은 평균 이미지 외에는 표현이 불가능
    - 학습시 이미지 전체의 특징을 적용하므로, 테스트시 이미지의 일부만 사용할 경우 잘 안된다
    - Convolution Nueral Networks가 해결방법!
- CNN
    - Local feature learning
    - Parameter sharing

    ![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled.png)

    - CNN is used a backbone of many CV tasks

# CNN architectures for image classification 1

![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%201.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%201.png)

## Alexnet

- Bigger
- Trained with ImageNet
- Using ReLU and regularization technique(dropout)
- Overall archietecture

![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%202.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%202.png)

![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%203.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%203.png)

- Linear layer에 넣기 위해 벡터화가 필요. Alexnet에선 flatten을 사용
- Local Response Normalization(LRN)
    - 지금은 안쓰는 방법
    - 명암에 normalization을 하는 것
    - Batch normalization으로 대체 됨
- 11x11 convolution filter
    - 넓은 receptive field를 커버하기 위해서 쓴다
    - 최신 모델들은 이런 큰 filter를 사용하지 않는다
- Receptive field in CNN
    - The region in the input space that a particular CNN feature is looking at
        - Suppose K$\times$K conv, filters with stride 1, and a pooling layer of size P$\times$P,
            - then a value of each unit in the pooling layer depends on an input patch of size:

                (P+K-1) $\times$ (P+K-1)

                ![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%204.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%204.png)

## VGGNet

![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%205.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%205.png)

- Deeper architecture
- Simpler archietecture
- Better performance
- Better generalization
- Key design choices
    - 3x3 convolution filters with stride 1
    - 2x2 max pooling operations
        - 작은 커널 사이즈라도 스택을 많이 쌓으면 큰 receptive field를 얻을 수 있다
        - Deeper with more non-linearities
        - Fewer parameters

---

# Annotation data efficient learning

## Data augmentation

- Dataset is (almost) always biased
- Images taken by camera(training data) $\ne$ real data

![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%206.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%206.png)

- The training dataset is sparse samples of real data
- The training dataset and real data always have gap
- 이러한 문제들 때문에 data augmentaion을 해서 real data와의 gap을 채워준다
- Image data augmentation
    - Applying various image transformations to the dataset
        - Crop, Shear, Brightness, Perspective, Rotate ...
- Brightness adjustment

```python
def brightness_augmentation(img):
		# numpy array img has RGB values(0~255) for each pixel
		img[:,:,0] = img[:,:,0] + 100
		img[:,:,1] = img[:,:,1] + 100
		img[:,:,2] = img[:,:,2] + 100

		img[:,:,0][img[:,:,0]>255] = 255 # clip R values over 255
		img[:,:,1][img[:,:,1]>255] = 255
		img[:,:,2][img[:,:,2]>255] = 255
		return img
```

- Rotate, flip

```python
img_rotated = cv2.rotate(image, cv2.ROTATE_90_CLOCKWISE)
img_flipped = cv2.rotate(image, cv2.ROTATE_180)
```

- Crop

```python
y_start = 500
crop_y_size = 400
x_start = 300
crop_x_size = 800
img_cropped = image[y_start:y_start+crop_y_size, x_start:x_start+crop_x_size, :]
```

- Affine transformation
    - Preserves 'line', 'length ratio', and 'parallelism' in image
        - For example, transforming a rectangle into a parallelogram
    - 6개의 행렬값이 필요하다
    - 대응하는 점 3개의 값을 넣어줘서 변환행렬값을 얻을 수 있다

```python
rows, cols, ch = image.shape
pts1 = np.float32(([50,50],[200,50],[50,200]])
pts2 = np.float32(([10,100],[200,50],[100,250]])
M = cv2.getAffineTransform(pts1,pts2)
shear_img = cv2.warpAffine(image, M, (cols,rows))
```

- CutMix
    - Mixing both images and labels

    ![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%207.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%207.png)

- RandAugment
    - 여러가지 영상처리 법을 조합하여 augmentation할수가 있다
    - Rand하게 augmetation기법들을 샘플링해서 성능이 좋은 것을 찾는 알고리즘

## Transfer learning

- high-qulaity dataset을 모으는 것은 비싸고 어렵다
    - transfer learning을 하자
- 한 데이터셋에서 배운 기을 다른 데이터셋에 활용하는 방법
- Approach 1 : Transfer knowledge from a pre-trained task to a new task
    - 데이터가 정말 작을때 사용

![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%208.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%208.png)

- Approach 2 : Fine-tuning the whole model

![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%209.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%209.png)

### Knowledge distillation (Teacher-student learning)

- 'Distillate' knowledge of a trained model into another smaller model
- Used for model compression(Mimicking what a larger model knows)
- Also, used for psuedo-labeling(Generating pseudo-labels for an unlabeld dataset)
- Teacher-student network structure

![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2010.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2010.png)

- label이 있는 데이터가 있을때,
    - When labeled data is available, can leverage labeled data for training(Student Loss)
    - Distillation loss to 'predict similar outputs with the teacher model'
    - Semantic information is not considered in distillation
        - 각각의 의미가 중요한 것이 아닌, 전체적인 개형을 따라하게 하는게 중요하다는 뜻

        ![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2011.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2011.png)

- Hard label vs. Soft label
    - Hard label(One-hot vector)
        - Typically obtained from the dataset
    - Soft label
        - Typically output of the model(=inference result)
        - Regard it as "knowledge". 모델이 어떻게 생각하는지 관찰할 수 있다
- Softmax with temperature(T)

    ![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2012.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2012.png)

    - softmax를 취하면 극단적으로 0과 1에 가까운 값이 된다
    - temperature를 통해 중간 값을 가지게 되고, 지식을 표현하는 정보를 더 많이 가지고 있다는 원리
- Intuition about distillation loss and student loss
    - Distillation Loss
        - KLdiv(Soft label, Soft prediction)
        - Loss = difference between the teacher and student network's inference
        - Learn what teacher network knows by mimicking
    - Student Loss
        - CrossEntropy(Hard label, Soft prediction)
        - Loss = difference between the studnet network's inference and true label
        - Learn the "right answer"

## Leveraging unlabeled dataset for training

- Semi-supervised learning
    - There are lots of unlabeled data
    - Semi-supervised learning with pseudo labeling

![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2013.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2013.png)

- Self-training
    - Augmentation + Teacher-Student networks + semi-supervised learning
    - Self-training with noisy student

    ![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2014.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2014.png)

    ![Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2015.png](Day%2031%20Image%20Classification1%20Annotation%20data%20effic%20b3387073ccd04f798ec8cd6f591d8cef/Untitled%2015.png)