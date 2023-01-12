# Notes and tips from papers

## [VERY DEEP CONVOLUTIONAL NETWORKS FOR LARGE-SCALE IMAGE RECOGNITION](https://arxiv.org/pdf/1409.1556.pdf), **VGG, 2014**
  * **Articles**:
    * [Introduction to VGG16 | What is VGG16?](https://www.mygreatlearning.com/blog/introduction-to-vgg16/)
  * **Notes:**
    * Building a deeper network proved to increase the performance to a certain point.
    * The main principle is that a stack of three 3×3 conv. layers are similar to a single 7×7 layer. And maybe even better! Because they use three non-linear activations in between (instead of one), which makes the function more discriminative.
  * **Training notes:**
    *  The batch size was set to 256, momentum to 0.9. The training was regularised by weight decay (the L2 penalty multiplier set to $5*10^{−4}$) and dropout regularisation for the first two fully-connected layers (dropout ratio set to 0.5). The learning rate was initially set to $10^{−2}$, and then decreased by a factor of 10 when the validation set accuracy stopped improving. In total, the learning rate was decreased 3 times, and the learning was stopped after 370K iterations (74 epochs)


## [Going deeper with convolutions](https://arxiv.org/pdf/1409.4842v1.pdf), **InceptionV1, 2014**
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

## [Rethinking the Inception Architecture for Computer Vision](https://arxiv.org/pdf/1512.00567v3.pdf), **InceptionV2 and InceptionV3, 2015**
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

## [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385), **ResNet, 2015**
  * **Articles:**
    * [Understanding and visualizing ResNets](https://towardsdatascience.com/understanding-and-visualizing-resnets-442284831be8)
    * [Implementation Pytorch](https://towardsdev.com/implement-resnet-with-pytorch-a9fb40a77448)
  * **Notes:**
    * Neural networks are approximation functions. Approximation the residual function $H(x) -x$ by multiple nonlinear layers is easier than the identity mapping $f(x) = x$. The identity mapping is closer to the optimal solution than the the zero mapping.
    * With ResNets, **the gradients can flow directly through the skip connections backwards from later layers to initial filters.**
    * The ResNet architecture follows two basic design rules. First, the number of filters in each layer is the same depending on the size of the output feature map. Second, if the feature map’s size is halved, it has double the number of filters to maintain the time complexity of each layer. 

  * **Training Notes**:
    * We adopt batch normalization (BN) right after each convolution and before activation. We use SGD with a mini-batch size of 256. The learning rate starts from 0.1 and is divided by 10 when the error plateaus, and the models are trained for up to 60 × 104 iterations. We use a weight decay of 0.0001 and a momentum of 0.9. We do not use dropout.

## [Inception-v4, Inception-ResNet and the Impact of Residual Connections on Learning](https://arxiv.org/pdf/1602.07261.pdf), **InceptionV4, Inception-ResNet v1 and v2, 2016**
  * **Articles:**
    * [A Simple Guide to the Versions of the Inception Network](https://towardsdatascience.com/a-simple-guide-to-the-versions-of-the-inception-network-7fc52b863202)
  * **Notes:**
    * Make the modules more uniform. The authors also noticed that some of the modules were more complicated than necessary. This can enable us to boost performance by adding more of these uniform modules.
    * For residual addition to work, the input and output after convolution must have the same dimensions. Hence, we use 1x1 convolutions after the original convolutions, to match the depth sizes (Depth is increased after convolution).
    * Networks with residual units deeper in the architecture caused the network to “die” if the number of filters exceeded 1000. Hence, to increase stability, the authors scaled the residual activations by a value around 0.1 to 0.3.

  * **Training Notes:**
    * We have trained our networks with stochastic gradient utilizing the TensorFlow distributed machine learning system using 20 replicas running each on a NVidia Kepler GPU. Our earlier experiments used momentum with a decay of 0.9, while our best models were achieved using RMSProp with decay of 0.9 and $\epsilon = 1.0$. We used a learning rate of 0.045, decayed every two epochs using an exponential rate of 0.94. Model evaluations are performed using a running average of the parameters computed over time.

## [Densely Connected Convolutional Networks](https://arxiv.org/pdf/1608.06993.pdf), **DenseNet, 2017**
  * **Notes**:
    * It skip connections work well, why not connect every everything?
    Dense blocks are made of convolution layer that share skip connections between all of them.
    * 1x1 convolution layers are used to decrease the number of filter and avoid their explosion.
    * The growth rate. It specifies the output features of each extra dense conv layer. Given $k_0$​ initial feature maps and kk growth rate, one can calculate the number of input feature maps of each layer ll as $k_0+k∗(l−1)$. In frameworks, the number $k$ is in multiples of 4, called bottleneck size (bn_size).

## [Big Transfer (BiT): General Visual Representation Learning](https://arxiv.org/pdf/1912.11370.pdf), **BiT, 2019**
  * Articles:
      * [Big Transfer (BiT): General Visual Representation Learning](https://sh-tsang.medium.com/review-big-transfer-bit-general-visual-representation-learning-cb4bf8ed9732)
  * **Notes:**:
      * This paper is about the engineering of transfer learning and not a new architecture. They use ResNet152x4.
      * BN is replaced with Group Normalization (GN) [52] and Weight Standardization (WS): BN is detrimental to Big Transfer for two reasons. First, when training large models with small per-device batches, BN performs poorly or incurs inter-device synchronization cost. Second, due to the requirement to update running statistics, BN is detrimental for transfer.
      GN, when combined with WS, has been shown to improve performance on small-batch training for ImageNet and COCO.
      * Scaling the model and training time with the dataset size is important.
      * Pre-trained model performs well even with low count of examples per class.

## [EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks](https://arxiv.org/pdf/1905.11946.pdf), **EfficientNet, 2019**
  * **Notes:**
    * Individual scaling:
      * With more layers (depth) one can capture richer and more complex features, but such models are hard to train (due to the vanishing gradients)
      * Wider networks are much easier to train. They tend to be able to capture more fine-grained features but saturate quickly.
      * By training with higher resolution images, convnets are in theory able to capture more fine-grained details. Again, the accuracy gain diminishes for quite high resolutions.
    * To constrain the design space, for a fixed baseline model, the authors restrict all the layers to uniform scaling with a constant ratio. This way, we have a more tractable optimization problem. And finally, one has to respect the maximum number of memory and FLOPs of our infrastructure.
    * **Compound scaling method:**  
      We use a compound coefficient $φ$ to uniformly scales network width, depth, and resolution in a principled way:
        
        $$ depth: d = αφ \\ width: w = βφ \\ resolution: r = γφ $$ 
        such that $ α · β^2 · γ^2 ≈ 2$ given that $α ≥ 1, β ≥ 1, γ ≥ 1$ where α, β, γ are constants that can be determined by a small grid search.
    * The baseline architecture was found using neural architecture search so that it optimizes both accuracy and FLOPS, called EfficientNet-B0.

## [Meta Pseudo Labels](https://arxiv.org/pdf/2003.10580.pdf), **2020**
  * **Notes:**
    * **Motivation:** If the pseudo labels are inaccurate, the student will NOT surpass the teacher. This is called confirmation bias in pseudo-labeling methods.

    * **High-level idea:** Design a feedback mechanism to correct the teacher’s bias.

    * The teacher and student are jointly trained. The teacher learns from the reward signal how well the student performs on a batch of images coming from the labeled dataset.

## [An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://arxiv.org/abs/2010.11929), **ViT, 2020**
  * Articles:
    * [How the Vision Transformer (ViT) works in 10 minutes: an image is worth 16x16 words](https://theaisummer.com/vision-transformer/)
  * **Notes:**
      * Transformers lack **the inductive biases** of Convolutional Neural Networks (CNNs), such as translation invariance and a locally restricted receptive field. The transformer is by design **permutation invariant**.
      * The total architecture is called Vision Transformer (ViT in short):
          1. Split an image into patches
          2. Flatten the patches
          3. Produce lower-dimensional linear embeddings from the flattened patches
          4. Add positional embeddings
          5. Feed the sequence as an input to a standard transformer encoder
          6. Pretrain the model with image labels (fully supervised on a huge dataset)
          7. Finetune on the downstream dataset for image classification
      * If ViT is trained on datasets with more than 14M images, at least, it can approach or beat state-of-the-art CNNs. If not, you better stick with ResNets or EfficientNets.

## [Data-Efficient Image Transformers](https://arxiv.org/abs/2012.12877), **DeiT, 2021**
  * Articles:
    * [DeiT - Data-efficient image transformers & distillation through attention (paper illustrated)](https://www.youtube.com/watch?v=HobIo2oT0xY&embeds_euri=https%3A%2F%2Ftheaisummer.com%2F&source_ve_path=MjM4NTE&feature=emb_title)