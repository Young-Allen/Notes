# 1. 引言
## 1.1 语言模型的发展历程
1. 分布式词向量又称为”词嵌入“（Word Embedding）
2. 统计语言模型（Statistical Language Model, SLM）；
3. 神经语言模型（Neural Language Model, NLM）；
4. 预训练语言模型（Pre-trained Language Model, PLM）
   - 预训练阶段旨在通过大规模无标注文本建立模型 的基础能力，而微调阶段则使用有标注数据对于模型进行特定任务的适配，从而 更好地解决下游的自然语言处理任务。
   - 编码器架构被认为更适合去解决自然语 言理解任务（如完形填空等），而解码器架构更适合解决自然语言生成任务（如文 本摘要等）
5. 大语言模型（Large Language Model, LLM）
   - GPT-3 可以通过“上下文学习”（In-Context Learning, ICL）的方 式来利用少样本数据解决下游任务，而 GPT-2 则不具备这一能力。这种大模型具 有但小模型不具有的能力通常被称为“**涌现能力**”（Emergent Abilities）。
## 1.2 LLM的能力特点
- 具有较为丰富的世界知识.
- 具有较强的通用任务解决能力.
- 具有较好的复杂任务推理能力.
- 具有较强的人类指令遵循能力.
- 具有较好的人类对齐能力.
- 具有可拓展的工具使用能力.