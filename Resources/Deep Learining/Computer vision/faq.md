# Convolutional Neural Networks FAQ

* How to choose the number of filters in each layer?

* How to compute the number of parameters in a conv layer?
  * The number of parameters is equal to $(k*k*ch_{in}) * ch_{out} + ch_{out}$, where $ch$ is the number of channels.
* How to compute the output size of conv layer? How to choose padding and stride size ?
  * The input spatial size (HxW) is usually preserved between layer. Using this formula, $out=(in+2∗p−k)/s +1$, where $out$ and $in$ are the spatial dims of output and input, $k$ is the kernel size, $p$ is the padding size and $s$ is the stride size, we can choose $p$ and $s$ based on the desired kernel size. $s$ is usually set to 1 or 2, higher values are rare in practice.  
  When $s=1$ and we want to preserve the size, we can use this simpler formula: $p=(k-1)/2$
* How to compute the output size of Pool layer?
  * $out=(in−k)/s +1$, where $out$ and $in$ are the spatial dims of output and input, $k$ is the kernel size, $s$ is the stride size. The depth remains unchanged. Common hyper-parameters are: $k=3, s=2$ and $k=2, s=2$. 
* How to perform flatten?
* What’s group normalization ([In-layer normalization techniques for training very deep neural networks | AI Summer](https://theaisummer.com/normalization/#group-normalization-2018)) ? what’s weight standardization ([In-layer normalization techniques for training very deep neural networks | AI Summer](https://theaisummer.com/normalization/#weight-standardization-2019))?  Difference from BN ?
* When to use stride and maxpool to reduce spatial dimensions?
* Should I use `inplace=True` in pytroch?
  * **`inplace=True` means that it will modify the input directly, without allocating any additional output.** It can sometimes slightly decrease the memory usage, but may not always be a valid operation (because the original input is destroyed). However, if you don’t see an error, it means that your use case is valid.
  * From pytorch [website](https://pytorch.org/docs/master/notes/autograd.html#in-place-operations-on-variables): **Performing inplace operations on the input of any of the functions is forbidden** as they may lead to unexpected side-effects. PyTorch will throw an error if the input to a pack hook is modified inplace but does not catch the case where the input to an unpack hook is modified inplace.

* How to choose the kernel size of the convolutional layer (e.g. 3x3, 5x5, 7x7) and what their effect?

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