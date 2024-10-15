# SDXL架构
## 基础架构图
Stable Diffusion XL是一个**二阶段的级联扩散模型（Latent Diffusion Model）**，在Base模型之后，级联了Refiner模型，**对Base模型生成的图像Latent特征进行精细化提升，其本质上是在做图生图的工作**。

**SDXL Base模型由U-Net、VAE以及CLIP Text Encoder（两个）三个模块组成**，在FP16精度下Base模型大小6.94G（FP32：13.88G），其中U-Net占5.14G、VAE模型占167M以及两个CLIP Text Encoder一大一小（OpenCLIP ViT-bigG和OpenAI CLIP ViT-L）分别是1.39G和246M。

**SDXL Refiner模型同样由U-Net、VAE和CLIP Text Encoder（一个）三个模块组成**，在FP16精度下Refiner模型大小6.08G，其中U-Net占4.52G、VAE模型占167M（与Base模型共用）以及CLIP Text Encoder模型（OpenCLIP ViT-bigG）大小1.39G（与Base模型共用）。
![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241012164032.png)
## VAE（**KL-f8**）
SDXL VAE模型中有三个基础组件：
1. GSC组件：GroupNorm+SiLU+Conv
2. Downsample组件：Padding+Conv
3. Upsample组件：Interpolate+Conv

同时SDXL VAE模型还有两个核心组件：ResNet Block模块和SelfAttention模型，两个模块的结构如下图所示。
![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241012193222.png)

## CLIP Text Encoder
**Stable Diffusion XL与之前的系列相比使用了两个CLIP Text Encoder，分别是OpenCLIP ViT-bigG（694M）和OpenAI CLIP ViT-L/14（123.65M），从而大大增强了Stable Diffusion XL对文本的提取和理解能力，同时提高了输入文本和生成图片的一致性.**

其中OpenCLIP ViT-bigG是一个只由Transformer模块组成的模型，一共有32个CLIPEncoder模块，下图是Stable Diffusion XL OpenCLIP ViT-bigG的结构图：

![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241014145013.png)

OpenAI CLIP ViT-L/14同样是一个只由Transformer模块组成的模型，一共有12个CLIPEncoder模块，下图是**SDXL OpenAI CLIP ViT-L/14的完整结构图**：

![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241014145127.png)

## U-Net
**Stable Diffusion XL Base U-Net包含十四个基本模块：**
1. **GSC模块**：Stable Diffusion Base XL U-Net中的最小组件之一，由GroupNorm+SiLU+Conv三者组成。
2. **DownSample模块**：Stable Diffusion Base XL U-Net中的下采样组件，使用了Conv（kernel_size=(3, 3), stride=(2, 2), padding=(1, 1)）进行采下采样。
3. **UpSample模块**：Stable Diffusion Base XL U-Net中的上采样组件，由插值算法（nearest）+Conv组成。
4. **ResNetBlock模块**：借鉴ResNet模型的“残差结构”，让网络能够构建的更深的同时，将Time Embedding信息嵌入模型。
5. **CrossAttention模块**：将文本的语义信息与图像的语义信息进行Attention机制，增强输入文本Prompt对生成图像的控制。
6. **SelfAttention模块**：SelfAttention模块的整体结构与CrossAttention模块相同，这是输入全部都是图像信息，不再输入文本信息。
7. **FeedForward模块**：Attention机制中的经典模块，由GeGlU+Dropout+Linear组成。
8. **Basic Transformer Block模块**：由LayerNorm+SelfAttention+CrossAttention+FeedForward组成，是多重Attention机制的级联，并且每个Attention机制都是一个“残差结构”。通过加深网络和多Attention机制，大幅增强模型的学习能力与图文的匹配能力。
9. **SDXL_Spatial Transformer_X模块**：由GroupNorm+Linear+X个BasicTransformer Block+Linear构成，同时ResNet模型的“残差结构”依旧没有缺席。
10. **SDXL_DownBlock模块**：由两个ResNetBlock+一个DownSample组成。
11. **SDXL_UpBlock_X模块**：由X个ResNetBlock模块组成。
12. **CrossAttnDownBlock_X_K模块**：是Stable Diffusion XL Base U-Net中Encoder部分的主要模块，由K个（ResNetBlock模块+SDXL_Spatial Transformer_X模块）+一个DownSample模块组成。
13. **CrossAttnUpBlock_X_K模块**：是Stable Diffusion XL Base U-Net中Decoder部分的主要模块，由K个（ResNetBlock模块+SDXL_Spatial Transformer_X模块）+一个UpSample模块组成。
14. **CrossAttnMidBlock模块**：是Stable Diffusion XL Base U-Net中Encoder和ecoder连接的部分，由ResNetBlock+SDXL_Spatial Transformer_10**+ResNetBlock组成。

![sdxl.png](https://raw.githubusercontent.com/Young-Allen/pic/main/sdxl.png)


## Refiner模型
由于已经有U-Net（Base）模型生成了图像的Latent特征，所以**Refiner模型的主要工作是在Latent特征进行小噪声去除和细节质量提升**。

SDXL Refiner模型和SDXL Base模型在结构上的异同：

1. SDXL Base的Encoder和Decoder结构都采用4个stage，而SDXL Base设计的是3个stage。
2. SDXL Refiner和SDXL Base一样，在第一个stage中没有使用Attention模块。
3. 在经过第一个卷积后，SDXL Refiner设置初始网络特征维度为384，而SDXL Base 采用的是320。
4. SDXL Refiner的Attention模块中SDXL_Spatial Transformer结构数量均设置为4。
5. SDXL Refiner的参数量为2.3B，比起SDXL Base的2.6B参数量略小一些。



![refiner.png](https://raw.githubusercontent.com/Young-Allen/pic/main/refiner.png)


# SD3架构

## 整体架构
从整体架构上来看，和之前的 SD 一样，SD3 主要基于隐扩散模型（latent diffusion model, LDM）。这套方法是一个两阶段的生成方法：先用一个 LDM 生成隐空间低分辨率的图像，再用一个自编码器把图像解码回真实图像。扩散模型 LDM 会使用一个神经网络模型来对噪声图像去噪。为了实现文生图，该去噪网络会以输入文本为额外约束。论文为：[Scaling Rectified Flow Transformers for High-Resolution Image Synthesis](https://arxiv.org/abs/2403.03206).下面是整体的模型架构图：

![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241014204535.png)

在方法设计上，主要提出了以下的改进：
- 首次在大型文生图模型上使用了整流模型（Rectified Flow）。
- 用新的Diffusion Transformer（DiT）替换U-Net神经网络更好地融合文本信息。
- 使用了各种小设计来提升模型的能力。如使用二维位置编码来实现任意分辨率的图像生成。


## Flow Matching
SD3相比之前的SD一个最大的变化是采用 [Rectified Flow](https://arxiv.org/abs/2210.02747) 来作为生成模型。**Flow Matching（流匹配）** 是一种用来生成图像或数据的新方法，其核心思想是通过学习如何把一个简单的分布（如随机噪声）**逐渐变成**你想要的目标分布（如真实图片）。我们可以把这个过程想象成“引导水流”，让它从一个地方自然地流向另一个地方。
[Flow Matching讲解](https://www.bilibili.com/video/BV1Wv3xeNEds/?spm_id_from=333.337.search-card.all.click&vd_source=ebaa9b5a24bde7756de385ec80faa6a9)

## Multimodal Diffusion Transformer(MM-DiT)
SD3 的去噪模型是一个 Diffusion Transformer (DiT)。如果去噪模型只有带噪图像这一种输入的话，DiT 则会是一个结构非常简单的模型，和标准 ViT 一样：图像过图块化层 (Patching) 并与位置编码相加，得到序列化的数据。这些数据会像标准 Transformer 一样，经过若干个子模块，再过反图块层得到模型输出。DiT 的每个子模块 DiT-Block 和标准 Transformer 块一样，由 LayerNorm, Self-Attention, 一对一线性层 (Pointwise Feedforward, FF) 等模块构成。
由于文本和图像嵌入在概念上完全不同，因此对这两种模态使用两组独立的权重。如下图所示，这相当于每种模态都有两个独立的转换器，但是将两种模态的序列连接起来进行注意力操作，这样两种表示都可以在自己的空间中工作，同时考虑另一种表示。

![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241015104700.png)



# FLUX.1架构
## 基础架构图
在Black Forest Labs发布的FLUX.1的介绍中，提到：“所有公开的FLUX.1模型基于[multimodal](https://arxiv.org/abs/2403.03206) 和 [parallel](https://arxiv.org/abs/2302.05442) [diffusion transformer](https://arxiv.org/abs/2212.09748) blocks 的混合架构，并扩展至120亿参数。我们在现有的最先进扩散模型基础上进行了改进，基于 [flow matching](https://arxiv.org/abs/2210.02747) 方法，这是一种通用且概念简单的生成模型训练方法，其中扩散模型是其特例之一。此外，我们通过集成[rotary positional embeddings](https://arxiv.org/abs/2104.09864) 和 [parallel attention layers](https://arxiv.org/abs/2302.05442) 提高了模型性能并改进了硬件效率。”
FLUX.1可以看作是SD3的续作，在一定程度上使用的SD3的Flow Matching的思想，并对SD3的MM-DiT做了进一步的改进。因为关于FLUX.1的相关技术报告还未发布，所以FLUX.1的内部详细信息是总计网上的内容得出，下面是来自于网上的FLUX.1的架构图：

![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241015105320.png)

![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241015105401.png)
