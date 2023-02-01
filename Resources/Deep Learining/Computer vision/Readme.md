# Deep Learning Computer Vision Resources

Computer vision is the field that studies the understanding and handling of images and videos by computer.  
Deep Learning has proven to be efficient in this field and able to tackle a countless number of tasks, from image classification of object tracking. 

This is a collection of resources that theory of Convolutional Neural Networks, its various architectures and techniques and recent works in the field such as Vision Transformers and Diffusion models.

## Convolutional Neural Networks:

### Cheat sheets
* ⭐ [Cheat sheet stanford CS230](https://stanford.edu/~shervine/teaching/cs-230/cheatsheet-convolutional-neural-networks)
* ⭐ [Detailed explanation stanford CS231](https://cs231n.github.io/convolutional-networks/)

### Courses

* [coursera course](https://www.coursera.org/learn/convolutional-neural-networks?irclickid=WUyTrCUidxyNRpEy9e3E-XntUkAzwmXb102tWs0&irgwc=1&utm_medium=partners&utm_source=impact&utm_campaign=2757406&utm_content=b2c)



## Image Classification
Image classification is the most basic task of computer vision. We teach an agent to provide a single label for an entire image.  
The research poured into this challenge helped create the backbones of various specialized models across different fields and tasks.

Understanding the notions and ideas behind the popular image classifier holds the key to more advanced techniques.

⭐ Collection of article and notes explaining Image Classification popular models (papers): [Image Classification papers explained](image-classification-papers.md)


An overview of the prominent architectures:
  * ⭐ [Best deep CNN architectures and their principles: from AlexNet to EfficientNet | AI Summer](https://theaisummer.com/cnn-architectures/)
  * [Illustrated: 10 CNN Architectures | by Raimi Karim | Towards Data Science](https://towardsdatascience.com/illustrated-10-cnn-architectures-95d78ace614d)
  * [Top 10 CNN Architectures Every Machine Learning Engineer Should Know](https://towardsdatascience.com/top-10-cnn-architectures-every-machine-learning-engineer-should-know-68e2b0e07201)
  * [A Simple Guide to the Versions of the Inception Network](https://towardsdatascience.com/a-simple-guide-to-the-versions-of-the-inception-network-7fc52b863202)

⭐ You can find my implementations in Pytorch: [here](https://github.com/Darkmyter/Popular-models-implemented-in-Pytorch#image-classification)

## Object Detection (WIP)

Object detection is the task of location and classifying objects on an image. The predictions are bounding box that are rough delimitations of the object's position.

⭐ Collection of article and notes explaining Object Detection popular models (papers): [Object Detection papers explained](object-detection-papers.md)


⭐ You can find my implementations in Pytorch: [here](https://github.com/Darkmyter/Popular-models-implemented-in-Pytorch#object-detection)

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
* Knowledge distillation:
  * 


## Designing CNNs

* [14 Design Patterns To Improve Your Convolutional Neural Networks](https://medium.com/topbots/14-design-patterns-to-improve-your-convolutional-neural-networks-971bb388a082), 2017

## FAQ

[List of questions and answers around Convolutional Neural Networks](faq.md)




