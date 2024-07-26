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
1. Style Stroke Extraction：
   
