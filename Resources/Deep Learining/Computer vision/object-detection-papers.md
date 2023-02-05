# Object Detection papers explained <!-- omit in toc -->

This is a collection of blogs, videos and personal notes that explain and dive into major Object Detection publications.
These materials helped grasped the concepts and ideas proposed in each paper.

- [**YOLOv8, 2023**](#yolov8-2023)
- [You Only Look Once: Unified, Real-Time Object Detection, **YOLOv1, 2015**](#you-only-look-once-unified-real-time-object-detection-yolov1-2015)


## **YOLOv8, 2023**
The paper is yet to come out.

YOlOv8 is a single-stage object detector, meaning one network is responsible for predicting the bounding boxes and classifying them. The YOLO series of algorithms are known for their low inference time.  
The network is built of three sections: the backbone, the neck and the head. In figure bellow, we see the full details of the network.

<div align="center">

| <img width="100%" src="https://user-images.githubusercontent.com/27466624/211974251-8de633c8-090c-47c9-ba52-4941dc9e3a48.jpg"> | 
|:--:| 
| *YOLOv8 architecture* |
| *(Source: [ open-mmlab/mmyolo](https://github.com/open-mmlab/mmyolo/tree/main/configs/yolov8))* |
</div>

**The Backbone**
The backbone network extract the important features from the images at different levels. It is composed of series of ``ConvBlock`` and ``CSPLayer_2``. The CSPLayer is made of residuals blocks whose filters are concatenated to form rich features.

**The Neck**
The neck is a feature pyramid network. This family of networks take as input the features of the backbone at low resolutions (the bottom-up pathway) and reconstruct them by up-scaling and applying convolution blocks between the layers. Lateral connection are added to ease the training (they function as residual connection) and compensate for the lost information due to the down-scaling and up-scaling.

<div align="center">

| <img width="100%" src="https://miro.medium.com/max/640/1*aMRoAN7CtD1gdzTaZIT5gA.webp"> | 
|:--:| 
| *FPN architecture* |
| *(Source: [Feature Pyramid Networks for Object Detection](https://arxiv.org/pdf/1612.03144.pdf))* |
</div>

**The head**

The head network applies convolutions to the each output of the neck layers. Meaning is looks for objects on different resolutions to identify small and large objects effectively. Its output is prediction of the bounding box coordinates, width and height, the probability and the object class.

**The loss**
The loss function is as follows:

$$
\begin{gathered}
loss = \lambda_1 L_{box} + \lambda_2 L_{cls} + \lambda_3 L_{dfl} \\

\end{gathered}
$$

The $L_{cls}$ is a Cross Entropy loss applied on the class.

The $L_{box}$ is CIoU loss, it aims to:

* Increase the overlapping area of the ground truth box and the predicted box.
* Minimize their central point distance.
* Maintain the consistency of the boxes aspect ratio.


The CIoU loss function can be defined as
$$
\mathcal{L}_{C I o U}=1-I o U+\frac{\rho^2\left(b, b^{g t}\right)}{c^2}+\alpha v .
$$
where $b$ and $b^{gt}$ denote the central points of prediction and of ground truth, $\rho$ is the Euclidean distance, and $c$ is the diagonal length of the smallest enclosing box covering the two boxes. The trade-off parameter $\alpha$ is defined as
$$
\alpha=\frac{v}{(1-I o U)+v}
$$

and $v$ measures the consistency of a aspect ratio,

$$
v=\frac{4}{\pi}\left(\arctan \frac{w^{g t}}{h^{g t}}-\arctan \frac{w}{h}\right)^2 .
$$

The $L_{dfl}$ is distributional focal loss.


## [You Only Look Once: Unified, Real-Time Object Detection](https://arxiv.org/abs/1506.02640v5), **YOLOv1, 2015**
* Articles:
  * [Review: YOLOv1 — You Only Look Once (Object Detection)](https://towardsdatascience.com/yolov1-you-only-look-once-object-detection-e1f3ffec8a89)
  * [YOLOv1 from Scratch](https://www.youtube.com/watch?v=n9_XyCGr-MI)
* Notes:
  * It outputs tensor of shape  [ $S$, $S$ , $C+5B$], where $S$ is the number of splits, $C$ is the number of classes and $B$ is the number of predicted bounding boxes per cell. In fact, The image is treated as $S*S$ cells. For each cell we predict $B$ bounding boxes and apply non maximum suppression to determine the final predictions.  
  * Loss function:
    $$
    \begin{gathered}
    \lambda_{\text {coord}} \sum_{i=0}^{S^2} \sum_{j=0}^B \mathbb{1}_{i j}^{\text {obj }}\left[\left(x_i-\hat{x}_i\right)^2+\left(y_i-\hat{y}_i\right)^2\right] \\
    +\lambda_{\text {coord }} \sum_{i=0}^{S^2} \sum_{j=0}^B \mathbb{1}_{i j}^{\text {obj }}\left[\left(\sqrt{w_i}-\sqrt{\hat{w}_i}\right)^2+\left(\sqrt{h_i}-\sqrt{\hat{h}_i}\right)^2\right] \\
    +\sum_{i=0}^{S^2} \sum_{j=0}^B \mathbb{1}_{i j}^{\text {obj }}\left(C_i-\hat{C}_i\right)^2 \\
    +\lambda_{\text {noobj }} \sum_{i=0}^{S^2} \sum_{j=0}^B \mathbb{1}_{i j}^{\text {noobj }}\left(C_i-\hat{C}_i\right)^2 \\
    +\sum_{i=0}^{S^2} \mathbb{1}_i^{\mathrm{obj}} \sum_{c \in \text { classes }}\left(p_i(c)-\hat{p}_i(c)\right)^2
    \end{gathered}
    $$

  * The first and second term compute loss over the coordinates of the bounding boxes. The sums are over the number of cells and predicted bounding boxes per cell. However only the terms where the cell has a true bounding box and the most confidante bounding box prediction.  
  * The third and the fourth term computer loss over predicted class. The former is for cells with objects and latter without objects.  
  * The last term focus on the probabilities of bounding boxes. The truth probability is set to the iou with most probable iou of the cell.  
