# Deep Learning Computer Vision Resources

Computer vision is the field that studies the understanding and handling of images and videos by computer.  
Deep Learning has proven to be efficient in this field and able to tackle a countless number of tasks, from image classification of object tracking. 

This is a collection of resources that theory of Convolutional Neural Networks, its various architectures and techniques and recent works in the field such as Vision Transformers and Diffusion models.

## **Convolutional Neural Networks**:

### Cheat sheets
* ⭐ [Cheat sheet stanford CS230](https://stanford.edu/~shervine/teaching/cs-230/cheatsheet-convolutional-neural-networks)
* ⭐ [Detailed explanation stanford CS231](https://cs231n.github.io/convolutional-networks/)

### Courses

* [coursera course](https://www.coursera.org/learn/convolutional-neural-networks?irclickid=WUyTrCUidxyNRpEy9e3E-XntUkAzwmXb102tWs0&irgwc=1&utm_medium=partners&utm_source=impact&utm_campaign=2757406&utm_content=b2c)



## Image Classification
Image classification is the most basic task of computer vision. We teach an agent to provide a single label for an entire image.  
The research poured into this challenge helped create the backbones of various specialized models across different fields and tasks.

Understanding the notions and ideas behind the popular image classifier holds the key to more advanced techniques.


An overview of the prominent architectures:
  * ⭐ [Best deep CNN architectures and their principles: from AlexNet to EfficientNet | AI Summer](https://theaisummer.com/cnn-architectures/)
  * [Illustrated: 10 CNN Architectures | by Raimi Karim | Towards Data Science](https://towardsdatascience.com/illustrated-10-cnn-architectures-95d78ace614d)
  * [Top 10 CNN Architectures Every Machine Learning Engineer Should Know](https://towardsdatascience.com/top-10-cnn-architectures-every-machine-learning-engineer-should-know-68e2b0e07201)
  * [A Simple Guide to the Versions of the Inception Network](https://towardsdatascience.com/a-simple-guide-to-the-versions-of-the-inception-network-7fc52b863202)

⭐ All you need to understand the papers: [Image Classification papers explained](image-classification-papers.md)

## Object detection (WIP)

Object detection is the task of location and classifying objects on an image. The predictions are bounding box that are rough delimitations of the object.

### Architectures

* YOLO:
  * [YOLO - You only look once (Single shot detectors) | AI Summer](https://theaisummer.com/YOLO/)

### Toolboxes and repos:
* [MMdetection](https://github.com/open-mmlab/mmdetection)

### Notions / Techniques
* Non-maximum suppression


## Image segmentation (WIP)


* [Semantic Segmentation in the era of Neural Networks | AI Summer](https://theaisummer.com/Semantic_Segmentation/) 


## Notions / Techniques
* Receptive field
  * [Understanding the receptive field of deep convolutional networks | AI Summer](https://theaisummer.com/receptive-field/)
* Skip connections
  * [Intuitive Explanation of Skip Connections in Deep Learning | AI Summer ](https://theaisummer.com/skip-connections/)
* Dilated Convolutions:
  * [Dilated Convolutions and Kronecker Factored Convolutions](https://www.inference.vc/dilated-convolutions-and-kronecker-factorisation/)
* Batch normalization:
  * BN’s parameters (means and variances) need adjustment between pre-training and transfer. On the other hand, GN doesn’t depend on any parameter states. Another reason is that BN uses batch-level statistics, which become unreliable for distributed training in small devices like TPU’s. A 4K batch distributed across 500 TPU’s means 8 batches per worker, which does not give a good estimation of the statistics. By changing the normalization technique to GN+WS they avoid synchronization across workers.
* VAE:
  * ⭐ [The theory behind Latent Variable Models: formulating a Variational Autoencoder | AI Summer](https://theaisummer.com/latent-variable-models/)


## Designing CNNs

* [14 Design Patterns To Improve Your Convolutional Neural Networks](https://medium.com/topbots/14-design-patterns-to-improve-your-convolutional-neural-networks-971bb388a082), 2017

## FAQ

[List of questions and answers around Convolutional Neural Networks](faq.md)



## Notes
* Note that all pre-trained models expect input images normalized in the same way, i.e. mini-batches of 3-channel RGB images of shape (N, 3, H, W), where N is the number of images, H and W are expected to be at least 224 pixels. The images have to be loaded in to a range of [0, 1] and then normalized using mean = [0.485, 0.456, 0.406] and std = [0.229, 0.224, 0.225]
* **Getting rid of pooling.** Many people dislike the pooling operation and think that we can get away without it. For example, [Striving for Simplicity: The All Convolutional Net](https://arxiv.org/abs/1412.6806) proposes to discard the pooling layer in favor of architecture that only consists of repeated CONV layers. To reduce the size of the representation they suggest using larger stride in CONV layer once in a while. Discarding pooling layers has also been found to be important in training good generative models, such as variational autoencoders (VAEs) or generative adversarial networks (GANs). It seems likely that future architectures will feature very few to no pooling layers.
* **Prefer a stack of small filter CONV to one large receptive field CONV layer.** Suppose that you stack three 3x3 CONV layers on top of each other (with non-linearities in between, of course). In this arrangement, each neuron on the first CONV layer has a 3x3 view of the input volume. A neuron on the second CONV layer has a 3x3 view of the first CONV layer, and hence by extension a 5x5 view of the input volume. Similarly, a neuron on the third CONV layer has a 3x3 view of the 2nd CONV layer, and hence a 7x7 view of the input volume. Suppose that instead of these three layers of 3x3 CONV, we only wanted to use a single CONV layer with 7x7 receptive fields. These neurons would have a receptive field size of the input volume that is identical in spatial extent (7x7), but with several disadvantages. First, the neurons would be computing a linear function over the input, while the three stacks of CONV layers contain non-linearities that make their features more expressive. Second, if we suppose that all the volumes have C
channels, then it can be seen that the single 7x7 CONV layer would contain $C*(7*7*C)=49C^2$ parameters, while the three 3x3 CONV layers would only contain $3*(C*(3*3
*C))=27C^2$ parameters. Intuitively, stacking CONV layers with tiny filters as opposed to having one CONV layer with big filters allows us to express more powerful features of the input, and with fewer parameters. As a practical disadvantage, we might need more memory to hold all the intermediate CONV layer results if we plan to do backpropagation.


