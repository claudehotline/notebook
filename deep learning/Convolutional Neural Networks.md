# Convolutional Neural Networks

## Week 1 Foundations of Convolutional Neural Networks

### 1. Convolutional Neural Networks

### 1.1 Edge Detection

Vertical edge detection
$$
filter = \begin {bmatrix}
1 & 0 & -1 \\
1 & 0 & -1 \\
1 & 0 & -1
\end {bmatrix}
$$

### 1.2 Padding

Valid: no Padding

> $n\times n -> (n-f+1)\times(n-f+1)$

"Same": Pad so that output size is the same as the input size.

>(n+2p-f+1) * (n+2p-f+1)
>
>n+2p-f+1 = n
>
>p = (f-1)/2

### 1.3 Strided Convolutions

n x n  f x f

padding p stride s

$\lfloor\frac{n+2p-f}{s} + 1\rfloor \times \lfloor\frac{n+2p-f}{s} + 1\rfloor$

### 1.4 Convolutions Over Volume

### 1.5 Pooling Layers

### 1.6 Why Convolutions

+ Parameter sharing
+ Sparsity of connections

## Week 2 Deep Convolutional Models

###  1. Case Studies

### 1. Classic Networks

### 2. ResNets

### 3. Networks in Networks and 1x1 Convolutions

### 4. Inception Network

### 5. MobileNet

### 6. EfficientNet

### 2. Practical Advice for Using ConvNets

#### 2.1 Transfer Learning

#### 2.2 Data Augmentation

## Week 3 Object Localization

### 1. Object Localization

### 2. Object Detection

+ Sliding windows detection
+ Convolutional Implementation of Sliding Windows
+ Bounding Box Predictions
+ Intersection Over Union
+ Non-max Suppression
+ Anchor Boxes
+ YOLO Algorithm
+ U-Net
