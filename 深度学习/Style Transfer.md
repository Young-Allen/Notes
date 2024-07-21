### 1. DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation
- **实验目的**：DreamBooth 旨在通过少量（通常3-5张）特定主题的图像，微调预训练的文本到图像模型（Diffusion Model），使其能够在不同的上下文中生成该主题的新图像，同时保持对主题关键视觉特征的高保真度。


### 2. Style Aligned Image Generation via Shared Attention
- **实验目的**：使用一张参考图像，生成一系列风格与参考图像一致的图像



### 3. （AdaIN）Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization（2017）
- **实验目的**：实现实时的、任意风格的风格迁移（style transfer），主要方法就是自适应实例标准化（Adaptive Instance Normalization，AdaIN），将内容图像（content image）特征的均值和方差对齐到风格图像（style image）的均值和方差。
- **Background**：
  1. **Batch Normalization**：
     Batch Normalization是2015年一篇论文中提出的数据归一化方法，往往用在深度神经网络中激活层之前。其作用可以加快模型训练时的收敛速度，使得模型训练过程更加稳定，避免梯度爆炸或者梯度消失。并且起到一定的正则化作用，几乎代替了Dropout。（简单来说就是对一个批次中每一张图像的对应像素值进行Normalization）
     BatchNorm是对一个batch-size样本内的每个特征[分别]做归一化，LayerNorm是[分别]对每个样本的所有特征做归一化。
     [【基础算法】六问透彻理解BN(Batch Normalization）](https://zhuanlan.zhihu.com/p/93643523)
  2. **Instance Normalization**：
     [Ulyanov等](https://arxiv.org/abs/1701.02096)发现，将BN替换为Instance Normalization（IN），可以提升风格迁移的性能。IN的操作跟BN类似，就是范围从一个batch变成了一个instance。只是IN是作用于单张图片，但是BN作用于一个Batch。
    ![](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721133613.png)

  3. **Conditional Instance Normalization**：
- **Adaptive Instance Normalization**：
  在BN，IN，CIN中，网络会学习仿射变换参数𝛾 和 𝛽，作者提出的AdaIN则无需学习这两个参数，直接用style image的特征的均值和标准差代替这两个参数，公式如下：
  
  $\mathrm{AdaIN}(x,y)=\sigma(y)\left(\frac{x-\mu(x)}{\sigma(x)}\right)+\mu(y)$
  
  其中， **𝜇(𝑥) 和 𝜎(𝑥)** 分别表示content image的特征的均值和标准差，**𝜇(𝑦) 和 𝜎(𝑦)** 分别表示style image的特征的均值和标准差。这个公式可以理解为，先去风格化（减去自身均值再除以自身标准差），再风格化到style image的风格（乘style image的标准差再加均值 ）。
  网络结构图：
  ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721130256.png)训练时，先用VGG提取content image和style image的特征，然后在AdaIN模块进行式（8）的操作，然后用于VGG对称的Decoder网络将特征还原为图像，然后将还原的图像再输入到VGG提取特征，计算content loss和style loss，style loss会对多个层的特征进行计算。VGG的参数在训练过程中是不更新的，训练的目的是为了得到一个好的Decoder。
