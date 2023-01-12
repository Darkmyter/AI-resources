# Computer vision Resources

## **CNN**:

## Courses

* https://www.coursera.org/learn/convolutional-neural-networks?irclickid=WUyTrCUidxyNRpEy9e3E-XntUkAzwmXb102tWs0&irgwc=1&utm_medium=partners&utm_source=impact&utm_campaign=2757406&utm_content=b2c

### Cheat sheets
* ⭐ [Cheat sheet stanford CS230](https://stanford.edu/~shervine/teaching/cs-230/cheatsheet-convolutional-neural-networks)
* ⭐ [Detailed explanation stanford CS231](https://cs231n.github.io/convolutional-networks/)

### Architectures:

* Overviews:
  * [Illustrated: 10 CNN Architectures | by Raimi Karim | Towards Data Science](https://towardsdatascience.com/illustrated-10-cnn-architectures-95d78ace614d)
  * [Top 10 CNN Architectures Every Machine Learning Engineer Should Know](https://towardsdatascience.com/top-10-cnn-architectures-every-machine-learning-engineer-should-know-68e2b0e07201)
  * ⭐ [Best deep CNN architectures and their principles: from AlexNet to EfficientNet | AI Summer](https://theaisummer.com/cnn-architectures/)
* [Dilated Convolutions and Kronecker Factored Convolutions](https://www.inference.vc/dilated-convolutions-and-kronecker-factorisation/)
* **InceptionNet|GoogleNet**:
  * [A Simple Guide to the Versions of the Inception Network](https://towardsdatascience.com/a-simple-guide-to-the-versions-of-the-inception-network-7fc52b863202)
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


### Design tips

* [14 Design Patterns To Improve Your Convolutional Neural Networks](https://medium.com/topbots/14-design-patterns-to-improve-your-convolutional-neural-networks-971bb388a082), 2017

## FAQ

* How to use the filter size?


* How compute number of parameters in a conv layer?
  * number of parameters is $(k*k*ch_{in}) * ch_{out} + ch_{out}$, where $ch$ is the number of channels.
* How to compute the output size of conv  layer? How to choose padding and stride size ?
  * The input spatial size (HxW) is usually preserved between layer. Using this formula, $out=(in+2∗p−k)/s +1$, where $out$ and $in$ are the spatial dims of output and input, $k$ is the kernel size, $p$ is the padding size and $s$ is the stride size, we can choose $p$ and $s$ based on the desired kernel size. $s$ is usually set to 1 or 2, higher values are rare in practice.  
  When $s=1$ and we want to preserve the size, we can use this simpler formula: $p=(k-1)/2$
* How to compute the output size of Pool layer?
  * $out=(in−k)/s +1$, where $out$ and $in$ are the spatial dims of output and input, $k$ is the kernel size, $s$ is the stride size. The depth remains unchanged. Common hyper-parameters are: $k=3, s=2$ and $k=2, s=2$. 
* How to perform flatten ?
* what’s group normalization ([In-layer normalization techniques for training very deep neural networks | AI Summer](https://theaisummer.com/normalization/#group-normalization-2018)) ? what’s weight standardization ([In-layer normalization techniques for training very deep neural networks | AI Summer](https://theaisummer.com/normalization/#weight-standardization-2019))?  Difference from BN ?
* How to calculate Conv output size and how to find the right pad and stride ?
* stide or maxpool ?
* Should I use `inplace=True` in pytroch?
  * **`inplace=True` means that it will modify the input directly, without allocating any additional output.** It can sometimes slightly decrease the memory usage, but may not always be a valid operation (because the original input is destroyed). However, if you don’t see an error, it means that your use case is valid.
  * From pytorch [website](https://pytorch.org/docs/master/notes/autograd.html#in-place-operations-on-variables): **Performing inplace operations on the input of any of the functions is forbidden** as they may lead to unexpected side-effects. PyTorch will throw an error if the input to a pack hook is modified inplace but does not catch the case where the input to an unpack hook is modified inplace.

* In convolutional neural networks, what effect does the size (e.g. 3x3, 5x5, 7x7) of the convolution kernel have on the architecture of the convolutional neural networks?

  * Convolutional neural networks work on 2 assumptions:
    * Low level features are local
    * What's useful in one place will also be useful in other places
  
  * Kernel size should be determined by how strongly we believe in those assumptions for the problem at hand.  
  * In one extreme case where we have 1x1 kernels, we are essentially saying low level features are per-pixel, and they don't affect neighbouring pixels at all, and that we should apply the same operation to all pixels.  
  * In the other extreme, we have kernels the size of the entire image. In this case the CNN essentially becomes fully connected, and stops being a CNN, and we are no longer making any assumption on low level feature locality.  
  * In practice, this is often done by just trying a few kernel sizes, and see what works best.

* What is time complexity and why preserve it between layers?


* Can we go lower than 3x3 receptive size filters if it provides the same benefits as factorizing 5x5 or 7x7?
  * The answer is “No.” 3 x 3 is considered to be the smallest size to capture the notion of left to right, top to down, etc. So lowering the filter size further could impact the ability of the model to understand the spatial features of the image.

* What's the use of 1x1 convolutions?
  * 1×1 convolution can be introduced as bottleneck layer to reduce the number of input feature-maps or increase them. This was applied in DenseNet and Inception networks.

## Notes
* Note that all pre-trained models expect input images normalized in the same way, i.e. mini-batches of 3-channel RGB images of shape (N, 3, H, W), where N is the number of images, H and W are expected to be at least 224 pixels. The images have to be loaded in to a range of [0, 1] and then normalized using mean = [0.485, 0.456, 0.406] and std = [0.229, 0.224, 0.225]
* **Getting rid of pooling.** Many people dislike the pooling operation and think that we can get away without it. For example, [Striving for Simplicity: The All Convolutional Net](https://arxiv.org/abs/1412.6806) proposes to discard the pooling layer in favor of architecture that only consists of repeated CONV layers. To reduce the size of the representation they suggest using larger stride in CONV layer once in a while. Discarding pooling layers has also been found to be important in training good generative models, such as variational autoencoders (VAEs) or generative adversarial networks (GANs). It seems likely that future architectures will feature very few to no pooling layers.
* **Prefer a stack of small filter CONV to one large receptive field CONV layer.** Suppose that you stack three 3x3 CONV layers on top of each other (with non-linearities in between, of course). In this arrangement, each neuron on the first CONV layer has a 3x3 view of the input volume. A neuron on the second CONV layer has a 3x3 view of the first CONV layer, and hence by extension a 5x5 view of the input volume. Similarly, a neuron on the third CONV layer has a 3x3 view of the 2nd CONV layer, and hence a 7x7 view of the input volume. Suppose that instead of these three layers of 3x3 CONV, we only wanted to use a single CONV layer with 7x7 receptive fields. These neurons would have a receptive field size of the input volume that is identical in spatial extent (7x7), but with several disadvantages. First, the neurons would be computing a linear function over the input, while the three stacks of CONV layers contain non-linearities that make their features more expressive. Second, if we suppose that all the volumes have C
channels, then it can be seen that the single 7x7 CONV layer would contain $C*(7*7*C)=49C^2$ parameters, while the three 3x3 CONV layers would only contain $3*(C*(3*3
*C))=27C^2$ parameters. Intuitively, stacking CONV layers with tiny filters as opposed to having one CONV layer with big filters allows us to express more powerful features of the input, and with fewer parameters. As a practical disadvantage, we might need more memory to hold all the intermediate CONV layer results if we plan to do backpropagation.


