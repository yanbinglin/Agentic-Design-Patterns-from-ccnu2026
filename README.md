# Agentic Design Patterns｜智能体设计模式解读

> Interpretation materials for *Agentic Design Patterns*, co-created by the 2026 cohort of graduate students in Artificial Intelligence at Central China Normal University (CCNU).
>
> 《Agentic Design Patterns（智能体设计模式）》学习与解读资料，由华中师范大学 2026 级人工智能专业全体研究生共同创作。

<p align="center">
  <img src="assets/Agentic%20Design%20Patterns/book_cover.png" alt="Agentic Design Patterns book cover" width="360">
</p>

<p align="center">
  <a href="#中文说明">中文</a> · <a href="#english">English</a>
</p>

---

## 中文说明

### 项目简介

本仓库汇集了我们在学习 *Agentic Design Patterns* 过程中整理的中文解读、概念说明与相关学习材料，旨在帮助读者系统理解智能体系统中常见的设计模式、实现思路与工程实践。

这些内容由同学们在课程学习与交流中共同完善，适合用于个人学习、课堂讨论和技术交流。

### 内容概览

本项目涵盖以下主题：

- **基础模式**：提示链、路由、并行化、反思、工具使用、规划与多智能体协作；
- **高级系统**：记忆管理、学习与适应、模型上下文协议（MCP）、目标设定与监控；
- **生产实践**：异常处理与恢复、人在回路、知识检索（RAG）；
- **多智能体架构**：智能体间通信（A2A）、资源感知优化、推理技术、安全护栏、评估监控、优先级排序与探索发现；
- **扩展主题**：高级提示技术、智能体交互、智能体框架、命令行智能体与编程智能体等。

### 快速开始

- [在线阅读主要内容](./Agentic%20Design%20Patterns.md)
- [查看中文 PDF](./origin_pdf_resource/Agentic-Design-Patterns-CN.pdf)

你可以直接在 GitHub 中阅读 Markdown 文件，也可以下载 PDF 离线查看。

### 章节与附录目录

**Part One: Foundational Patterns**

- [Chapter 1: Prompt Chaining 提示链](Prompt%20Chaining/readme.md)
- [Chapter 2: Routing 路由](Routing/readme.md)
- [Chapter 3: Parallelization 并行化](Parallelization/readme.md)
- [Chapter 4: Reflection 反思](Reflection/readme.md)
- [Chapter 5: Tool Use (Function Calling) 工具使用(函数调用) ](Tool%20Use%20%28Function%20Calling%29/readme.md)
- [Chapter 6: Planning 规划](Planning/readme.md)
- [Chapter 7: Multi-Agent Collaboration 多智能体协作](Multi-Agent%20Collaboration/readme.md)

**Part Two: Advanced Systems**

- [Chapter 8: Memory Management 记忆管理](Memory%20Management/readme.md)
- [Chapter 9: Learning and Adaptation 学习与适应](Learning%20and%20Adaptation/readme.md)
- [Chapter 10: Model Context Protocol (MCP) 模型上下文协议](Model%20Context%20Protocol%20%28MCP%29/readme.md)
- [Chapter 11: Goal Setting and Monitoring 目标设定与监控](Goal%20Setting%20and%20Monitoring/readme.md)

**Part Three: Production Concerns**

- [Chapter 12: Exception Handling and Recovery 异常处理与恢复](Exception%20Handling%20and%20Recovery/readme.md)
- [Chapter 13: Human in the Loop 人在回路中](Human%20in%20the%20Loop/readme.md)
- [Chapter 14: Knowledge Retrieval (RAG) 知识检索](Knowledge%20Retrieval%20%28RAG%29/readme.md)

**Part Four: Multi-Agent Architectures**

- [Chapter 15: Inter-Agent Communication (A2A) 智能体间通信](Inter-Agent%20Communication%20%28A2A%29/readme.md)
- [Chapter 16: Resource-Aware Optimization 资源感知优化](Resource-Aware%20Optimization/readme.md)
- [Chapter 17: Reasoning Techniques 推理技术](Reasoning%20Techniques/readme.md)
- [Chapter 18: Guardrails and Safety Patterns 护栏与安全模式](Guardrails%20and%20Safety%20Patterns/readme.md)
- [Chapter 19: Evaluation and Monitoring 评估与监控](Evaluation%20and%20Monitoring/readme.md)
- [Chapter 20: Prioritization 优先级排序](Prioritization/readme.md)
- [Chapter 21: Exploration and Discovery 探索与发现](Exploration%20and%20Discovery/readme.md)

**Appendix**

- [Appendix A: Advanced Prompting Techniques 高级提示词技巧](Advanced%20Prompting%20Techniques/readme.md)
- [Appendix B: AI Agentic Interactions: From GUI to Real-World Environment 智能体交互：从图形界面到真实世界环境](AI%20Agentic%20Interactions%20-%20From%20GUI%20to%20Real-World%20Environment/readme.md)
- [Appendix C: Quick Overview of Agentic Frameworks 智能体设计与开发框架速览](Quick%20Overview%20of%20Agentic%20Frameworks/readme.md)
- [Appendix D: Building an Agent with AgentSpace (online only) 使用 AgentSpace 构建智能体(仅在线)](Building%20an%20Agent%20with%20AgentSpace%20%28online%20only%29/readme.md)
- [Appendix E - AI Agents on the CLI 命令行上的智能体](AI%20Agents%20on%20the%20CLI/readme.md)
- [Appendix F: Under the Hood: An Inside Look at the Agent's Reasoning Engines 幕后揭秘:智能体推理引擎内窥](Under%20the%20Hood%20-%20An%20Inside%20Look%20at%20the%20Agent%27s%20Reasoning%20Engines/readme.md)
- [Appendix G: Coding Agents 编程智能体](Coding%20Agents/readme.md)

### 共创者

**华中师范大学 2026 级人工智能专业全体研究生**（暂定）

欢迎通过 Issue 或 Pull Request 提出勘误、补充内容与改进建议。

### 致谢与声明

- 原著 *Agentic Design Patterns* 作者：**Antonio Gulli**、**Mauro Sauco**。
- 本项目为非官方、非商业性的学习与解读资料，与原著作者及出版方不存在官方关联。
- 原著内容及相关权利归其作者和权利人所有。本仓库中的解读内容仅用于学习、研究与交流；如有侵权，请联系我们处理。
- 引用或转载本项目内容时，请注明来源与共创者。

---

## English

### About This Project

This repository contains Chinese interpretation notes, concept explanations, and supporting study materials for *Agentic Design Patterns*. It aims to help readers systematically understand common design patterns, implementation approaches, and engineering practices for agentic systems.

The materials are collaboratively developed through coursework, study, and discussion. They are intended for personal learning, classroom discussion, and technical exchange.

### What Is Covered

- **Foundational patterns:** Prompt Chaining, Routing, Parallelization, Reflection, Tool Use, Planning, and Multi-Agent Collaboration;
- **Advanced systems:** Memory Management, Learning and Adaptation, Model Context Protocol (MCP), and Goal Setting and Monitoring;
- **Production concerns:** Exception Handling and Recovery, Human in the Loop, and Knowledge Retrieval (RAG);
- **Multi-agent architectures:** Agent-to-Agent Communication (A2A), Resource-Aware Optimization, Reasoning Techniques, Guardrails, Evaluation and Monitoring, Prioritization, and Exploration;
- **Additional topics:** Advanced Prompting, Agentic Interactions, Agent Frameworks, CLI Agents, Coding Agents, and more.

### Getting Started

- [Read the main materials](./Agentic%20Design%20Patterns.md)
- [View the Chinese PDF](./origin_pdf_resource/Agentic-Design-Patterns-CN.pdf)

You can read the Markdown file directly on GitHub or download the PDF for offline use.

### Project Contributors

**All graduate students in the 2026 cohort of the Artificial Intelligence program at Central China Normal University (CCNU)** *(tentative)*

Corrections, additions, and suggestions are welcome through Issues and Pull Requests.

### Acknowledgements and Disclaimer

- *Agentic Design Patterns* was written by **Antonio Gulli** and **Mauro Sauco**.
- This is an unofficial, non-commercial study and interpretation project. It is not affiliated with or endorsed by the original authors or publisher.
- All rights to the original work belong to their respective authors and rights holders. The materials in this repository are provided solely for learning, research, and discussion. Please contact us if any content raises copyright concerns.
- When quoting or redistributing materials from this project, please credit the source and the project contributors.

---

<p align="center">
  Made with curiosity, collaboration, and respect for knowledge.<br>
  因好奇而探索，因协作而完善，因尊重而分享。
</p>
