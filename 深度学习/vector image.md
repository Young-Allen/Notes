一般矢量图的生成可以分为两类，
1)：直接生成矢量图，
2)：将光栅图进行矢量化来生成矢量图（image vectorization）。
其中2）image vectorization：还可以细分为algorithmic-based和machine learning-based两种方法。
### 1. Image Vectorization: a Review（2023）
- 本文是对Machine Learning-based的方法进行的综述，将Machine Learning-based的一些方法进行实验比较，以下是矢量化的主要对比指标：
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

