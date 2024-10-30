# Abstract

# Introduction
「抽象」是「具象」的相对概念，是就多种事物抽出其共通之点，加以综合而成一个新的概念，此一概念就叫做「抽象」。抽象绘画是以直觉和想象力为创作的出发点，排斥任何具有象征性、文学性、说明性的表现手法，仅将造形和色彩加以综合、组织在画面上，因此抽象绘画呈现出来的纯粹形色。目前Diffusion model在图像生成领域大放异彩，但是以SDXL为代表的基于U-Net的网络架构在训练时并没有对形态和神态抽象的图像进行特殊训练，导致其生成的图像偏向于写实风格，其图像特点多偏向于具有复杂的阴影或者是笔触等。抽象化的图像因其形态和神态的抽象层面，可以使用较少的笔触表达丰富的内容，因此对于图像抽象化生成的研究是具有意义的。

目前，图像抽象的研究主要可分为三个层面：基于像素的抽象、基于线稿的抽象和基于几何图形的抽象。其中基于像素的抽象的研究大多集中在扩散模型诞生之前，使用卷积的类似手段将图像的复杂的特征进行简化。而在CLIP等模型诞生后，又有许多工作基于线条的形式，将图像抽象成为线稿图。基于几何图形的抽象是将图像抽象为一些特定几何图形的组合，比如说三角形、圆形和矩形等。但是，目前基于几何图形的抽象研究相对较少。

# Preliminarles

# Pixel-based Image Abstraction


# Sketch-based Image Abstraction


# Geometric-based Image Abstraction

## Keywords
1. Geometric art 
2. Computational art 
3. Low poly rendering 
4. Style transfer 
5. Non-photorealistic rendering
## Problems that Need Attention
These are two of the main reasons why modern logo design and animation especially favor the use of geometric primitives.

1. **Simplicity and Expressibility**: On one hand, geometric primitive has very compact and explicit parameter format, thus facilitating compact representation and convenient manipulation. On the other hand, numerous combinations of geometric primitives imply rich expressive power compared with sketches. For example, the Bauhaus and Cubism arts tend to adopt ONLY simplistic shapes like rectangles and spheres, yet still with extremely rich artistic expression.
2. **Semantic Preservation**: Human beings tend to think of complex visual patterns abstracted into some primitives and their combinations.For example, a big circle, two small circles and two triangles sufficiently form a human face. 


## Related Work
1. Editable Image Geometric Abstraction via Neural Primitive Assembly（2023-ICCV）
   这项研究提出了一种新颖的图像几何抽象范式，通过组合一组预定义的简单参数化基本图元（如三角形、矩形、圆形和半圆），在无监督的情况下实现图像信息的表征。能够使用四种简单的图形原语（如三角形、矩形、圆形和半圆形）有效捕捉图像中的几何信息。此外，在推理过程中，用户只需简单地替换原语类型，即可实现形状的可控编辑，从而大大提高了操作的灵活性。
2. Modern Evolution Strategies for Creativity: Fitting Concrete Images and Abstract Concepts（2022-EvoMUSART）
   这项研究通过将现代进化策略（ES）算法与受极简主义艺术风格启发的三角形绘图原语相结合，重新审视了进化算法在计算创造力中的应用。研究表明，与传统遗传算法相比，所提出的方法在质量和效率上有显著提升，并且其表现与基于梯度的方法相当。ES算法能够生成多样且独特的几何抽象艺术作品，这些作品与人类对语言和图像的解读相一致。
3. Pic2PolyArt: Transforming a photograph into polygon-based geometric art（2021-Signal Processing: Image Communication)
   