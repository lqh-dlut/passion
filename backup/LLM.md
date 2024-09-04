**RAG: retrieval-augmented generation检索增强生成**
> 整合了从庞大知识库中检索到的相关信息，并以此为基础，指导大型语言模型生成更为精准的答案，从而显著提升了回答的准确性与深度

1. 数据处理：清洗处理，转化格式，存数据库
2. 检索阶段：问题输入，数据库检索
3. 增强阶段：检索得到信息进行增强
4. 生成阶段：增强的信息输入到模型中，模型生成答案

![image](https://github.com/user-attachments/assets/604a3396-1cc5-4087-9107-5e8b76d79be6)


### LangChain
> 核心组件：Model I/O, Data connection, Chains, Memory, Agents, Callbacks

### 大模型的开发
> 通过 Prompt Engineering、数据工程、业务逻辑分解等手段来充分发挥大模型能力，适配应用任务

### 基本概念
1. Prompt： 在自然语言吃力中是一个用于引导模型生成特定输出的输入文本或问题，替代给LLM的输入，而completion替代LLM输出

2. Temperature：该参数用于控制LLM生成结果的随机性与创造性
> 参数在0~1之间，越靠近0越保守，可预测；越接近1越随机，更有创意，多样化

3. System Prompt：提升用户体验的一种策略，初始化设定，与User Prompt相区分