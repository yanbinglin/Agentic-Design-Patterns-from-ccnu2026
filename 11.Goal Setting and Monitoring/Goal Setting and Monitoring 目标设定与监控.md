# Goal Setting and Monitoring 目标设定与监控

> 来源：本目录 [Ch11_Goal_Setting_and_Monitoring_.pptx](Ch11_Goal_Setting_and_Monitoring_.pptx)，共 16 页。按原课件页序整理文字、表格与插图；保留代码文本及截图，未执行示例代码。下文为课件内容转录，版本、指标与技术判断沿用课件，未另行核验。

## 内容概览

目标设定为智能体提供明确的完成标准，监控则持续检查行为、环境与目标之间的偏差，形成评估和调整的闭环。课件从模式与六类场景切入，走读自主 AI 程序员的提示链与门禁机制，分析自写自审的局限，再讨论职责分离、SMART 准则与 Google ADK 的落地方式。

## 课件逐页内容

> 每节对应一页幻灯片。文字按课件对象顺序保留，同一文本框内保留换行；流程图中的连接关系请结合原 PPTX 阅读。

### 第 1 页：目标设定与监控

《智能体设计模式》

AGENTIC DESIGN PATTERNS · CHAPTER 11

第 11 章：目标设定与监控

Goal Setting and Monitoring —— 从“被动应答”到“主动闭环”的智能体跃迁

🎯 范式本质

明确目标 (SMART) + 持续进度追踪与动态重规划

🔁 核心图例

完整拆解书中 Fig.1 (AI 程序员闭环) 与 Fig.2 (目标模式)

🛠️ 思辨与实战

单智能体自评四大陷阱 → Gemini 五人小队 & ADK 落地

### 第 2 页：课堂教学路线图

课堂教学路线图

01

痛点破冰与模式全貌

旅行规划类比 (P151) · 书中图 2 (Goal Design Patterns) · 六大场景三元组矩阵 (P152)

02

架构深潜：自主 AI 程序员闭环

书中图 1 (Goal Setting & Monitor) · LangChain+GPT-4o 三步提示链与二元门禁源码精讲

03

课堂思辨：这段代码能上生产吗？

单智能体自评四大工程致命伤 · 职责分离的 Gemini 五人小队与四步审查法

04

工程落地与微实战：SMART × Google ADK

SMART 目标五维工程转化 · ADK instruction/state/LoopAgent 监控实现 · 60 秒重构实战

### 第 3 页：痛点破冰与模式全貌

01

MODULE 01 · PATTERN OVERVIEW & CORE MECHANISM

痛点破冰与模式全貌

为什么“会处理信息、会调工具”的智能体，在复杂任务中依然会迷失方向？

▸ 核心探究 1：规划 (Planning) 与 目标设定及监控 (Goal Setting & Monitoring) 的本质边界在哪？<br>
▸ 核心探究 2：如何用“目标—监控信号—纠偏动作”三元组公式，统领书中 6 大真实应用场景？

### 第 4 页：01 模式概览

01 模式概览

核心范式跃迁：从“静态反应”到“动态导航”

《Agentic Design Patterns》第 11 章：目标设定与监控 (P151–P159)  |  翻转课堂精讲

4 / 16

💡 核心命题 ：若要让 AI 智能体真正有效、有的放矢，仅有信息处理或工具调用能力并不足够；它必须拥有清晰的方向感（明确待达成的具体目标 Goal Setting），以及判断自身是否切实取得进展的手段（跟踪进度与完成度判定 Monitoring）。

🧭 书中类比拆解：规划一次旅行（Planning a Trip）的五个认知要素与闭环（P151）

1. 初始状态
Initial State

明确当前起点在哪
（现有数据、上下文与环境状态）

➔

2. 目标状态
Goal State

决定最终要去哪里
（明确、可检验的预期终局状态）

➔

3. 权衡约束与选项
Constraints

评估交通、路线、预算
（可用工具、Token/时间预算与红线）

➔

4. 拆解行动序列
Step Sequence

订票→打包→去车站→乘车→入住
（依赖有序的子目标与工具调用链）

➔

5. 持续监控与重规划
Monitor & Replan

遇航班取消或延误时动态改签
（对照目标检测偏差 → 实时修正计划）

📊 架构范式对比：传统反应式系统 vs. 目标导向与监控型智能体

| 对比维度 | 反应式/单步工具系统 (Reactive System) | 目标设定与监控型智能体 (Goal-Monitored Agent) |
| --- | --- | --- |
| 驱动方式 | 被动响应即时输入，执行单步或固定线性脚本 | 由高层目标拆解子目标，主动朝明确终局状态推进 |
| 过程感知 | 无进度概念，“调完即止”，盲目相信单次输出 | 持续观测行为、环境状态与工具输出，逐项核对清单 |
| 异常应对 | 遇到意外阻碍或输出跑偏时直接失败或错误传播 | 通过反馈回路自适应调整策略、重规划或升级人工 |
| 典型类比 | 纸质列车时刻表（静态死板，遇晚点即失效） | 车载实时 GPS 导航（偏航秒级感知，动态重算路线） |

🙋 课堂辨析

❓ 提问：第 6 章讲了“规划 (Planning)”，第 4 章讲了“反思 (Reflection)”，第 11 章的“目标设定与监控”跟它们是什么关系？<br>
🔑 核心洞察 1（目标即标尺）：Planning 负责把高层意图拆成步骤；而 Goal Setting 提供终极判分标尺（Benchmark）。<br>
🔑 核心洞察 2（监控+反思=闭环）：Monitoring 负责采集轨迹与状态偏差；Reflection 则作为纠偏引擎，根据监控信号修正下一步计划（见 P63 & P151）。

### 第 5 页：01 核心图例 ②

01 核心图例 ②

模式速览与核心架构（含书中图 2：Goal design patterns）

《Agentic Design Patterns》第 11 章：目标设定与监控 (P151–P159)  |  翻转课堂精讲

5 / 16

📖 书中原图例 2：Fig.2 Goal design patterns (P158)

![课件第 5 页原图 1](assets/pptx/slide-05-image-01.png)

🔍 图例 2 深度解读（如何读懂这张架构图？）：<br>
1. 目标显式外化：右侧靶心卡片（Goal 1 ~ Goal 5）表明目标不再是隐含意图，而是拆解为可逐项核对的显式指标清单。<br>
2. 持续对齐比对：Agent 在接收 User Prompt 后，每一步推理与行动都必须与右侧 Goal 清单比对。<br>
3. 双向闭环反馈：Output 不仅交付给 User，同时受控于目标达成度验证，未达标则在内部迭代修正。

❓ What（要解决什么痛点？| P157）

智能体常缺乏清晰方向，仅能做简单被动反应；没有明确目标就无法独立编排复杂多步工作流，也缺乏内在机制判断自身行动是否正通向成功。

💡 Why（标准化解法是什么？| P158）

为智能体注入“目的感”与“自评机制”：① 显式定义具体、可衡量的目标（SMART）；② 建立持续监控机制，实时追踪智能体进度与环境状态，偏航时主动纠偏。

📏 Rule of Thumb（何时采用本模式？| P158）

当 AI 智能体必须自主执行多步任务、适应动态环境条件，并在较少人工干预（Low Human Intervention）下可靠达成特定高层目标时，必用此模式。

⚙️ 监控器到底看什么？反馈回路怎么动？（P159 核心机制提炼）

观测信号 1

智能体行为
Agent Actions

观测信号 2

环境状态
Env States

观测信号 3

工具输出
Tool Outputs

➔

三大闭环反馈动作：<br>
① 适应 (Adapt)：微调当前执行参数<br>
② 修订 (Revise)：重排或改写后续计划<br>
③ 升级 (Escalate)：超界时移交人类专家

### 第 6 页：01 实践用例

01 实践用例

六大真实场景全景拆解：目标怎么设？监控看什么？

《Agentic Design Patterns》第 11 章：目标设定与监控 (P151–P159)  |  翻转课堂精讲

6 / 16

💡 教学提炼：将书中第 152 页的 6 个行业用例，统一拆解为【设定目标 Goal】×【监控信号 Signals】×【纠偏/升级动作 Action】三元组：

1. 客户支持自动化 (Customer Support)

🎯 设定目标：解决客户的账单咨询 (Resolve billing inquiry)<br>
📡 监控信号：跟踪对话流、核查数据库账单变更记录、收集客户正负向反馈<br>
⚡ 纠偏动作：调用账单调整工具；若多轮未解决或客户不满，立即触发升级 (Escalate) 转人工

2. 个性化学习系统 (Personalized Learning)

🎯 设定目标：提升学生对代数的理解与掌握度<br>
📡 监控信号：实时监控习题完成进度、答题准确率 (Accuracy) 与单题耗时<br>
⚡ 纠偏动作：若监测到学生卡壳或准确率下滑，动态调整教材难度与讲解策略

3. 项目管理助手 (Project Management)

🎯 设定目标：确保项目里程碑 X 在截止日期 Y 前按期交付<br>
📡 监控信号：持续监控看板任务状态、团队沟通记录与关键资源可用性<br>
⚡ 纠偏动作：一旦发现进度滞后风险，自动标出延误预警 (Flag delays) 并生成纠偏排期建议

4. 自动交易机器人 (Automated Trading Bots)

🎯 设定目标：在严格的风险承受范围内最大化投资组合收益<br>
📡 监控信号：毫秒级监控实时行情数据、当前持仓净值与风险敞口指标<br>
⚡ 纠偏动作：条件吻合时自动执行交易；一旦触及风险阈值立即调整策略或触发止损

5. 机器人与自动驾驶 (Robotics & Autonomous Vehicles)

🎯 设定目标：安全、高效地将乘客从 A 点运送至 B 点<br>
📡 监控信号：高频监控外部环境（他车/行人/信号灯）、自身状态（车速/电量）及路线进度<br>
⚡ 纠偏动作：根据路况动态调整加减速、变道或重新规划绕行路线，确保安全抵达

6. 内容审核系统 (Content Moderation)

🎯 设定目标：精准识别并移除平台 X 上的违规与有害内容<br>
📡 监控信号：监控实时流入内容，运行分类模型，持续跟踪误报率 (FP) 与漏报率 (FN)<br>
⚡ 纠偏动作：动态收紧或放宽过滤阈值；将边界模糊的疑难案例交由人工复核员终审

### 第 7 页：架构深潜：自主 AI 程序员案例

02

MODULE 02 · HANDS-ON CODE & ARCHITECTURE WALKTHROUGH

架构深潜：自主 AI 程序员案例

基于 LangChain + GPT-4o 的代码生成、自我审查与迭代精炼闭环

▸ 核心探究 1：书中图 1（Fig.1）的每个流程图节点，分别对应 Python 脚本里的哪个核心函数？<br>
▸ 核心探究 2：如何把一段自由文本的评审意见（Feedback），转化为确定性的布尔循环门禁（True/False）？

### 第 8 页：02 核心图例 ①

02 核心图例 ①

动手示例闭环拆解（含书中图 1：Goal Setting and Monitor example）

《Agentic Design Patterns》第 11 章：目标设定与监控 (P151–P159)  |  翻转课堂精讲

8 / 16

📖 书中图例 1：Fig.1 目标设定与监控示例

![课件第 8 页原图 2](assets/pptx/slide-08-image-02.png)

🔗 图例 1 与 Python 源码 的 1:1 架构映射走读（自主 AI 程序员）

Step 1 · User ➔ Prompt（项目简报与质量清单输入）

入口函数：`run_code_agent(use_case, goals_input, max_iterations=5)` —— 接收具体编程任务 `use_case` 与逗号分隔的目标清单 `goals`（如：简单易懂、功能正确、处理边界情况）。

Step 2 · Prompt ➔ Code（动态组装提示词与代码生成/修订）

核心函数：`generate_prompt(use_case, goals, previous_code, feedback)` + `clean_code_block()` —— 首轮生成初稿；后续迭代将上一版代码与评审意见注入 Prompt 进行定向重构，并剥离 Markdown 围栏。

Step 3 · Self-Review + Quality Checklist（对照目标清单严格自审）

审查函数：`get_code_feedback(code, goals)` —— 令 LLM 切换为 `Python code reviewer` 角色，对照 `goals` 逐项审视清晰度、简洁度、正确性、边界处理与测试覆盖度，返回结构化批评文本。

Step 4 · 菱形判断框 If Good（二元门禁裁决 True / False）

门禁函数：`goals_met(feedback_text, goals) -> bool` —— 令 `AI reviewer` 基于目标清单与评审文本裁决是否全部达标，强约束仅输出单字 `True` 或 `False`（`response == 'true'`）。

Step 5 · 循环分支：False 返工修订 vs. True 打包落盘交付

闭环控制：若为 `False` 且未满 5 轮则更新 `previous_code = code` 回环重写；若为 `True`（或触顶 5 轮），调用 `add_comment_header()` 与 `save_code_to_file()` 生成 `{short_name}_{4位随机数}.py` 落盘。

### 第 9 页：02 源码精讲

02 源码精讲

核心代码走读：三阶提示链与二元门禁机制

《Agentic Design Patterns》第 11 章：目标设定与监控 (P151–P159)  |  翻转课堂精讲

9 / 16

阶段 ①：迭代式生成提示组装

generate_prompt() | P153

```text
def generate_prompt(use_case, goals,
    previous_code="", feedback=""):
  base_prompt = f"""
You are an AI coding agent.
Use Case: {use_case}
Your goals are:
{chr(10).join(f'- {g}' for g in goals)}"""
  if previous_code:
    base_prompt += f"\nPreviously generated code:\n{previous_code}"
  if feedback:
    base_prompt += f"\nFeedback on previous version:\n{feedback}"
  base_prompt += "\nPlease return only the revised Python code."
  return base_prompt
```

💡 工程亮点：增量状态注入
不堆砌冗余历史对话，仅将最新一轮的 `previous_code` 与针对性 `feedback` 显式拼入 Prompt，实现高信噪比的自我修正。

阶段 ②：质量清单对照审查

get_code_feedback() | P153-154

```text
def get_code_feedback(code, goals):
  feedback_prompt = f"""
You are a Python code reviewer.
Based on the following goals:
{chr(10).join(f'- {g}' for g in goals)}
Please critique this code and identify
if the goals are met. Mention if
improvements are needed for clarity,
simplicity, correctness, edge case
handling, or test coverage.
Code:
{code}"""
  return llm.invoke(feedback_prompt)
```

💡 工程亮点：显式评估维度锚定
不仅传入用户自定义 `goals`，还在提示词中硬编码了五大工程复核维度（清晰、简洁、正确、边界、测试），防止笼统敷衍。

阶段 ③：二元门禁解耦裁决

goals_met() | P154

```text
def goals_met(feedback_text, goals) -> bool:
  review_prompt = f"""
You are an AI reviewer.
Here are the goals:
{chr(10).join(f'- {g}' for g in goals)}
Here is the feedback on the code:
\"\"\"{feedback_text}\"\"\"
Based on the feedback above,
have the goals been met?
Respond with only one word:
True or False."""
  response = llm.invoke(review_prompt)
  return response.content.strip().lower() == "true"
```

💡 工程亮点：CoT 批评与布尔判断分离
先在阶段 ② 充分展开文字批评，再在阶段 ③ 将自然语言评审归约为单字 `True/False`，使 Python `if` 分支具备确定性控制力。

🧪 书中 P155 的三个 CLI 实战测试案例（为什么 Example 2 的 Goal 写得如此耐人寻味？）：<br>
• Example 1 (求正整数 BinaryGap)：Goals = "Code simple to understand, Functionally correct, Handles comprehensive edge cases, Takes positive integer input only..."<br>
• Example 2 (递归统计目录文件总数)：Goals = "...Handles comprehensive edge cases, Ignore recommendations for performance, Ignore recommendations for test suite use like unittest or pytest"  👈 【思考：作者为何被迫加这两条“免死金牌”？】<br>
• Example 3 (命令行读取 Word .doc/.docx 统计词数与字符数)：Goals = "Code simple to understand, Functionally correct, Handles edge cases"

### 第 10 页：批判思辨：这段代码能上生产吗？

03

MODULE 03 · CRITICAL THINKING & MULTI-AGENT EVOLUTION

批判思辨：这段代码能上生产吗？

从“单智能体自评陷阱”到“职责分离的 Gemini 五人智能体小队”

▸ 核心探究 1：为什么作者在 P156 明确警告“本例仅供演示，不宜直接用于生产”？<br>
▸ 核心探究 2：如何用职责分离的多智能体小队（Crew）和四步审查法解决“既当运动员又当裁判员”的困局？

### 第 11 页：03 课堂思辨

03 课堂思辨

单智能体“自写自审”的四大工程致命伤

《Agentic Design Patterns》第 11 章：目标设定与监控 (P151–P159)  |  翻转课堂精讲

11 / 16

💬 假设你是技术总监，实习生把 P153 代码直接推上生产环境，你会指出哪些隐患？

📖 原文警示 (P156 Caveats and Considerations): "It is important to note that this is an exemplary illustration and not production-ready code."

致命伤 ①：同模自评偏见（认知盲区共享）

Same LLM as Writer & Judge | P156-157

• 现象：同一个 `gpt-4o` 实例既负责写代码，又负责挑错和判 `True`（既当运动员又当裁判员）。
• 根因：生成模型与评审模型共享完全相同的权重与推理盲区；写代码时没意识到的逻辑漏洞，自审时同样极难察觉。
• 工程解药：拆分为独立角色或采用异构模型交叉评审（Producer-Critic 分离）。

致命伤 ②：目标曲解与“幻觉式达标”

Goal Misinterpretation & Hallucinated True | P156

• 现象：LLM 未能真正理解目标的业务内涵，或产生幻觉（Hallucination），在代码仍有缺陷时自信满满地返回 `True`。
• 根因：纯靠 LLM 主观文本判断“是否处理了所有边界”，缺乏客观硬性标尺。
• 工程解药：将主观自然语言目标转化为可量化的形式化契约（Pydantic 校验 / 断言测试）。

致命伤 ③：纸上谈兵，零沙箱运行验证

Zero Code Execution & No Unit Tests | P157

• 现象：生成的代码从未被解释器真正执行过！哪怕存在 `SyntaxError` 或死循环，只要外观工整就可能通过文本审查。
• 原文金句："LLMs do not produce flawless code by magic; you still need to run and test the produced code."
• 工程解药：引入沙箱代码执行工具（如 ADK `BuiltInCodeExecutor`）与自动化单元测试智能体。

致命伤 ④：苛刻挑刺引发死循环与成本失控

Basic Monitoring & Infinite Loop Risk | P157

• 现象：扮演 Reviewer 的模型往往“为了挑刺而挑刺”（不断要求加单测、优性能），导致 `goals_met` 持续为 `False`。
• 揭秘 Example 2：这就是为何作者在 P155 被迫写上 `Ignore recommendations for performance/unittest`！若无 `max_iterations` 硬兜底，进程将无限空转烧光预算。

### 第 12 页：03 架构演进

03 架构演进

生产级演进方案：职责分离的 Gemini 五人智能体小队

《Agentic Design Patterns》第 11 章：目标设定与监控 (P151–P159)  |  翻转课堂精讲

12 / 16

🚀 书中 P157 进阶方案：将“编码、审查、测试、文档、提示调优”职责彻底分离，组建 Gemini 专职智能体小队 (Multi-Agent Crew)：

1. 同伴程序员
Peer Programmer

💻 编码生成

专注头脑风暴与核心功能代码编写，不兼任裁判

2. 代码审查员
Code Reviewer

🔍 独立裁判

独立实体审阅代码，揪出逻辑漏洞并给出重构对比

3. 测试编写者
Test Writer

🧪 实测验证

编写全面单元测试，用客观测试通过率取代主观臆断

4. 文档撰写者
Documenter

📄 规范文档

生成清晰简洁的函数说明、API 文档与使用示例

5. 提示优化者
Prompt Refiner

✨ 交互调优

持续精炼与 AI 交互的指令结构，防止目标漂移

📜 书中 P157 工业级提示词范本：如何写好一个消除“代码幻觉”的独立 Code Reviewer？

"Act as an expert code reviewer with a deep commitment
to producing clean, correct, and simple code. Your core
mission is to eliminate code 'hallucinations' by ensuring
every suggestion is grounded in reality and best practices.
When I provide you with a code snippet, I want you to:
 -- Identify and Correct Errors: Point out any logical
    flaws, bugs, or potential runtime errors.
 -- Simplify and Refactor: Suggest changes that make the
    code more readable, efficient, and maintainable...
 -- Provide Clear Explanations: Reference principles of
    clean code, performance, or security.
 -- Offer Corrected Code: Show the 'before' and 'after'..."

1. 查错纠错 (Identify & Correct Errors)：精准定位逻辑缺陷、潜在 Bug 与运行时异常，杜绝“带病放行”。

2. 简化重构 (Simplify & Refactor)：在不牺牲正确性的前提下，提升代码可读性、执行效率与可维护性。

3. 阐明原理 (Provide Clear Explanations)：每条建议必须援引 Clean Code、性能或安全原则解释“为什么改”，拒绝主观拍脑袋。

4. 前后对比 (Offer 'Before' & 'After' Code)：显式给出修改前后的代码 Diff 对照，让生产者智能体或人类开发者零歧义合入。

### 第 13 页：工程落地与课堂微实战

04

MODULE 04 · SMART CRITERIA, GOOGLE ADK & LIVE WORKSHOP

工程落地与课堂微实战

SMART 目标设计法则 × Google ADK 状态监控机制 × 重构实战 (P158–159)

▸ 核心探究 1：如何把管理学中的 SMART 原则，转化为智能体系统中的 Prompt 契约与停止条件？<br>
▸ 核心探究 2：在 Google ADK 框架中，如何用 instruction、session.state 与 LoopAgent 实现目标监控？

### 第 14 页：04 落地与实战

04 落地与实战

黄金法则与框架落地：SMART 准则 × Google ADK 实现

《Agentic Design Patterns》第 11 章：目标设定与监控 (P151–P159)  |  翻转课堂精讲

14 / 16

📏 智能体目标的 SMART 五维工程转化表 (P159)

S - Specific
具体明确

明确规定输入契约、输出格式（如 JSON/Python）与边界条件，拒绝含糊指令。

M - Measurable
可量化衡量

定义可由代码或独立裁判检验的二元/数值标准（如单测通过率 100%、`goals_met==True`）。

A - Achievable
能力可达成

目标必须在底层 LLM 推理上限与已挂载工具（Tools/Permissions）的能力包络线内。

R - Relevant
业务强相关

子目标严格对齐顶层意图，剔除无关加戏（如显式声明忽略非必要的性能/框架扩展）。

T - Time-bound
有时限/轮数兜底

必须配置 `max_iterations` 最大迭代轮数、超时阈值（Timeout）与 Token 预算红线！

🛠️ Google ADK 中的目标传递与状态监控实现 (P159)

1. 目标设定 (Goal Conveyance)：通过 `LlmAgent(instruction=..., output_key=...)` 将明确目标与输出契约下发给专职智能体。<br>
2. 状态监控 (State & Tool Monitoring)：在工具函数内通过 `tool_context.state` 实时记录进度指标（如 `state['status'] = 'completed'`）。<br>
3. 循环门禁 (LoopAgent & Escalation)：用 `LoopAgent(max_iterations=10)` 串联执行步与 `ConditionChecker`，达标时触发 `EventActions(escalate=True)` 终止循环。

```text
# Google ADK 目标监控闭环核心模式 (P159 结合 P106/P116)
class GoalChecker(BaseAgent):
  async def _run_async_impl(self, ctx: InvocationContext):
    if ctx.session.state.get("goals_met") == True:
      yield Event(author=self.name, actions=EventActions(escalate=True))

monitored_agent = LoopAgent(
  name="GoalMonitoredLoop", max_iterations=5,
  sub_agents=[coder_agent, reviewer_agent, GoalChecker()]
)
```

⚡ 微实战：如何为我们教育学部设计一个“翻转课堂课前预习诊断智能体”？

❌ 反面设计（无监控、易死循环）：<br>
Prompt：“你是一个 AI 助教，请辅导学生预习第 11 章内容，直到学生完全掌握为止。”<br>
缺陷诊断：什么是“完全掌握”？（不可衡量）；学生一直答不上来怎么办？（无轮数上限与升级机制）。

✅ SMART + 监控闭环重构版（可直接落地 ADK）：<br>
• [S/M 具体可衡量目标]：围绕第 11 章生成 3 道概念辨析题；当 `state['correct_count'] >= 2` 且学生能简述图 1 闭环时，判定 `master = True`。<br>
• [Monitoring 监控信号]：观测学生每轮回答（Env State）与答题评分工具输出（Tool Output），实时更新薄弱点列表 `state['weak_points']`。<br>
• [T 时限兜底与升级]：设置 `max_iterations = 3` 轮苏格拉底式追问；若 3 轮后仍未达标，自动生成学情诊断卡并升级（Escalate）推送给任课教师。

### 第 15 页：📌 第 11 章《目标设定与监控》课堂核心要点回顾

📌 第 11 章《目标设定与监控》课堂核心要点回顾

1. 目标即标尺 (SMART)

将模糊意图拆解为具体、可量化、有时限的检查点清单（见图 2）

2. 监控三信号与闭环

持续观测行为、环境状态与工具输出，驱动适应、重规划或升级（见图 1）

3. 生产级多智能体演进

打破单智能体同模自评盲区，以职责分离小队 + 沙箱实测保障可靠交付

“赋予智能体设定目标与审视自身进展的能力，是迈向更可信、更可问责的 AI 系统的基石之一。”

— Antonio Gulli, 《Agentic Design Patterns》Chapter 11 Conclusion (P159)

感谢聆听

🔗 延伸思考（衔接第 12 章《异常处理与恢复》）：当监控发现外部工具报错 500 或网络中断时，智能体该如何优雅降级与回滚状态？

### 第 16 页：05 团队分工

05 团队分工

小组成员与分工

余

余可

主讲人<br>
PPT 制作

李

李航

协助 PPT 制作<br>
章节理解

徐

徐家伟

代码实践<br>
实例提供

席

席宇昂

思路灵感提供<br>
协助 PPT 制作

《Agentic Design Patterns》第 11 章：目标设定与监控 (P151–P159)  |  翻转课堂精讲

16 / 16

## 小组成员与分工

> 依据课件中的目录、署名与分工页整理；按课件分工顺序列出。

- 余可：主讲人，PPT 制作

- 李航：协助 PPT 制作，章节理解

- 徐家伟：代码实践，实例提供

- 席宇昂：思路灵感提供，协助 PPT 制作
