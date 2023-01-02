# Notes and tips from papers


* [Going deeper with convolutions](https://arxiv.org/pdf/1409.4842v1.pdf), **InceptionV1, 2014**
  * **Articles:**
    * [A Simple Guide to the Versions of the Inception Network](https://towardsdatascience.com/a-simple-guide-to-the-versions-of-the-inception-network-7fc52b863202)
  * **Notes:**
    * Salient parts in the image can have extremely large variation in size. For instance, an image with a dog can be either of the following, as shown below. The area occupied by the dog is different in each image. Because of this huge variation in the location of the information, choosing the right kernel size for the convolution operation becomes tough. A larger kernel is preferred for information that is distributed more globally, and a smaller kernel is preferred for information that is distributed more locally **--> Use filters of different sizes and concatenate them.** 
    * **Very deep networks are prone to overfitting**. It also hard to pass gradient updates through the entire network.
    * Naively stacking large convolution operations is **computationally expensive**.
    * Embedding contain a lot of compressed information even at lower dimensions, however they are harder to model. 
    * To avoid **vanishing gradient** problems, two aux heads are added to the middle of the network.
      ```
      # The total loss used by the inception net during training.
      total_loss = real_loss + 0.3 * aux_loss_1 + 0.3 * aux_loss_2
      ```
  * **Training notes:**
    * Our training used asynchronous stochastic gradient descent with 0.9 momentum, fixed learning rate schedule (decreasing the learning rate by 4% every 8 epochs).

* [Rethinking the Inception Architecture for Computer Vision](https://arxiv.org/pdf/1512.00567v3.pdf), **InceptionV2 and InceptionV3, 2015**
  * **Articles:**
    * [A Simple Guide to the Versions of the Inception Network](https://towardsdatascience.com/a-simple-guide-to-the-versions-of-the-inception-network-7fc52b863202)
  * **Notes:**
    * Reduce representational bottleneck. The intuition was that, neural networks perform better when convolutions didn’t alter the dimensions of the input drastically. Reducing the dimensions too much may cause loss of information, known as a **“representational bottleneck”**
      * **--> The filter banks in the module were expanded (made wider instead of deeper) to remove the representational bottleneck. If the module was made deeper instead, there would be excessive reduction in dimensions, and hence loss of information.**
    * Using smart factorization methods, convolutions can be made more efficient in terms of computational complexity. 
      * **--> Factorize 5x5 convolution to two 3x3 convolution operations to improve computational speed.**
      * **--> Factorize convolutions of filter size nxn to a combination of 1xn and nx1 convolutions. 33% cheaper.**

  * **Techniques applied:**
    * RMSProp Optimizer.
    * Factorized 7x7 convolutions.
    * BatchNorm in the Auxillary Classifiers.
    * Label Smoothing (A type of regularizing component added to the loss formula that prevents the network from becoming too confident about a class. Prevents over fitting). 
  * **Training notes:**
    * batch size 32 for 100 epochs.
    * **RMSProp** with decay of 0.9 and $\epsilon$ = 1.0. We used a learning rate of 0.045, decayed every two epoch using an exponential rate of 0.94.
    * **Gradient clipping** with threshold 2.0 was found to be useful to stabilize the training

* [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385), **ResNet, 2015**
  * **Articles:**
    * [Understanding and visualizing ResNets](https://towardsdatascience.com/understanding-and-visualizing-resnets-442284831be8)
    * [Implementation Pytorch](https://towardsdev.com/implement-resnet-with-pytorch-a9fb40a77448)
  * **Notes:**
    * Neural networks are approximation functions. Approximation the residual function $H(x) -x$ by multiple nonlinear layers is easier than the identity mapping $f(x) = x$. The identity mapping is closer to the optimal solution than the the zero mapping.
    * With ResNets, **the gradients can flow directly through the skip connections backwards from later layers to initial filters.**
    * The ResNet architecture follows two basic design rules. First, the number of filters in each layer is the same depending on the size of the output feature map. Second, if the feature map’s size is halved, it has double the number of filters to maintain the time complexity of each layer. 
    * Ney