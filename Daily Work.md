#### 时间：7月14日
1. 看完diffusion model原理
2. 看完了综述Neural Style Transfer: A Review，详细了解了一下style transfer
3. 下载stylealigned模型

#### 时间： 7月15日
1. 学习Stable Diffusion模型原理
2. 看style-aligned论文
3. 试着跑了一下style-aligned（结果显存不够，申请服务器）
4. 下载LlamaGen模型来玩一玩
#### 时间：7月16日
1. 配服务器环境
#### 时间 7月17日
1. 运行style-aligned代码
2. 看DreamBooth论文
#### 时间7月18日
1. DeBug代码，style-aligned
#### 时间7月19日
1. 初步运行style-aligned代码得到结果
2. 继续看style-aligned论文，结合代码分析训练步骤和细节

#### 时间7月21日
1. 再看了一遍Image Vectorization: a Review这篇文章，主要分析他提出的5个进行实验对比的指标，思考对比实验可以采用什么方法和指标。
   - similarity to the original bitmap；
   - the simplicity or complexity of the resulting image including the number of shapes and their parameters；
   - the speed of generation；
   - versatility — the ability to generate a fairly accurate copy of the input image without prior model training；
   - human control to adjust hyperparameters.
   总结来说，矢量化方法在图像质量、路径数量、段数、路径是否封闭、迭代次数和运行时间之间存在权衡。（我们在生成矢量图形时需要考虑的因素）
