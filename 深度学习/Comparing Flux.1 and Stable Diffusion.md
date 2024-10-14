# SDXL架构
## 基础架构图
Stable Diffusion XL是一个**二阶段的级联扩散模型（Latent Diffusion Model）**，在Base模型之后，级联了Refiner模型，**对Base模型生成的图像Latent特征进行精细化提升，其本质上是在做图生图的工作**。

**SDXL Base模型由U-Net、VAE以及CLIP Text Encoder（两个）三个模块组成**，在FP16精度下Base模型大小6.94G（FP32：13.88G），其中U-Net占5.14G、VAE模型占167M以及两个CLIP Text Encoder一大一小（OpenCLIP ViT-bigG和OpenAI CLIP ViT-L）分别是1.39G和246M。

**SDXL Refiner模型同样由U-Net、VAE和CLIP Text Encoder（一个）三个模块组成**，在FP16精度下Refiner模型大小6.08G，其中U-Net占4.52G、VAE模型占167M（与Base模型共用）以及CLIP Text Encoder模型（OpenCLIP ViT-bigG）大小1.39G（与Base模型共用）。
![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20241012164032.png)
## VAE（**KL-f8**）
SDXL VAE模型中有三个基础组件：
1. GSC组件：GroupNorm+SiLU+[Conv](https://zhida.zhihu.com/search?content_id=231112916&content_type=Article&match_order=1&q=Conv&zhida_source=entity)
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

![](https://pic3.zhimg.com/v2-03cf776c6281ff727e157e6088dbb394_r.jpg)

