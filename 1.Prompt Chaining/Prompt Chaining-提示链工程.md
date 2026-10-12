采用**分而治之**架构构建复杂智能体系统的实践指南
传统单提示的认知瓶颈（单一指令）
- 指令忽视
- 上下文漂移
- 幻觉飙升
提示链：从“单词博弈“到”可靠工作流“的智能基石
提示链模式（PROMPT CHAINING）：（分而治之Divide and Conquer）
输入-子任务1-子任务2-最终输出
将庞大问题拆解为可管理的小子问题的工程范式转变
User-Prompt1-...-Agent 1 - Output -> Prompt n - ... -> Agent n - final Output
步骤N的输出作为步骤N+1的输入，
结构化握手（Structure Handshaker）：节点之间强制使用JSON或XML等结构化数据格式进行传递，防止自然语言的模糊性导致整控模糊性导致整条链条崩溃

核心工程：上下文工程（Context Engineering）
在生成Token之前，为AI模型设计、构建并交付完整信息环境的系统学科
Context Engineering：
Prompt Engineering、Structured Outputs、RAG、State/History、Memory

