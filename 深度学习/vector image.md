一般矢量图的生成可以分为两类，
1)：直接生成矢量图，
2)：将光栅图进行矢量化来生成矢量图（image vectorization）。
其中2）image vectorization：还可以细分为algorithmic-based和machine learning-based两种方法。
### 1. Image Vectorization: a Review（2023）
- 本文是对image vectorization中的Machine Learning-based的方法进行的综述，将Machine Learning-based的一些方法进行实验比较，以下是矢量化的主要对比指标：
  1）：similarity to the original bitmap；
  2）：the simplicity or complexity of the resulting image including the number of shapes and their parameters；
  3）：the speed of generation；
  4）：versatility — the ability to generate a fairly accurate copy of the input image without prior model training；
  5）：human control to adjust hyperparameters.

- **文中提到的Machine Learning-compatible Methods：**
1. DiffVG
2. Im2Vec
3. LIVE
4. ClipGen
5. Mang2Vec
6. Deep Vectorization of Technical Drawings
![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240721203140.png)
- **Conclusion**：所有现有的机器学习兼容方法也需要人工控制和调整方法迭代次数、输出矢量图像参数等。Im2Vec模型无法存储和生成复杂图像，也不是一个可以为任何输入图像创建矢量模拟的通用矢量化工具。LIVE方法是唯一允许您控制绘制形状数量的通用模型，但是由于采用迭代方法，生成单个图像需要大量时间。根据我们的测量，DiffVG在机器学习方法中是最快的，并且质量没有明显损失。然而，高质量的结果需要大量路径。同时，LIVE能够使用更少的形状获得不逊色的质量。但是，LIVE方法的主要问题是其超长的运行时间。**总的来说，可能最好的选择是使用在线方法，但它们不允许调整应用形状的数量**。总结来说，矢量化方法在图像质量、路径数量、段数、路径是否封闭、迭代次数和运行时间之间存在权衡。
### 2. A Survey of Smooth Vector Graphics: Recent Advances in Representation, Creation, Rasterization, and Image Vectorization（2022）
- 本文是对image vectorization中的algorithmic-based的方法进行的综述，
- 一些文章中提到的问题：
  1. 矢量图形的简洁性可能是一把双刃剑。虽然创建简单的色彩渐变很容易，但像平滑阴影（smooth shadows）、腐蚀效果（caustic effects）和散焦模糊（defocus blur）等复杂的色彩变换却很难通过基本的渐变函数直接表现出来。（是否可以使用相关的生成模型，通过优化渐变函数的生成，来达到更容易生成更复杂的色彩变换？）
  2. 当所需的细节量增加时，矢量图形的编辑过程既需要扎实的艺术技巧，又需要大量的工作时间才能获得复杂的效果。（能否设计一个便于非艺术专家也可以很好的进行矢量图编辑的工具，在进一步优化生成时间的同时还可以指导用户完成对应工作的需求？）
  3. 矢量图更多地用于表示风格化的图像（stylized images）。（最近的风格化矢量图生成的论文：VectorPainter: A Novel Approach to Stylized Vector Graphics Synthesis with Vectorized Strokes。有点像style aligned的工作）
- **本文将矢量图形的表示法分为两类**：
  1. Mesh-based：将图像域划分为不重叠的2D块，在这些块上插入颜色。该表示方法主要涉及块的放置和连接性，并确定颜色的插值方式。面片形状可以是三角形、矩形或者是不规则图形，而颜色和其他属性存储顶点或面片内部。
  2. Curve-based representations mimic the traditional artistic habits of drawing on paper. The curves usually represent extrema in the color gradient, and the resulting image is often computed by solving a partial differential equation (PDE).

### 3. VectorPainter: A Novel Approach to Stylized Vector Graphics Synthesis with Vectorized Strokes
- **实验目的**：生成风格化的矢量图形。给定文本提示和参考样式的图像，VP（VectorPainter）会生成一个矢量图形，该矢量图形的内容与文本提示对齐，并在样式上保持参考图像的忠实。
- **文章中提及的一些问题**：
  1. 对风格化矢量图生成的研究目前仍然有限。该领域内唯一现有的研究StyleCLIPDraw，主要遵循用于光栅图像的样式传输管道。在生成过程中，渲染合成的矢量图形，并将其风格与光栅图像空间中的参考图像进行比较。
  2. 首先，SVG利用贝塞尔曲线和颜色等基元来描绘图像，这与通常由单独笔画组成的艺术绘画的本质完美契合。其次，艺术家在进行作品绘制的过程当中经常会采用自己独特的笔触风格。最后，风格化过程可以被概念化为重新排列风格图像的笔画以创建新的内容。如果将参考图像解构为矢量笔画的集合，那么这些笔画就可以作为生成新内容的基础元素。
  3. 近年来，风格迁移的研究广泛集中于光栅图像中，而对矢量图形的研究很少。这是因为矢量图形的基本单元是数学公式和几何描述等图元，与光栅图像中的像素不同。因此用于光栅图像的风格转换方法对矢量图不是很有效。
- **Contributions**：
  1. 我们引入了一个新模型VP，它创新地将程序化矢量图生成任务概念化为重新排列参考笔画的过程。这种方法可以提高风格复制和内容生成的保真度。
  2. 我们提出了一种从参考图像中提取一组矢量化笔画的新颖算法。这些笔画是形成新内容的基本元素。此外我们引入了样式保留损失，以在整个生成过程中保持笔画形状的完整性。
  3. 我们进行了大量的实验来评估我们模型的有效性。结果证明VP在生成高质量风格化SVG方面的优越性。
- **METHODOLOGY**：
  ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240726094617.png)
  1. **Style Stroke Extraction**：
     我们从笔触的角度提取和学习参考图像的绘画风格，从而在矢量图形中准确再现参考图像的风格。同一幅艺术作品中的类似笔触表现出类似的属性，如纹理、颜色和亮度，因此形成具有视觉意义的不规则像素簇。利用这一观察结果，我们采用超级像素方法（a super-pixel method）来描绘这些像素簇，促进笔触的分组表示。
     ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240726113303.png)
     如图5所示，当给定一张风格图像时，我们使用 SLIC 方法将图像中具有相似特征的笔触划分为不同区域。我们将这些笔触组矢量化，每个笔触组对应一个 SVG 路径组，而这些 SVG 路径组作为矢量化参考图像的基本表示单元。此外，我们提出了一种新颖的笔触提取算法，用于将提取的区域表示为矢量化的笔触。如算法12所示，为了提取每个分割区域内的笔触，我们提出使用距离最远的一对点作为初始和终端控制点，计算区域的平均颜色作为笔触颜色，并计算边界点和控制点之间连接的平均距离。
     ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240726112850.png)
  2. **Vector Stroke Initialization**：
    如图6所示，提取的矢量笔触可以精确重构原始参考图像，有效地保留其复杂的细节。这些矢量笔触包含参考图像的风格，可用于为矢量图形的生成提供风格先验信息。具体而言，我们建议使用从参考图像中获得的矢量笔触作为文本到SVG生成的初始SVG。与随机初始化矢量笔触形状的方法相比，我们的方法在最大程度上保留了参考图像的风格和细节。图10展示了两种初始化方法的定性比较。我们的方法在笔触层面上对参考图像表现出更高的保真度。
    ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240726114223.png)
 3. **SVG Synthesis with Style Supervision：**
    为了在不显著改变整体优化过程的情况下保持风格一致性，从而在最大程度上保留最终风格并最小化对单个笔触的更改，我们引入了风格保留损失函数（Style Preservation loss）。该损失函数从两个角度监控SVG合成：局部笔触级变形约束（local stroke-level deformation constraints）和整体绘画风格约束（global paint-conscious style constraints）。风格保留损失包含两个部分：一个是通过控制点实现的笔触对笔触级约束，另一个是通过感知相似度确定的全局级风格约束。
    将从参考图像中提取的矢量笔触集合作为ground-truth（GT），在优化的过程中，每一个更新了的笔触都会与GT一一进行计算，
    $\begin{aligned}&\mathcal{L}_{\mathrm{stroke}}(s^{\prime},s)=\frac{1}{N}\sum_{i=1}^{N}\|d(s_{i}^{\prime})-d(s_{i})\|_{2}^{2}\\&&\text{(1)}\\&\mathrm{where~}d(s)=\|\vec{p_{1}p_{2}}\|+\|\vec{p_{2}p_{3}}\|+\cos<\vec{p_{1}p_{2}},\vec{p_{2}p_{3}}>\end{aligned}$
    公式中的$s' \in \{s'_i\}_{i=1}^{N}​$表示笔触集合中的一个笔触，而 $s_i​$ 表示更新后的笔触。 $p_i$​ 表示笔触 $s$和 $s′$的第 $i$ 个控制点。$Lstroke$损失函数在合成的矢量图形中避免过度的笔触变化，从而在微观层面上保持参考图像的风格。
    笔触级损失确保优化中的矢量图形保持与参考图像一致的笔触。然而，这仍然缺乏全局风格一致性约束。因此，我们在风格保留损失中加入了$LPIPS$损失。$LPIPS$损失有助于使生成的风格化矢量图形在感知相似度水平上更接近参考图像$Is$。
    $L_\mathrm{style}=\lambda_sL_\mathrm{stroke}(s',s)+\lambda_gL_\mathrm{LPIPS}(I,Is)$
    其中，$\lambda_s$ 和 $\lambda_g$是风格保留损失的权重。$I = R(θ)$是渲染结果。LPIPS损失鼓励合成的风格化矢量图形在宏观层面上看起来更相似，例如使两幅图像的整体颜色更接近。实验结果表明，我们生成的矢量图形在微观和宏观层面上都与参考图像的风格相匹配。
 4. **Total Loss Objectives**：
   
   
- **EXPERIMENTS**：
  我们将我们的方法（VP）和最近的一些方法进行了比较。
  1. vector graphics synthesis methods：StyleCLIPDraw、Neural Style Transfer for Vector Graphics、Diffsketcher
  2. strokebased rendering methods：Rethinking style transfer: From pixels to parameterized brushstrokes
  3. style transfer methods on raster images：Style transfer by relaxed optimal transport and self-similarity
  ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240726132646.png)





