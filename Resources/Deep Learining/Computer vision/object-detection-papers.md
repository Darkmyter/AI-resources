# Object Detection papers explained <!-- omit in toc -->

This is a collection of blogs, videos and personal notes that explain and dive into major Object Detection publications (Backbones of modern architectures).
These materials helped grasped the concepts and ideas proposed in each paper.


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
