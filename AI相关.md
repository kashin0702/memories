### 前端AI要掌握的能力

LLM能力产品化

Prompt / RAG / Agent 产品化



### Prompt 结构化指令系统

在人工智能（尤其是大语言模型，如我这样的AI）的语境中，**“prompt”**（中文常译为“提示”或“提示词”）是指**用户输入给AI的指令、问题或上下文信息**，用于引导AI生成期望的输出。

简单来说，**prompt 就是你告诉 AI 要做什么的方式**。

system prompt(角色，规则)

User prompt (用户输入)

Context prompt(上下文，历史)

output prompt (输出格式)

- AI 的输出质量高度依赖于 prompt 的清晰度和具体性。
- 好的 prompt（称为“高质量提示”或“有效提示”）能显著提升 AI 回答的准确性、相关性和实用性。
- 这也催生了一个新技能——**Prompt Engineering（提示工程）**，即如何设计和优化 prompt 来获得最佳结果。



#### 前端可做的

Prompt模板化和参数化

prompt可配置（JSON/表单/DSL）

prompt版本管理

prompt动态注入上下文

prompt基础安全（防注入）



### RAG 检索增强生成

1. **你问一个问题**
   比如：“2025年诺贝尔物理学奖得主是谁？”

2. **AI 先去“查资料”**（这就是 **Retrieval 检索**）
   它不是靠自己记忆，而是快速从**最新的、可靠的文档库**（比如维基百科、公司数据库、新闻等）里找出相关段落。

3. **然后它看着这些资料写答案**（这就是 **Generation 生成**）
   它把查到的信息整理成通顺、易懂的回答，而不是瞎猜

   

   没有 RAG 的 AI：像一个只靠记忆答题的学霸，但可能答错新问题。

   有 RAG 的 AI：像一个能随时翻课本、查手机的聪明学生，答案更准、更新、更有依据。

总结：**RAG = 先查资料 + 再写答案，让 AI 不靠瞎猜，而是“有据可依”地说话。**



- **传统模型**：输入 prompt → 直接输出答案（端到端生成）
- **RAG 模型**：
  输入 prompt → **检索外部数据库** → 拿到相关文档 → **把文档+问题一起喂给大模型** → 生成答案

| 如果你需要……           | 选哪个？                |
| ---------------------- | ----------------------- |
| 写一首浪漫的情诗       | 传统生成模型            |
| 回答公司内部政策问题   | RAG                     |
| 获取2025年最新科技新闻 | RAG（配合更新的数据库） |
| 聊天闲扯、发散思维     | 传统模型更自然          |
| 避免AI胡说八道         | RAG 更可靠              |



#### 前端可做的

知识管理UI

文档上传（pdf/word/md）

文档切片（chunk size / overlap）

文档状态（已向量化 / 处理中）



常见技术

文件上传（分片/断点续传）

文档预览（pdf.js / markdown-it）

RAG可解释性展示（高级）

命中来源高亮，回答引用来源，支持“基于哪段文档回答的”



前端常用RAG库

| 类型         | 库              |
| ------------ | --------------- |
| AI SDK       | LangChain.js    |
| 向量可视化   | 自研            |
| Markdown渲染 | react-markdown  |
| 引用高亮     | range + mark.js |



### Agent 流程编排UI （多智能体）

拖拽式 agent flow

步骤配置

条件分支

前端库

| 场景     | 技术                  |
| -------- | --------------------- |
| 流程图   | react flow/ vue flow  |
| 状态管理 | Zustand/ Mobx / pinia |
| 实时通信 | Websocket / SSE       |
| 日志流   | Streaming UI          |



### AI Streaming

核心：

SSE(Sever-Sent Events)

webSocket

Token Streming

Abort/ Retry

前端要考虑：流式渲染，光标效果，中断生成，多轮上下文



### AI应用常见前短裤

**AI SDK / API**

openAI SDK / DeepSeek SDK / 其他大模型SDK

LangChain.js  / LangGraph.js / LangSmith

Vercel AI SDK 

**UI 组件**

Chat UI (自研 / shadcn)

Markdown渲染

Code block高亮



### 前端AI 全栈项目长什么样

Chat UI + streaming

Prompt模板系统

RAG知识库

Agent 流程

Token & 成本可视化