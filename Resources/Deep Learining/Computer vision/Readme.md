# Deep Learning Computer Vision Resources <!-- omit from toc -->

Computer vision is the field that studies the understanding and handling of images and videos by computer.  
Deep Learning has proven to be efficient in this field and able to tackle a countless number of tasks, from image classification of object tracking. 

This is a collection of resources that theory of Convolutional Neural Networks, its various architectures and techniques and recent works in the field such as Vision Transformers and Diffusion models.

- [Convolutional Neural Networks:](#convolutional-neural-networks)
  - [Cheat sheets](#cheat-sheets)
- [Image Classification](#image-classification)
- [Object Detection (WIP)](#object-detection-wip)
  - [Losses](#losses)
  - [Notions / Techniques](#notions--techniques)
  - [Toolboxes and repos:](#toolboxes-and-repos)
- [Multi Object Tracking](#multi-object-tracking)
- [Image segmentation (WIP)](#image-segmentation-wip)
- [Notions / Techniques](#notions--techniques-1)
- [Designing CNNs](#designing-cnns)
- [FAQ](#faq)
 

## Convolutional Neural Networks:

### Cheat sheets
* ⭐ [Cheat sheet stanford CS230](https://stanford.edu/~shervine/teaching/cs-230/cheatsheet-convolutional-neural-networks)
* ⭐ [Detailed explanation stanford CS231](https://cs231n.github.io/convolutional-networks/)

<!-- ### Courses

* [coursera course](https://www.coursera.org/learn/convolutional-neural-networks?irclickid=WUyTrCUidxyNRpEy9e3E-XntUkAzwmXb102tWs0&irgwc=1&utm_medium=partners&utm_source=impact&utm_campaign=2757406&utm_content=b2c) -->



## Image Classification
Image classification is the most basic task of computer vision. We teach an agent to provide a single label for an entire image.  
The research poured into this challenge helped create the backbones of various specialized models across different fields and tasks.

Understanding the notions and ideas behind the popular image classifier holds the key to more advanced techniques.

⭐ [Image Classification papers explained](image-classification-papers.md): Collection of article and notes explaining Image Classification popular models (papers).


An overview of the prominent architectures:
  * ⭐ [Best deep CNN architectures and their principles: from AlexNet to EfficientNet | AI Summer](https://theaisummer.com/cnn-architectures/)
  * [Illustrated: 10 CNN Architectures | by Raimi Karim | Towards Data Science](https://towardsdatascience.com/illustrated-10-cnn-architectures-95d78ace614d)
  * [Top 10 CNN Architectures Every Machine Learning Engineer Should Know](https://towardsdatascience.com/top-10-cnn-architectures-every-machine-learning-engineer-should-know-68e2b0e07201)
  * [A Simple Guide to the Versions of the Inception Network](https://towardsdatascience.com/a-simple-guide-to-the-versions-of-the-inception-network-7fc52b863202)

⭐ You can find my implementations in Pytorch: [here](https://github.com/Darkmyter/Popular-models-implemented-in-Pytorch#image-classification)

## Object Detection (WIP)

Object detection is the task of location and classifying objects on an image. The predictions are bounding box that are rough delimitations of the object's position.

⭐ [Object Detection papers explained](object-detection-papers.md): Collection of article and notes explaining Object Detection popular models (papers).

⭐ You can find my implementations in Pytorch: [here](https://github.com/Darkmyter/Popular-models-implemented-in-Pytorch#object-detection)

### Losses
Object detection task involves predicting a bounding box coordinates and the class of the object.  
The Cross Entropy loss is used for class term.
For the bounding box term, we find:
* IoU loss
  * It optimizes the overlapping area between the prediction $b$ and ground truth $b^{gt}$.
  * $$L_{IoU} = - logIoU(b, b^{gt})
* GIoU loss:
  * The Generalized IoU loss, attempts to bring the prediction $b$ and ground truth $b^{gt}$ closer by reducing the size the smallest convex hull  $c$ that encloses them. It optimizes the IoU too. 
  * The loss thus improves even if there is no intersection between $b$ and $b^{gt}$
  * $$L_{GIoU} = 1 - IoU + {|c- b \cup b^{gt}| \over |c|}$$
* DIoU loss
  * Distance IoU loss improves on GIoU by computing the distance between $b$ and $b^{gt}$ and the length of the diagonal $c$ of the smallest convex hull  that encloses them. This improves the convergence rate of the loss 
  * $$L_{DIoU} = 1 - IoU + {\rho^2(b,b^{gt}) \over c^2}$$
* CIoU loss:
  * Complete IoU loss adds a term to $L_{DIoU}$ that preserves the ratio of the bounding box.
  * So CIoU:
    * Increase the overlapping area of the ground truth box and the predicted box.
    * Minimize their central point distance.
    * Maintain the consistency of the boxes aspect ratio.
  * $$L_{DIoU} = 1 - IoU + {\rho^2(b,b^{gt}) \over c^2} + \alpha v \\ v=\frac{4}{\pi}\left(\arctan \frac{w^{g t}}{h^{g t}}-\arctan \frac{w}{h}\right)^2 \\ \alpha=\frac{v}{(1-I o U)+v}$$

* Focal Loss
  * Articles: [link1](https://medium.com/visionwizard/understanding-focal-loss-a-quick-read-b914422913e7), [link2](https://www.analyticsvidhya.com/blog/2020/08/a-beginners-guide-to-focal-loss-in-object-detection/)
  * The focal loss is a modified version of the CE loss. It treats the class imbalance between background and foreground objects. It down weights high probability correct prediction (i.e, background) and up weights low probability prediction. Meaning that it puts more weights on hard miss classified examples.
  * $$L_{focal} = - \alpha_t (1 - p_t)^\gamma log(p_t)$$
* Generalized focal loss:
  * ...

### Notions / Techniques
* Non-maximum suppression

### Toolboxes and repos:
* [MMdetection](https://github.com/open-mmlab/mmdetection)


## Multi Object Tracking
Multi Object Tracking is the task is identifying and tracking multiple detected objects in a video. The detection phase is performed frame by frame separately.

⭐ [Multi Object Tracking papers explained](multi-object-tracking-papers.md): Collection of article and notes explaining Multi Object Tracking popular models (papers).

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
* Knowledge distillation:
  * 


## Designing CNNs

* [14 Design Patterns To Improve Your Convolutional Neural Networks](https://medium.com/topbots/14-design-patterns-to-improve-your-convolutional-neural-networks-971bb388a082), 2017

## FAQ

[List of questions and answers around Convolutional Neural Networks](faq.md)




