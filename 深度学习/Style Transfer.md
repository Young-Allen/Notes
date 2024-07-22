### 1. DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation
- **实验目的**：DreamBooth 旨在通过少量（通常3-5张）特定主题的图像，微调预训练的文本到图像模型（Diffusion Model），使其能够在不同的上下文中生成该主题的新图像，同时保持对主题关键视觉特征的高保真度。


### 2. Style Aligned Image Generation via Shared Attention
- **Intro**：使用一张参考图像，生成一系列风格与参考图像一致的图像，解决了在大规模文本到图像模型中实现风格对齐图像生成的难题。通过在扩散过程中引入带有AdaIN的注意力共享操作，使得在生成图像中成功建立风格一致和视觉连贯的图像。

- **Method overview**：
  我们方法的关键见解是利用自注意力机制来允许各种生成的图像之间进行通信。在生成的图像中共享注意力层。
  1. 但是Full Attention Sharing会导致图像之间的内容泄露，如下图中，独角兽获取了恐龙的颜色信息。![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721143035.png)
  2. 同时，Full Attention Sharing还会导致同一组生成的图像集缺少多样性。

  为了限制内容泄露并让生成的图像的内容更加多样化，我们只对生成集合中的一张图像进行注意力共享。（一般是一个batch中的第一张图像）但是只关注集合中的一张图像会产生风格相似的不同集合，如下图恐龙和独角兽的朝向就不太统一。![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721150426.png)
  引入AdaIN对Reference Features和Target Features的K和Q进行AdaIN归一化操作，然后进行Scaled Dot-Product Attention。![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721144104.png)
  公式如下：
  $\hat{Q}_t=\mathrm{AdaIN}(Q_t,Q_r)\quad\hat{K}_t=\mathrm{AdaIN}(K_t,K_r),$
  $\mathrm{AdaIN}(x,y)=\sigma(y)\left(\frac{x-\mu(x)}{\sigma(x)}\right)+\mu(y)$
  $\mathrm{Attention}(\hat{Q}_{t},K_{rt}^{T},V_{rt}),$
  $\text{where }K_{rt}=\begin{bmatrix}K_r\\\hat{K}_t\end{bmatrix}\text{and}V_{rt}=\begin{bmatrix}V_r\\V_t\end{bmatrix}.$
- **Evaluations and Experiments**
  1. **Evaluation set**：在 ChatGPT 的支持下，我们生成了100个文本提示，描述了四个随机对象的不同图像风格。例如，“{A guitar, A hot air balloon, A sailboat, A mountain} in papercut art style.”对于每种风格和对象集，我们使用我们的方法生成一组图像。
  2. **Metrics**：为了验证每个生成的图像包含其指定的对象，我们测量图像与对象文本描述之间的 CLIP 余弦相似度 。此外，我们通过测量每个生成集内生成图像的 DINO VIT-B/8 [9] 嵌入的成对平均余弦相似度，来评估每个生成集的风格一致性。![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721152112.png)
  3. **Comparisons**：作为基准，我们将我们的方法与T2I个性化方法进行比较。我们用评估数据集中每组的第一张图像训练StyleDrop [55]和DreamBooth [47]，并使用训练后的个性化权重生成每组中的另外三张图像。我们使用了非回归T2I模型的公共非官方实现版StyleDrop1（SDRPunofficial）。由于非官方MUSE模型与官方模型之间存在较大的质量差距，我们遵循StyleDrop并在SDXL（SDRP–SDXL）上实现了一个适配器模型，在模型的注意力块的每个前馈层后训练一个低秩线性层。为了训练DreamBooth，我们在SDXL上采用了LoRA [25, 49]变体（DB–LoRA），使用公共的huggingface–diffusers实现。我们遵循[55]中报告的超参数调整，并对SDRP–SDXL和DB–LoRA进行了400步的训练，以防止过拟合到风格训练图像。![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721161517.png)
  

- **代码：**
  1. StyleAligned over SDXL：
     share_group_norm=True,   share_layer_norm=True,  share_attention=True,![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721184539.png)share_group_norm=False,   share_layer_norm=True,  share_attention=True,![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721184832.png)share_group_norm=False,   share_layer_norm=False,  share_attention=True,![](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721185252.png)
     share_group_norm=False,   share_layer_norm=False,  share_attention=False,![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721185740.png)
      

  



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
  ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721130256.png)训练时，先用VGG提取content image和style image的特征，然后在使用AdaIN进行操作，然后用于VGG对称的Decoder网络将特征还原为图像，然后将还原的图像再输入到VGG提取特征，计算content loss和style loss，style loss会对多个层的特征进行计算。VGG的参数在训练过程中是不更新的，训练的目的是为了得到一个好的Decoder。
