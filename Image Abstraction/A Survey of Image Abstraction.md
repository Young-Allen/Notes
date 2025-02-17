![](Image%20Abstraction.png)

# Introduction
「抽象」是「具象」的相对概念，是就多种事物抽出其共通之点，加以综合而成一个新的概念，此一概念就叫做「抽象」。抽象绘画是以直觉和想象力为创作的出发点，排斥任何具有象征性、文学性、说明性的表现手法，仅将造形和色彩加以综合、组织在画面上，因此抽象绘画呈现出来的纯粹形色。目前Diffusion model在图像生成领域大放异彩，但是以SDXL为代表的基于U-Net的网络架构在训练时并没有对形态和神态抽象的图像进行特殊训练，导致其生成的图像偏向于写实风格，其图像特点多偏向于具有复杂的阴影或者是笔触等。抽象化的图像因其形态和神态的抽象层面，可以使用较少的笔触表达丰富的内容，因此对于图像抽象化生成的研究是具有意义的。

目前，图像抽象的研究主要可分为三个层面：基于像素的抽象、基于线稿的抽象和基于几何图形的抽象。其中基于像素的抽象的研究大多集中在扩散模型诞生之前，使用卷积的类似手段将图像的复杂的特征进行简化。而在CLIP等模型诞生后，又有许多工作基于线条的形式，将图像抽象成为线稿图。基于几何图形的抽象是将图像抽象为一些特定几何图形的组合，比如说三角形、圆形和矩形等。但是，目前基于几何图形的抽象研究相对较少。

# Preliminarles

# Pixel-based Image Abstraction
## Key Words
1. non-photorealistic rendering
2. image simplification
3. image abstraction
4. image segmentation

## Related Work
1. **Stylization and Abstraction of Photographs(2002-SIGGRAPH)**：本文提出了一种新的计算方法，用于对照片进行样式化和抽象处理，以增强图像中重要视觉结构的清晰度。该方法通过识别照片中的视觉部分和边界，提供保留和突出关键元素的框架。用户只需通过注视图像来识别重要内容，系统会利用眼动数据来预测图像中携带重要信息的元素，并应用一系列转换（如去除细节、平均颜色、叠加粗线边缘），以增强图像的视觉表现。
2. **Image Abstraction by Structure Adaptive Filtering(2008-TPCG)**：本文提出了一种非真实感图像处理方法，旨在从3D应用的高信息密度图像中提取出关键视觉信息，以优化小屏幕显示效果。该方法改进了Winnemöller等人的迭代双边滤波（用于图像抽象）和差分高斯（DoG）边缘提取方法，适应局部方向，且计算更高效。通过构建平滑张量场并在梯度方向上垂直滤波，该方法能在曲线边界上获得平滑输出，适合颜色量化处理。相比传统DoG边缘，基于流的差分高斯（FDoG）生成的线条更连贯，同时计算复杂度更低。
3. **Pixelated Image Abstraction(2012-NPAR)**：本文提出了一种自动化方法，将高分辨率图像转化为像素艺术风格的低分辨率、有限调色板图像。该方法首先使用改进的图像分割方法对图像区域进行映射；接着，通过质量约束的退火法优化调色板的选择和像素关联。
# Sketch-based Image Abstraction
## Related Work
1. **CLIPascene: Scene Sketching with Different Types and Levels of Abstraction（2023-ICCV）**：本文提出了一种将自然场景照片转换为多层次抽象草图的计算方法，分为“保真度”和“简约度”两个抽象轴。通过在这两个轴上逐渐移动生成草图，保真度轴侧重于保留图像的几何结构或语义信息，而简约度轴控制细节层次。通过分离前景和背景并引入两个抽象轴，该方法为生成不同抽象层次的草图提供了灵活框架。此外，草图由贝塞尔曲线表示，利用CLIP-ViT和多层感知机（MLP）网络进行训练以保持语义和几何信息。研究表明生成的草图可以在多个抽象层次上展示场景的核心特征，并优于以往基于风格的草图生成方法。
2. **Clipasso: Semantically-aware object sketching（2022-TOG）**：本文提出了一种基于CLIP引导的优化方法，用于将照片转化为抽象草图，不依赖于特定的草图数据集。该方法通过CLIP编码器捕捉目标对象的语义特征，同时利用照片提供的几何基础，实现语义与几何简化。草图由贝塞尔曲线组成，抽象层次由笔触数量控制。利用可微分光栅化器优化笔触的控制点位置，并通过局部注意力图进行显著性引导的初始化，使得生成的草图不仅简约，还保留了输入对象的关键特征。最终生成的草图能够有效表达输入对象的语义和视觉信息，并提供较高的类别与实例识别能力。
3. **Learning Realistic Sketching: A Dual-agent Reinforcement Learning Approach（2024-MM）**：本文提出了一种基于双智能代理的新方法，用于实现逼真且具有语义准确性、风格一致性和细节完整性的素描生成。针对传统网格分割导致的笔触不连续和分配不均的问题，引入了注意力代理，通过动态调整绘制区域位置和大小，灵活分配笔触并提高笔触连续性。此外，本文引入了一个素描风格特征提取器和绘图代理，用于语义提取与风格转换分离，并通过分布奖励和XDoG奖励机制确保草图细节完整性。实验表明，在相同笔触数量下，该方法在视觉效果和感知指标上均优于当前最先进方法，特别是在多种真实场景图像中的素描实验中展现了优异表现，生成的绘图更为逼真生动。
4. **Sketch Generation with Drawing Process Guided by Vector Flow and Grayscale（2021-AAAI）**：本文提出了一种新颖的图像转铅笔素描算法，不仅能够生成高质量的铅笔画，还可展示绘画过程。该算法通过观察真实的铅笔素描绘制过程，并借鉴艺术家笔触的方向、阴影等特点，将绘制过程分为两步：首先开发基于参数控制的笔触生成机制，随后通过引导框架进行笔触排列。与现有方法相比，该算法在纹理质量、风格表现和用户评价方面表现优越。此方法通过模拟艺术家绘画技术，提供了更强的可解释性，能够生成具有不同视觉效果的铅笔素描。
5. **Learning Deep Sketch Abstraction（2018-CVPR）**：本文提出了第一个深度素描抽象模型，开发了一种基于循环神经网络（RNN）的抽象模型，学习评估每个笔画片段的重要性，并决定是否跳过或保留该片段。任何部分的去除对可识别性的影响都与其他部分的保留/去除相互依赖。该模型能够在类别级和实例级合成中实现对抽象水平的控制，同时提出了一种新的照片到手绘素描合成方法，用于生成细粒度素描图像检索（FG-SBIR）训练所需的多样化数据，避免了对照片素描对的依赖。实验表明，该模型在信息传达的效果上超过了现有方法，为拓展FG-SBIR提供了新的思路和潜力。
6. **Synthesizing human-like sketches from natural images  using a conditional convolutional decoder（2020-WACV）**：文章中提及的”human-like“的概念比较有趣。本文旨在开发一种将自然图像转换为人类风格素描的算法，以反映人类抽象能力。研究显示，卷积神经网络（CNNs）在图像分类任务中倾向于纹理，但通过适当的结构和训练，这些网络也能够学到隐含的形状表示。本文采用了一个全卷积编码-解码结构，实现了图像空间到素描空间的映射，生成具有“人类风格”的素描。研究结果表明，这种方法不仅可用于增强基于素描的检索数据库等应用，还对复杂的形状相关任务提供了新的视角。
7. **Learning to generate line drawings that convey geometry and semantics（2022-CVPR）**：文中提出的无监督生成方法，可能可以在一定程度上指导几何抽象过程（因为从图像到几何抽象图的过程也可能面临没有ground truth的问题）本文提出了一种新的无监督方法，用于从照片自动生成有效的线条画。该方法通过显式注入几何形状和语义信息，解决了传统自动生成方法中缺乏配对训练数据的问题。通过深度学习，方法能够从线条画中解码出深度、语义和外观信息，从而确保生成的线条画准确地反映输入图像的几何和语义。


# Geometric-based Image Abstraction

## Keywords
1. Geometric art 
2. Geometric abstraction
3. Image abstraction
4. Image simplification
5. Parametric Primitives
6. Low poly rendering
7. Style transfer 
8. Non-photorealistic rendering


## Problems that Need Attention
These are two of the main reasons why modern logo design and animation especially favor the use of geometric primitives.

1. **Simplicity and Expressibility**: On one hand, geometric primitive has very compact and explicit parameter format, thus facilitating compact representation and convenient manipulation. On the other hand, numerous combinations of geometric primitives imply rich expressive power compared with sketches. For example, the Bauhaus and Cubism arts tend to adopt ONLY simplistic shapes like rectangles and spheres, yet still with extremely rich artistic expression.
2. **Semantic Preservation**: Human beings tend to think of complex visual patterns abstracted into some primitives and their combinations.For example, a big circle, two small circles and two triangles sufficiently form a human face. 


## Related Work
1. **Editable Image Geometric Abstraction via Neural Primitive Assembly**（**2023-ICCV**）：这项研究提出了一种新颖的图像几何抽象范式，通过组合一组预定义的简单参数化基本图元（如三角形、矩形、圆形和半圆），在无监督的情况下实现图像信息的表征。能够使用四种简单的图形原语（如三角形、矩形、圆形和半圆形）有效捕捉图像中的几何信息。此外，在推理过程中，用户只需简单地替换原语类型，即可实现形状的可控编辑，从而大大提高了操作的灵活性。
2. **Modern Evolution Strategies for Creativity: Fitting Concrete Images and Abstract Concepts**（**2022-EvoMUSART**）：这项研究通过将现代进化策略（ES）算法与受极简主义艺术风格启发的三角形绘图原语相结合，重新审视了进化算法在计算创造力中的应用。研究表明，与传统遗传算法相比，所提出的方法在质量和效率上有显著提升，并且其表现与基于梯度的方法相当。ES算法能够生成多样且独特的几何抽象艺术作品，这些作品与人类对语言和图像的解读相一致。
3. **Pic2PolyArt: Transforming a photograph into polygon-based geometric art**（**2021-Signal Processing: Image Communication**)：Pic2PolyArt是一个统一的以主题为中心的几何抽象框架，可以支持基于三角形和基于多边形的抽象。给定输入照片，我们提出的算法首先结合显着性、边缘和面部检测技术来识别图像的主要主题和重要特征。然后，它生成一组种子点，Delaunay 三角剖分和 Voronoi 曲面细分使用这些种子点分别生成基于三角形和基于多边形的几何抽象。
4. **Pic2Geom: A Fast Rendering Algorithm for Low-Poly Geometric Art**（**2017-PCM**）：该算法利用边缘检测、显着性检测和人脸检测来生成一组种子点，然后由 Delaunay 三角剖分使用该种子点来生成低多边形抽象。
5. **Cubist Style Rendering from Photographs**（**2003-IEEE TCVG**）：这项研究首先引入了照片的计算立体派风格渲染。他们的方法将同一场景的图像集中的显着特征（例如眼睛、鼻子和嘴巴）识别为构图元素，并在渲染构图之前对这些特征进行几何扭曲，以产生立体派艺术中常见的更有角度的形式。
5. **Abstract Art by Shape Classification**（**2013-IEEE TVCG**）：这项工作提出了一种通过形状分类生成抽象艺术的方法。给定一幅图像，该方法从图像分割层次中获取输入区域并输出最佳拟合形状；例如圆形、三角形和矩形，替换输入区域以形成抽象艺术。
6. **Approximation by piecewise polynomials on Voronoi tessellation**（**2014-Graphical Models**）：提出了一种通过结合 Voronoi 细分来生成解析函数和彩色图像的分段多项式近似的方法。该算法利用梯度信息来分配种子点，使得生成的Voronoi区域的边界与原始图像的特征线对齐。结果表明，该方法仍然缺乏保持主要特征可识别的信息，因为梯度信息本身可能无法有效识别感兴趣区域。
7. **Fogleman Primitive**（**2017**）：提出了一种名为 Primitive 的算法，它用重叠的几何基元（例如三角形、矩形和椭圆形）来再现抽象。该算法通过从空白画布开始，一次迭代地添加一个形状来模拟人类绘图方法。形状是随机生成的，如果形状不具有一定程度的相似性，算法就会改变形状。
8. **A generic framework for the structured abstraction of images**（**2017-NPAR**）：提出了一个图像结构化抽象的通用框架，可以生成从几何抽象和绘画效果到风格转换的照片抽象渲染。所提出的框架首先将输入图像分解为拓扑树图，然后对其进行修改，以便删除低于给定阈值的小形状（形状选择），并将剩余形状替换为椭圆形和矩形等形状。然后以重叠的方式将这些形状一一渲染，形成最终的抽象。