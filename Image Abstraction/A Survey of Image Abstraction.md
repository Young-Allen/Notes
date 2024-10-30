# Abstract

# Introduction
「抽象」是「具象」的相对概念，是就多种事物抽出其共通之点，加以综合而成一个新的概念，此一概念就叫做「抽象」。抽象绘画是以直觉和想象力为创作的出发点，排斥任何具有象征性、文学性、说明性的表现手法，仅将造形和色彩加以综合、组织在画面上，因此抽象绘画呈现出来的纯粹形色。目前Diffusion model在图像生成领域大放异彩，但是以SDXL为代表的基于U-Net的网络架构在训练时并没有对形态和神态抽象的图像进行特殊训练，导致其生成的图像偏向于写实风格，其图像特点多偏向于具有复杂的阴影或者是笔触等。抽象化的图像因其形态和神态的抽象层面，可以使用较少的笔触表达丰富的内容，因此对于图像抽象化生成的研究是具有意义的。

目前，图像抽象的研究主要可分为三个层面：基于像素的抽象、基于线稿的抽象和基于几何图形的抽象。其中基于像素的抽象的研究大多集中在扩散模型诞生之前，使用卷积的类似手段将图像的复杂的特征进行简化。而在CLIP等模型诞生后，又有许多工作基于线条的形式，将图像抽象成为线稿图。基于几何图形的抽象是将图像抽象为一些特定几何图形的组合，比如说三角形、圆形和矩形等。但是，目前基于几何图形的抽象研究相对较少。

# Preliminarles

# Pixel-based Image Abstraction


# Sketch-based Image Abstraction


# Geometric-based Image Abstraction
## Occupy
These are two of the main reasons why modern logo design and animation especially favor the use of geometric primitives.

1. **Simplicity and Expressibility**: On one hand, geometric primitive has very compact and explicit parameter format, thus facilitating compact representation and convenient manipulation. On the other hand, numerous combinations of geometric primitives imply rich expressive power compared with sketches. For example, the Bauhaus and Cubism arts tend to adopt ONLY simplistic shapes like rectangles and spheres, yet still with extremely rich artistic expression.
2. **Semantic Preservation**: Human beings tend to think of complex visual patterns abstracted into some primitives and their combinations.For example, a big circle, two small circles and two triangles sufficiently form a human face. 


## Related Work
1. Editable Image Geometric Abstraction via Neural Primitive Assembly（2023ICCV）
   介绍了一种图像几何抽象方法，通过一组预定义的简单参数化基本图元（如三角形、矩形、圆形和半圆）进行装配，方便对图像中的形状进行可控编辑。
2. Modern Evolution Strategies for Creativity: Fitting Concrete Images and Abstract Concepts（2022EvoMUSART）
   