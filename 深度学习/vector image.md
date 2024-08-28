一般矢量图的生成可以分为两类，
1)：初始化SVG路径，然后转换为raster并计算损失，反向传播迭代优化SVG生成最后的矢量图；
2)：将光栅图直接进行矢量化来生成矢量图（image vectorization）。
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

### 3. VectorPainter: A Novel Approach to Stylized Vector Graphics Synthesis with Vectorized Strokes(2024)
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


### 4. VectorFusion: Text-to-SVG by Abstracting Pixel-Based Diffusion Models(2023)
前置论文知识（DiffVG、LIVE）
- **Methods**：
  1. **A baseline: text-to-image-to-vector**
       ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240805145838.png)
     我们首先开发了一个两阶段的流程：从Stable Diffusion生成图像，然后自动将其矢量化。给定文本，我们使用Runge-Kutta求解器在50个采样步骤中以指导比例ω = 7.5（Diffusers库中的默认设置）从Stable Diffusion采样出一个光栅图像。通常，扩散模型生成的图像具有摄影风格和细节，这些很难用少量常量颜色的SVG路径来表达。为了鼓励生成抽象的、平面的矢量风格图像，我们在文本后附加一个后缀：“minimal flat 2d vector icon. lineal color. on a white background. trending on artstation”。这个提示经过定性调整。
     由于生成的样本可能与标题不一致，我们采样K个图像，并根据CLIP ViT-B/16选择与标题最一致的Stable Diffusion样本。
     接下来，我们使用现成的分层图像矢量化程序（LIVE）自动跟踪光栅样本以将其转换为SVG。LIVE通过在高损失区域局部初始化路径，逐步生成相对干净的SVG。为了鼓励路径仅解释图像的单个特征，LIVE通过距离最近路径的距离对L2重建损失进行加权，
     ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240805144242.png)
     这样生成了一组路径$\theta_{LIVE} = \{p1, p2, \ldots, pk\}$。图3(b)显示了在添加8-16条路径的阶段中优化矢量参数的过程。图1显示了更多自动转换的结果。尽管简单，这个流程通常会生成不适合矢量化的图像。
  2. **Sampling vector graphics by optimization**
    对于VectorFusion，我们调整了Score Distillation Sampling以支持潜在扩散模型（LDM），如开源的Stable Diffusion。我们初始化一个包含路径集合$\theta = \{p_1, p_2, \ldots, p_k\}$的SVG。每次迭代中，DiffVG渲染一个600×600的图像x。像CLIPDraw一样，我们通过透视变换和随机裁剪来增强图像，得到512×512的图像$x_{aug}​$。然后，我们建议在潜在空间中使用LDM编码器$E_{\phi}$​来计算SDS损失，预测$z = E_{\phi}(x_{aug})$。对于每次优化迭代，我们用随机噪声$z_t = \alpha_tz + \sigma_t\epsilon$对潜在变量进行扩散，用教师模型$\hat{\epsilon}_{\phi}(z_t, y)$去噪，并使用方程4的潜在空间修改来优化SDS损失：
    ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240805170936.png)
    由于Stable Diffusion是一个离散时间模型，具有T=1000个时间步长，我们从$t \sim U(50, 950)$中采样。为了提高效率，我们在半精度下运行扩散模型$\hat{\epsilon}_{\theta}$。我们发现，为了数值稳定性，重要的是在全FP32精度下计算编码器的雅可比矩阵$\frac{\partial z}{\partial x_{aug}}​$。项$\frac{\partial x_{aug}}{\partial \theta}$​​通过增强和可微矢量图形光栅化器DiffVG进行自动微分计算。$_{LSDS}$可以看作是$LSDS$的一个适配，其中光栅化器、数据增强和冻结的LDM编码器被视为具有可优化参数$\theta$的单个图像生成器。在优化过程中，我们还通过对自交进行正则化。
    ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240805171900.png)
  3. **Reinitializing paths**
     在我们最灵活的设置中，为了合成扁平图标化矢量图，我们允许优化路径控制点、填充颜色和SVG背景颜色。在优化过程中，许多路径会学习到低不透明度或缩小到一个小区域，从而未被使用。为了鼓励路径的使用，从而生成更具多样性和细节的图像，我们定期重新初始化不透明度或面积低于阈值的路径。重新初始化的路径将从优化和SVG中移除，并作为随机定位和上色的圆形在现有路径上重新创建。
   

### 5. Differentiable Vector Graphics Rasterization for Editing and Learning（2020）
- **Background：**
  我们引入了一种可微分的光栅化器，它连接了矢量图形和光栅图像领域，使得基于光栅的损失函数、优化程序和机器学习技术能够编辑和生成矢量内容。我们观察到，在像素预滤波后，矢量图形的光栅化是可微分的。我们的可微分光栅化器提供了两种预滤波选项：一种是解析预滤波技术，另一种是多重采样抗锯齿技术。解析预滤波变体更快，但可能会出现诸如合并等伪影。多重采样变体仍然高效，并且能够渲染高质量图像，同时计算每个像素相对于曲线参数的无偏梯度。
  我们展示了我们的光栅化器启用的新应用，包括：
  1. **由图像度量引导的矢量图形编辑器**：这种编辑器可以根据图像度量来调整矢量图形。
  2. **绘画风格的渲染算法**：通过最小化深度感知损失函数，将矢量原语拟合到图像中。
  3. **新矢量图形编辑算法**：利用诸如缝合雕刻等知名的图像处理方法进行矢量图形编辑。
  4. **深度生成模型**：这些模型在VAE或GAN训练目标下，利用仅有光栅监督来生成矢量内容。
     这些应用展示了我们的可微分光栅化器在矢量图形和光栅图像领域的强大功能，使得复杂的矢量图形生成和编辑成为可能。

### 6. （LIVE）Towards Layer-wise Image Vectorization(2022)
(层次的图像矢量化)
- **Methods**：
  我们首先介绍了一个组件初始化方法，用于选择主要组件作为初始化点。然后，我们运行一个递归流程，逐步根据路径数量调度序列N添加n条路径。对于每一步，我们基于一些新提出的目标函数来优化图形，包括无符号距离引导的焦点（UDF）损失和自交叉（Xing）损失，以获得更好的重建质量和自交互问题的优化结果。除了逐层表示能力外，我们的方法能够用最少数量的贝塞尔路径重建图像，与其他方法相比显著减少SVG文件大小。更多细节将在以下部分中介绍。
  ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240807151942.png)
  1. **Component-wise Path Initialization**：
    一个不好的初始化会导致拓扑提取失败并生成冗余形状。为了克服这一缺陷，我们引入了组件级路径初始化，这大大有助于优化过程。组件级路径初始化的设计原则是根据每个组件的颜色和大小来确定路径的最合适初始位置。一个组件是一个填充颜色均匀的连通区域。正如我们之前提到的，LIVE是一个渐进学习流程。根据前阶段的SVG输出，我们优先考虑下一个学习目标，使组件既大又缺失。
    我们通过以下步骤来验证这种组件：
    a) 计算当前渲染的SVG与真实图像之间的L1像素级颜色差异。
    b) 拒绝小于预设阈值cα的颜色差异。根据经验，本文中cα = 0.1。颜色差异小于cα的像素区域被认为已正确渲染。
    c) 对于其他区域，将所有有效的颜色差异值大于cα的区域均匀量化为200个bin。量化近似均匀分布；
    d) 最后，我们根据量化结果识别出最大的连通组件，然后使用其质心作为下一个路径的初始位置。如果我们想添加K条路径，则选择前K个组件作为下一阶段的初始化。注意，对于每条路径，我们考虑将所有控制点均匀初始化在圆上的初始化方法。在向现有图形中添加新路径时，我们的初始化方法始终能够识别出颜色相似的最大缺失组件，并填补主要区域。
  2. **UDF Loss for Reconstruction：**
     在不失一般性的情况下，我们假设只有一条路径的情况来公式化我们的UDF损失。我们渲染路径并计算每个像素到路径的有符号距离，由$di​,i∈\{1,...,h×w\}$表示。然后我们阈值化、翻转并标准化无符号距离 $|d_i|$：
     
     $d_i'=\frac{\mathrm{ReLU}(\tau-|d_i|)}{\sum_{j=1}^{w\times h}\mathrm{ReLU}(\tau-|d_j|)},$
     
     其中 $i$ 和 $j$ 都是像素的索引，$\tau$ 是距离阈值。默认情况下，我们设置 $\tau$ 为10。接下来，我们将无符号距离引导的焦点损失公式化为：
     
     $L_{\mathrm{UDF}}=\frac{1}{3}\sum_{i=1}^{w\times h}d_i'\sum_{c=1}^3(I_{i,c}-\hat{I}_{i,c})^2,$ 
     
     其中  $i$ 索引 $I$ 中的像素，$c$ 索引 RGB通道。在UDF损失的帮助下，我们能够密切关注路径轮廓，避免内层或远处区域的影响。图2展示了无符号距离引导的焦点损失的学习过程。为了在我们的LIVE框架中支持多条路径，我们可以通过对所有路径的 $d'_i$ 取平均值来轻松扩展公式2。 
  3. **Xing Loss for Self-Interaction Problem：**
     我们注意到在优化过程中，一些贝塞尔路径可能会发生自交，导致有害的伪影和不适当的拓扑结构。
     ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240808095930.png)
     假设本文中的所有贝塞尔曲线都是三阶的，通过分析许多优化后的形状，我们发现自交路径总是会与其控制点的线相交，反之亦然。这表明，与其优化贝塞尔路径，不如在控制点上添加约束。假设三次贝塞尔路径的控制点依次为A、B、C和D，我们添加一个约束，即 $\overrightarrow{AB}$ 和 $\overrightarrow{CD}$ 之间的角度（图中的 $\theta$）应大于180°。我们首先确定 $\angle ABC$ 的特征（锐角或钝角）作为D1，并通过以下公式确定 $\theta$ 的正弦值D2：
     ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240808100207.png)其中 $\mathbb{I}(\cdot)$ 是符号函数，当D1 > 0时返回1，否则返回0，$\times$ 是向量积，返回一个实值。然后我们将Xing损失公式化为：
     ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240808101344.png)
     公式4的基本思想是我们仅在 $\theta < 180^\circ$ 时进行优化（通过 $\text{ReLU}(\pm D2)$ 实现）。第一项针对D1 = 1的情况设计，第二项针对D1 = 0的情况设计。结合UDF损失和Xing损失，我们的最终损失函数L为：
     ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240808101457.png)

- **Disscusion:**
  1. 分层操作效率不如单次优化。其他一些方法也遇到了这个问题。一**个有趣的研究方向是如何结合深度模型的高效推理与基于优化方法的泛化能力。**
  2. 引入渐变色并自适应选择每个段的段数和颜色类型将是值得探索的方向。
  3. 对于更复杂的图像，如风景或人物照片，将分层矢量化与像素空间中的深度非完全分割结合起来将是一个有趣的话题。


### 7. DeepSVG: A Hierarchical Generative Network for Vector Graphics Animation（2020）
- **Methods**：
  1. SVG Dataset and representation
     从SVG-icon8中引入了新的数据集，该数据集包含56个不同类别的100000个高质量图标。
  2. Data structure
    ![](https://raw.githubusercontent.com/Young-Allen/pic/main/20240828103859.png)
  3. SVG Embedding
     ![image.png](https://raw.githubusercontent.com/Young-Allen/pic/main/20240828105534.png)


