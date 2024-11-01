# Comparing FLUX.1 and Stable Diffusion

# SDXL架构

## 基础架构图

Stable Diffusion XL是一个**二阶段的级联扩散模型（Latent Diffusion Model）**，在Base模型之后，级联了Refiner模型，**对Base模型生成的图像Latent特征进行精细化提升，其本质上是在做图生图的工作**。

**SDXL Base模型由U-Net、VAE以及CLIP Text Encoder（两个）三个模块组成**，在FP16精度下Base模型大小6.94G（FP32：13.88G），其中U-Net占5.14G、VAE模型占167M以及两个CLIP Text Encoder一大一小（OpenCLIP ViT-bigG和OpenAI CLIP ViT-L）分别是1.39G和246M。

**SDXL Refiner模型同样由U-Net、VAE和CLIP Text Encoder（一个）三个模块组成**

，在FP16精度下Refiner模型大小6.08G，其中U-Net占4.52G、VAE模型占167M（与Base模型共用）以及CLIP Text Encoder模型（OpenCLIP ViT-bigG）大小1.39G（与Base模型共用）。

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241012164032.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241012164032.png)

## VAE（**KL-f8**）

SDXL VAE模型中有三个基础组件：

1. GSC组件：GroupNorm+SiLU+Conv
2. Downsample组件：Padding+Conv
3. Upsample组件：Interpolate+Conv

同时SDXL VAE模型还有两个核心组件：ResNet Block模块和SelfAttention模型，两个模块的结构如下图所示。

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241012193222.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241012193222.png)

## CLIP Text Encoder

**Stable Diffusion XL与之前的系列相比使用了两个CLIP Text Encoder，分别是OpenCLIP ViT-bigG（694M）和OpenAI CLIP ViT-L/14（123.65M），从而大大增强了Stable Diffusion XL对文本的提取和理解能力，同时提高了输入文本和生成图片的一致性.**

其中OpenCLIP ViT-bigG是一个只由Transformer模块组成的模型，一共有32个CLIPEncoder模块，下图是Stable Diffusion XL OpenCLIP ViT-bigG的结构图：

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241014145013.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241014145013.png)

OpenAI CLIP ViT-L/14同样是一个只由Transformer模块组成的模型，一共有12个CLIPEncoder模块，下图是**SDXL OpenAI CLIP ViT-L/14的完整结构图**：

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241014145127.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241014145127.png)

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

![https://raw.githubusercontent.com/Young-Allen/pic/main/sdxl.png](https://raw.githubusercontent.com/Young-Allen/pic/main/sdxl.png)

## Refiner模型

由于已经有U-Net（Base）模型生成了图像的Latent特征，所以**Refiner模型的主要工作是在Latent特征进行小噪声去除和细节质量提升**。

SDXL Refiner模型和SDXL Base模型在结构上的异同：

1. SDXL Base的Encoder和Decoder结构都采用4个stage，而SDXL Base设计的是3个stage。
2. SDXL Refiner和SDXL Base一样，在第一个stage中没有使用Attention模块。
3. 在经过第一个卷积后，SDXL Refiner设置初始网络特征维度为384，而SDXL Base 采用的是320。
4. SDXL Refiner的Attention模块中SDXL_Spatial Transformer结构数量均设置为4。
5. SDXL Refiner的参数量为2.3B，比起SDXL Base的2.6B参数量略小一些。

![https://raw.githubusercontent.com/Young-Allen/pic/main/refiner.png](https://raw.githubusercontent.com/Young-Allen/pic/main/refiner.png)

# SD3架构

## 整体架构

从整体架构上来看，和之前的 SD 一样，SD3 主要基于隐扩散模型（latent diffusion model, LDM）。这套方法是一个两阶段的生成方法：先用一个 LDM 生成隐空间低分辨率的图像，再用一个自编码器把图像解码回真实图像。扩散模型 LDM 会使用一个神经网络模型来对噪声图像去噪。为了实现文生图，该去噪网络会以输入文本为额外约束。论文为：[Scaling Rectified Flow Transformers for High-Resolution Image Synthesis](https://arxiv.org/abs/2403.03206).下面是整体的模型架构图：

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241014204535.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241014204535.png)

在方法设计上，主要提出了以下的改进：

- 首次在大型文生图模型上使用了整流模型（Rectified Flow）。
- 用新的Diffusion Transformer ( [DiT](https://arxiv.org/abs/2212.09748) )替换U-Net神经网络更好地融合文本信息。
- 使用了各种小设计来提升模型的能力。如使用二维位置编码来实现任意分辨率的图像生成。

## Flow Matching

SD3相比之前的SD一个最大的变化是采用 [Rectified Flow](https://arxiv.org/abs/2210.02747) 来作为生成模型。**Flow Matching（流匹配）** 是一种用来生成图像或数据的新方法，其核心思想是通过学习如何把一个简单的分布（如随机噪声）**逐渐变成**你想要的目标分布（如真实图片）。我们可以把这个过程想象成“引导水流”，让它从一个地方自然地流向另一个地方。 [Flow Matching讲解](https://www.bilibili.com/video/BV1Wv3xeNEds/?spm_id_from=333.337.search-card.all.click&vd_source=ebaa9b5a24bde7756de385ec80faa6a9)

## Multimodal Diffusion Transformer(MM-DiT)

SD3 的去噪模型是一个 Diffusion Transformer (DiT)。如果去噪模型只有带噪图像这一种输入的话，DiT 则会是一个结构非常简单的模型，和标准 ViT 一样：图像过图块化层 (Patching) 并与位置编码相加，得到序列化的数据。这些数据会像标准 Transformer 一样，经过若干个子模块，再过反图块层得到模型输出。DiT 的每个子模块 DiT-Block 和标准 Transformer 块一样，由 LayerNorm, Self-Attention, 一对一线性层 (Pointwise Feedforward, FF) 等模块构成。

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241015110635.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241015110635.png)

然而，扩散模型中的去噪网络一定得支持带约束生成。这是因为扩散模型约束于去噪时刻 t。此外，作为文生图模型，SD3 还得支持文本约束。DiT 及本文的 MM-DiT 把模型设计的重点都放在了处理额外约束上。

我们先看一下模块是怎么处理较简单的时刻约束的。此处，如下图所示，SD3 的模块保留了 DiT 的设计，用自适应 LayerNorm (Adaptive LayerNorm, AdaLN) 来引入额外约束。具体来说，过了 LayerNorm 后，数据的均值、方差会根据时刻约束做调整。另外，过完 Attention 层或 FF 层后，数据也会乘上一个和约束相关的系数。

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241015110701.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241015110701.png)

我们再来看文本约束的处理。文本约束以两种方式输入进模型：与时刻编码拼接、在注意力层中融合。具体数据关联细节可参见下图。如图所示，为了提高 SD3 的文本理解能力，描述文本 (“Caption”) 经由三种编码器编码，得到两组数据。一组较短的数据会经由 MLP 与文本编码加到一起；另一组数据会经过线性层，输入进 Transformer 的主模块中。

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241015110842.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241015110842.png)

SD3 的 DiT 的子模块结构图如下所示。我们可以分几部分来看它。先看时刻编码 y 的那些分支。和标准 DiT 子模块一样，y 通过修改 LayerNorm 后数据的均值、方差及部分层后的数据大小来实现约束。再看输入的图像编码 x 和文本编码 c。二者以相同的方式做了 DiT 里的 LayerNorm, FF 等操作。不过，相比此前多数基于 DiT 的模型，此模块用了一种特殊的融合注意力层。具体来说，在过注意力层之前，x 和 c 对应的 Q,K,V 会分别拼接到一起，而不是像之前的模型一样，Q 来自图像，K,V 来自文本。过完注意力层，输出的数据会再次拆开，回到原本的独立分支里。由于 Transformer 同时处理了文本、图像的多模态信息，所以作者将模型取名为 MM-DiT (Multimodal DiT)。

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241015104700.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241015104700.png)

# FLUX.1架构

## 基础架构图

在Black Forest Labs发布的FLUX.1的介绍中，提到：“所有公开的FLUX.1模型基于[multimodal](https://arxiv.org/abs/2403.03206) 和 [parallel](https://arxiv.org/abs/2302.05442) [diffusion transformer](https://arxiv.org/abs/2212.09748) blocks 的混合架构，并扩展至120亿参数。我们在现有的最先进扩散模型基础上进行了改进，基于 [flow matching](https://arxiv.org/abs/2210.02747) 方法，这是一种通用且概念简单的生成模型训练方法，其中扩散模型是其特例之一。此外，我们通过集成[rotary positional embeddings](https://arxiv.org/abs/2104.09864) 和 [parallel attention layers](https://arxiv.org/abs/2302.05442) 提高了模型性能并改进了硬件效率。”

下面是FLUX.1做的一些改动

- 略微变动的图块化策略
- 不使用 Classifier-Free Guidance 的指引蒸馏
- 为不同分辨率图像调整流匹配噪声调度
- 用二维旋转式位置编码 (RoPE) 代替二维正弦位置编码
- 在原 Stable Diffusion 3 双流 Transformer 块后添加并行单流 Transformer 块

FLUX.1可以看作是SD3的续作，在一定程度上使用的SD3的Flow Matching的思想，并对SD3的MM-DiT做了进一步的改进。因为关于FLUX.1的相关技术报告还未发布，所以FLUX.1的内部详细信息是总结网上的内容得出，下面是来自于网上的FLUX.1架构图：

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241015105320.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241015105320.png)

![https://preview.redd.it/fluxs-architecture-diagram-dont-think-theres-a-paper-so-had-v0-7n3ix8do9vgd1.png?width=1080&crop=smart&auto=webp&s=e183bc880d354a40d3a18156c10d7291a4942569](https://preview.redd.it/fluxs-architecture-diagram-dont-think-theres-a-paper-so-had-v0-7n3ix8do9vgd1.png?width=1080&crop=smart&auto=webp&s=e183bc880d354a40d3a18156c10d7291a4942569)

## VAE模型

SD3相比之前版本的SD一个重要改进就是采用了[特征通道](https://zhida.zhihu.com/search?content_id=248245529&content_type=Article&match_order=1&q=%E7%89%B9%E5%BE%81%E9%80%9A%E9%81%93&zhida_source=entity)更多的VAE，特征维度从原来的4增加到16，可以减少VAE重建所导致的畸变。Flux和SD3一样也采用16通道的VAE，但Flux的VAE并不是直接用SD3的VAE，而是重新训练了，因为模型结构虽然一样，但是参数变了。

## 文本编码器

SD3采用三个文本编码器，分别是Open[CLIP](https://zhida.zhihu.com/search?content_id=248245529&content_type=Article&match_order=1&q=CLIP&zhida_source=entity)-ViT/G，CLIP-ViT/L以及T5-xxl，其中两个CLIP的pooling特征拼接在一起加在time embedding上，而且两个CLIP的[text embedding](https://zhida.zhihu.com/search?content_id=248245529&content_type=Article&match_order=1&q=text+embedding&zhida_source=entity)拼接在一起，并和T5-xxl的text embedding沿着[token维度](https://zhida.zhihu.com/search?content_id=248245529&content_type=Article&match_order=1&q=token%E7%BB%B4%E5%BA%A6&zhida_source=entity)拼接在一起送入MMDiT。但是Flux只使用CLIP-ViT/L和T5-xxl两个文本编码器，CLIP-ViT/L的pooling特征加在time embedding上，而T5-xxl的text embedding直接送入MMDiT输入中。所以Flux更依赖T5-xxl，而SD3其实CLIP特征还有较大的作用，比如SD3可以去掉T5只用CLIP来生成图像。

## MM-DiT 和 Single DiT

**MMDiT**，Flux相比SD3有一些变动，原来的MMDiT是文本和图像是独立两个分支，只用在 attention 的时候共享计算，而Flux只在前面的19层采用这样的MMDiT block，后面的38层直接采用普通的 [DiT block](https://arxiv.org/abs/2212.09748) ，文本和图像拼接在一起共用一套参数。直观上看，前面的MMDiT block可以实现两个模态融合了，后面就不需要这样做了，这样就可以把节省的参数用来增加模型的深度。最终Flux的模型大小是12B，比8B的SD3还大40%。

![https://raw.githubusercontent.com/Young-Allen/pic/main/20241015113931.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241015113931.png)

## FluxTransformer2DModel

```python
num_layers (`int`, *optional*, defaults to 18): The number of layers of MMDiT blocks to use.
        
num_single_layers (`int`, *optional*, defaults to 18): The number of layers of single DiT blocks to use.
```

（1）19层`transformer_blocks`

```python
  (transformer_blocks): ModuleList(
    (0-18): 19 x FluxTransformerBlock(
      (norm1): AdaLayerNormZero(
        (silu): SiLU()
        (linear): Linear(in_features=3072, out_features=18432, bias=True)
        (norm): LayerNorm((3072,), eps=1e-06, elementwise_affine=False)
      )
      (norm1_context): AdaLayerNormZero(
        (silu): SiLU()
        (linear): Linear(in_features=3072, out_features=18432, bias=True)
        (norm): LayerNorm((3072,), eps=1e-06, elementwise_affine=False)
      )
      (attn): Attention(
        (norm_q): RMSNorm()
        (norm_k): RMSNorm()
        (to_q): Linear(in_features=3072, out_features=3072, bias=True)
        (to_k): Linear(in_features=3072, out_features=3072, bias=True)
        (to_v): Linear(in_features=3072, out_features=3072, bias=True)
        (add_k_proj): Linear(in_features=3072, out_features=3072, bias=True)
        (add_v_proj): Linear(in_features=3072, out_features=3072, bias=True)
        (add_q_proj): Linear(in_features=3072, out_features=3072, bias=True)
        (to_out): ModuleList(
          (0): Linear(in_features=3072, out_features=3072, bias=True)
          (1): Dropout(p=0.0, inplace=False)
        )
        (to_add_out): Linear(in_features=3072, out_features=3072, bias=True)
        (norm_added_q): RMSNorm()
        (norm_added_k): RMSNorm()
      )
      (norm2): LayerNorm((3072,), eps=1e-06, elementwise_affine=False)
      (ff): FeedForward(
        (net): ModuleList(
          (0): GELU(
            (proj): Linear(in_features=3072, out_features=12288, bias=True)
          )
          (1): Dropout(p=0.0, inplace=False)
          (2): Linear(in_features=12288, out_features=3072, bias=True)
        )
      )
      (norm2_context): LayerNorm((3072,), eps=1e-06, elementwise_affine=False)
      (ff_context): FeedForward(
        (net): ModuleList(
          (0): GELU(
            (proj): Linear(in_features=3072, out_features=12288, bias=True)
          )
          (1): Dropout(p=0.0, inplace=False)
          (2): Linear(in_features=12288, out_features=3072, bias=True)
        )
      )
    )
  )

```

（2）38层`single_transformer_blocks`

```python
(single_transformer_blocks): ModuleList(
    (0-37): 38 x FluxSingleTransformerBlock(
      (norm): AdaLayerNormZeroSingle(
        (silu): SiLU()
        (linear): Linear(in_features=3072, out_features=9216, bias=True)
        (norm): LayerNorm((3072,), eps=1e-06, elementwise_affine=False)
      )
      (proj_mlp): Linear(in_features=3072, out_features=12288, bias=True)
      (act_mlp): GELU(approximate='tanh')
      (proj_out): Linear(in_features=15360, out_features=3072, bias=True)
      (attn): Attention(
        (norm_q): RMSNorm()
        (norm_k): RMSNorm()
        (to_q): Linear(in_features=3072, out_features=3072, bias=True)
        (to_k): Linear(in_features=3072, out_features=3072, bias=True)
        (to_v): Linear(in_features=3072, out_features=3072, bias=True)
      )
    )
  )
```

# 模型架构的对比

|**Feature**|**SDXL**|**SD3**|**FLUX.1**|
|---|---|---|---|
|Architecture|Two-stage cascaded Latent Diffusion Model (U-Net)|MM-DiT (Multimodal DiT)|Hybrid of multimodal and parallel diffusion transformer blocks（改进的MM-DiT和single DiT）|
|Text Encoders|Two CLIP Text Encoders|Three (OpenCLIP-ViT/G, CLIP-ViT/L, T5-xxl)|Two (CLIP-ViT/L and T5-xxl)|
|VAE|KL-f8|16-channel VAE|16-channel VAE (retrained)|
|Position Encoding|2D sinusoidal position encoding|2D sinusoidal position encoding|2D rotary position encoding (RoPE)|
|Training Method|Latent diffusion|Flow matching|Flow matching|
|Unique Features|Refiner model for image enhancement|Multimodal fusion in attention layers|Combination of MM-DiT and single DiT blocks|

# 参考内容

[黑森林实验室官宣FLUX.1](https://blackforestlabs.ai/announcing-black-forest-labs/) [Stable Diffusion 3 论文及源码概览](https://zhouyifan.net/2024/07/14/20240703-SD3/)

[Stable Diffusion 3「精神续作」FLUX.1 源码深度前瞻解读](https://zhouyifan.net/2024/09/03/20240809-flux1/)

[FLUX.1 原理与源码解析](https://zhuanlan.zhihu.com/p/741939590)

[DiT类模型纵评-从DiT到SD3到Flux](https://zhuanlan.zhihu.com/p/714252753)

[Diffusion Transformer (DiT) Models: A Beginner’s Guide](https://encord.com/blog/diffusion-models-with-transformers/)

[Flux's Architecture diagram on Reddit](https://www.reddit.com/r/LocalLLaMA/comments/1ekr7ji/fluxs_architecture_diagram_dont_think_theres_a/#lightbox)

hugging face 的diffuser中flux的源码：[https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/transformers/transformer_flux.py#L234](https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/transformers/transformer_flux.py#L234)