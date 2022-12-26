# Computer vision Resources

## **CNN**:

### Cheat sheets
* ⭐ [Cheat sheet stanford CS230](https://stanford.edu/~shervine/teaching/cs-230/cheatsheet-convolutional-neural-networks)
* ⭐ [Detailed explanation stanford CS231](https://cs231n.github.io/convolutional-networks/)

### Architectures:

* Must Know architectures:
  * [Illustrated: 10 CNN Architectures | by Raimi Karim | Towards Data Science](https://towardsdatascience.com/illustrated-10-cnn-architectures-95d78ace614d)
  * [Top 10 CNN Architectures Every Machine Learning Engineer Should Know](https://towardsdatascience.com/top-10-cnn-architectures-every-machine-learning-engineer-should-know-68e2b0e07201)
  * ⭐ [Best deep CNN architectures and their principles: from AlexNet to EfficientNet | AI Summer](https://theaisummer.com/cnn-architectures/)
* [Dilated Convolutions and Kronecker Factored Convolutions](https://www.inference.vc/dilated-convolutions-and-kronecker-factorisation/)
* **VAE**:
  * ⭐ [The theory behind Latent Variable Models: formulating a Variational Autoencoder | AI Summer](https://theaisummer.com/latent-variable-models/)



## Notions / Techniques
* **Receptive field**
  * [Understanding the receptive field of deep convolutional networks | AI Summer](https://theaisummer.com/receptive-field/)
* **Skip connections**
  * [Intuitive Explanation of Skip Connections in Deep Learning | AI Summer ](https://theaisummer.com/skip-connections/)
* **Batch normalization**:
  * BN’s parameters (means and variances) need adjustment between pre-training and transfer. On the other hand, GN doesn’t depend on any parameter states. Another reason is that BN uses batch-level statistics, which become unreliable for distributed training in small devices like TPU’s. A 4K batch distributed across 500 TPU’s means 8 batches per worker, which does not give a good estimation of the statistics. By changing the normalization technique to GN+WS they avoid synchronization across workers.



## **Image segmentation**
* [Semantic Segmentation in the era of Neural Networks | AI Summer](https://theaisummer.com/Semantic_Segmentation/) 

## **Object detection**

### Architectures

* YOLO:
  * [YOLO - You only look once (Single shot detectors) | AI Summer](https://theaisummer.com/YOLO/)

### Toolboxes and repos:
* [MMdetection](https://github.com/open-mmlab/mmdetection)

### Notions / Techniques
* Non-maximum suppression

## FAQ

* How to use the filter size?
* what’s group normalization ([In-layer normalization techniques for training very deep neural networks | AI Summer](https://theaisummer.com/normalization/#group-normalization-2018)) ? what’s weight standardization ([In-layer normalization techniques for training very deep neural networks | AI Summer](https://theaisummer.com/normalization/#weight-standardization-2019))?  Difference from BN ?
* How to calculate Conv output size and how to find the right pad and stride ?
* stide or maxpool ?


## Notes
* Note that all pre-trained models expect input images normalized in the same way, i.e. mini-batches of 3-channel RGB images of shape (N, 3, H, W), where N is the number of images, H and W are expected to be at least 224 pixels. The images have to be loaded in to a range of [0, 1] and then normalized using mean = [0.485, 0.456, 0.406] and std = [0.229, 0.224, 0.225]

