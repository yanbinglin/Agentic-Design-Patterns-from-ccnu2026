# Resource-Aware Optimization

资源感知优化
## 双语课堂汇报内容稿（供修改）

> 当前交付：章节结构、逐页内容、代码片段及备注；不制作 PPT。
> 建议规模：正文 26 页，附录 3 页；正文约 25–30 分钟，可按课程时长删并。

## 编写与呈现约定（不上屏）

1. **英文优先**：先确定英文概念，再逐项给出贴近英文的中文译文。英文稿为忠实压缩和改写，不冒充原文逐字引述。
2. **双语对应**：每页标题与正文均英文在前、中文在后。每句英文下一行紧跟对应中文，不标注语言标签，不按语言分块；代码页解释也采用相同方式。
3. **代码例外**：代码中的注释、自然语言提示词、角色描述、测试问题和打印信息全部使用中文。变量名、模型 ID、API 字段、分类标签和协议值保留原样，避免破坏含义或接口。
4. **示例边界**：沿用原文中的模型名称解释架构，不声称这些模型当前最优、最便宜或仍可调用。本稿不提供实时价格、排名或当前 API 可用性结论。
5. **教学节奏**：正文只保留短代码与核心决策；配置、完整质检提示和实现问题放附录。所有代码均为未执行的教学节选或伪代码，具体类别逐段注明。
6. **图示文字**：未来图中的节点、箭头说明也采用英文／中文对应；不将教学扩展图标为原书图。

## 章节安排（不上屏）

| Section<br>节 | Slides<br>页码 | Focus<br>核心问题 | 建议用时 |
| --- | --- | --- | --- |
| 1. Motivation and Definition<br>动机与定义 | 01–05 | What is being optimized?<br>优化什么？ | 4 分钟 |
| 2. Architecture and Routing<br>架构与路由 | 06–10 | How are execution paths selected?<br>如何选择执行路径？ | 5 分钟 |
| 3. Critique and Feedback<br>评议与反馈 | 11–13 | How does evaluation improve allocation?<br>评估如何改进分配？ | 3 分钟 |
| 4. Code Walkthrough<br>代码讲解 | 14–18 | How does the example work?<br>示例如何运作？ | 6 分钟 |
| 5. Optimization Beyond Routing<br>路由之外的优化 | 19–22 | What else can be optimized?<br>还有什么可优化？ | 4 分钟 |
| 6. Evaluation and Discussion<br>评估与讨论 | 23–26 | How do we judge the result?<br>如何判断结果？ | 4 分钟 |
| Appendix<br>附录 | A1–A3 | 配置、提示与代码边界 | 按需 |

---

# Section 1. Motivation and Definition

动机与定义

## Slide 01. Resource-Aware Optimization

资源感知优化

Chapter 16 · Agentic Design Patterns  
第 16 章 · 智能体设计模式

Managing computational, temporal, and financial resources during agent operation.  
在智能体运行期间管理计算、时间与资金资源。

**备注**：标题页只保留上述信息，汇报人及课程信息待补。


## Slide 02. One Task, Different Resource Constraints

同一任务，不同资源约束

- A financial analyst needs a preliminary report immediately: use a faster, more affordable model to summarize key trends.  
   金融分析师立即需要初步报告：使用更快、更经济的模型概括主要趋势。

- A critical forecast allows more time and a larger budget: allocate more resources to a more capable predictive model.  
   一项关键预测允许更多时间和更高预算：为能力更强的预测模型分配更多资源。

- The execution choice depends on the required output and the available resources.  
   执行方式取决于所需输出和可用资源。

**备注**：延续原文金融分析例子，但不暗示“更贵必然更准确”，也不将它讲成投资建议。


## Slide 03. Planning and Resource-Aware Execution

规划与资源感知执行

- Planning focuses primarily on the sequence of actions.  
  规划主要关注行动顺序。

- Resource-aware optimization decides how actions should be executed under resource constraints.  
  资源感知优化决定如何在资源约束下执行行动。

- Decisions include model choice, compute allocation, response depth, and fallback paths.  
  决策包括模型选择、计算资源分配、回答深度和回退路径。


A correct sequence still needs an affordable and timely execution path.  
正确的行动顺序仍需要费用可承受、时间可接受的执行路径。

**备注**：用同一个旅行计划的“先查交通、再选住宿”解释顺序，用“谁来查、允许花多少时间”解释执行选择。


## Slide 04. Resources and Output Quality

资源与输出质量

| Resource<br>资源 | What is managed?<br>管理对象 |
| --- | --- |
| Computational<br>计算 | Processing capacity and compute allocation.<br>处理能力与计算资源分配。 |
| Temporal<br>时间 | Execution time and response latency.<br>执行时间与响应时延。 |
| Financial<br>资金 | Model, tool, and API spending.<br>模型、工具与 API 费用。 |

The central trade-off is between output quality and the resources required to produce it.  
核心权衡存在于输出质量与产生该输出所需的资源之间。

**备注**：质量不是要被无条件牺牲的变量。后续补充用质量下限、预算上限、时延上限共同判断可行方案。


## Slide 05. Practical Applications

实际应用

- Cost-optimized LLM usage: match model capacity to task needs and budget.  
  成本优化的 LLM 使用：使模型能力与任务需求和预算相匹配。

- Latency-sensitive operations: choose a timely reasoning path.  
  时延敏感的操作：选择能够及时完成的推理路径。

- Energy efficiency: conserve power on constrained devices.  
  能源效率：在资源受限设备上节约电力。

- Reliability fallback: use a backup when the primary model is unavailable.  
  可靠性回退：主模型不可用时使用备用模型。

- Data usage management: retrieve summaries rather than full datasets when appropriate.  
  数据使用管理：适当时检索摘要，而非完整数据集。

- Adaptive task allocation: assign tasks according to load and available time.  
  自适应任务分配：根据负载与可用时间分配任务。


**备注**：六条作为短列表展示，重点讲两条，其余点到为止。


---

# Section 2. Architecture and Routing

架构与路由

## Slide 06. The Resource-Aware Pattern

资源感知模式

- A user submits a prompt to an agent.  
   用户向智能体提交提示。

- The agent considers the resource budget while selecting and executing actions.  
   智能体在选择和执行行动时考虑资源预算。

- The output returns to the user, while resource information informs execution decisions.  
   输出返回用户，资源信息则为执行决策提供依据。

**原文图示**：

![Resource-Aware Optimization Design Pattern / 资源感知优化设计模式](assets/资源感知优化/file-20261014164444697.png)

**备注**：未来可保留原图，并在旁边补充双语图例：User／用户、Prompt／提示、Agent／智能体、Budget／预算、Output／输出。下一页再扩展为路由和评议架构，避免把扩展图说成原图。


## Slide 07. Router, Answering Agents, and Critique

路由器、回答智能体与评议

| Role<br>角色 | Responsibility<br>职责 |
| --- | --- |
| Router Agent<br>路由智能体 | Assess the request and select a model or tool path.<br>评估请求并选择模型或工具路径。 |
| Answering Agents<br>回答智能体 | Execute the selected task with different model capabilities.<br>使用不同的模型能力执行被选定的任务。 |
| Critique Agent<br>评议智能体 | Evaluate responses and provide feedback for refinement.<br>评估回答，并为改进提供反馈。 |

Different roles support modular design and more targeted optimization.  
不同角色支持模块化设计和更有针对性的优化。

**备注**：流程图为 Request／请求 → Router／路由器 → Answering Agent／回答智能体 → Critique／评议 → Output／输出；评议反馈指向回答与路由策略。图中箭头只是讲解结构，不表示每个请求都必须调用全部角色。


## Slide 08. A Hierarchical Travel Planner

分层旅行规划器

| Subtask<br>子任务 | Resource choice<br>资源选择 |
| --- | --- |
| Understand a complex request and construct a coherent itinerary.<br>理解复杂需求并构建连贯行程。 | A more capable model handles high-level planning.<br>能力更强的模型处理高层规划。 |
| Look up flight prices, hotel availability, and restaurant reviews.<br>查询航班价格、酒店空房与餐厅评价。 | A faster, lower-cost model handles routine tool calls.<br>更快、成本更低的模型处理常规工具调用。 |

Allocate resources at the subtask level because one workflow can contain different levels of difficulty.  
按子任务分配资源，因为同一工作流可以包含不同的难度层次。

**备注**：原文以 Gemini Pro 和 Flash 为例；这里先讲能力分工，再在代码页出现模型名称。搜索结果的复杂性或工具失败仍可能要求升级，不能把全部查询永远视为简单任务。


## Slide 09. From Length Rules to Semantic Routing

从长度规则到语义路由

- A basic router can use query length as a rough heuristic.  
   基础路由器可以将查询长度作为粗略启发式规则。

- A more capable router can analyze task meaning and reasoning requirements.  
   能力更强的路由器可以分析任务含义和推理需求。

- Prompt tuning and labeled query–model pairs can improve routing decisions.  
   提示调优与带标注的“查询—模型”样本可以改进路由决策。

**Teaching example**  
教学示例

“Prove that there are infinitely many primes.”

“证明素数有无穷多个。”

A short query can still require substantial reasoning.  
短查询仍然可能需要大量推理。

**备注**：原代码 `len(query.split()) < 20` 只是示例阈值，而且空格分词不能直接衡量中文长度。不要把它解释成可靠的复杂度判断器。


## Slide 10. Task Complexity Is Only One Input

任务复杂度只是一个输入

- Task signals: complexity, need for current information, and expected output quality.  
   任务信号：复杂度、对当前信息的需求以及预期输出质量。

- Resource signals: remaining budget, time allowance, and model availability.  
   资源信号：剩余预算、允许时间以及模型可用性。

- A feasible route must satisfy both task requirements and resource constraints.  
   可行路径必须同时满足任务要求与资源约束。

Classifying a task does not, by itself, enforce a budget.  
仅仅对任务分类，本身并不能执行预算约束。

**备注**：任务信息与资源信息两组输入汇入路由决策。强调原文的理念包含预算与时间，但后面的分类示例没有实现完整预算管理。


---

# Section 3. Critique and Feedback

评议与反馈

## Slide 11. What the Critique Agent Evaluates

评议智能体评估什么

- Factual correctness and consistency of reasoning.  
   事实正确性与推理一致性。

- Completeness, relevance, and balanced presentation.  
   完整性、相关性与平衡的表述。

- Missing information and specific opportunities for revision.  
   缺失信息以及具体的修订机会。

Reviewing both the original query and the answer helps determine whether the response addresses the actual request.  
同时审查原始查询和答案，有助于判断回答是否回应了实际请求。

**备注**：统一将 Critique Agent 译作“评议智能体”，强调建设性审查；与文档中的“批评智能体”指同一角色。LLM 评议是评估信号，不是事实正确性的保证。


## Slide 12. Feedback for Correction and Routing

用于纠错与路由的反馈

| Feedback use<br>反馈用途 | Example<br>示例 |
| --- | --- |
| Self-correction<br>自我纠错 | Identify a reasoning gap and revise the answer.<br>识别推理缺口并修订答案。 |
| Performance monitoring<br>性能监控 | Track accuracy and relevance across requests.<br>跨请求跟踪准确性与相关性。 |
| Routing refinement<br>路由改进 | Reconsider routes that repeatedly produce inadequate answers.<br>重新考虑反复产生不充分回答的路径。 |

The Critique Agent supports resource optimization indirectly by revealing poor allocation choices.  
评议智能体通过暴露不良分配选择，间接支持资源优化。

**备注**：简单题分给高成本模型可形成资源浪费，复杂题分给能力不足的模型可导致质量下降。仅凭答案正确无法判断是否浪费，还需查看路由记录和成本。


## Slide 13. Evaluation Also Consumes Resources

评估也消耗资源

- Routing, critique, and retries add cost and latency.  
   路由、评议与重试都会增加费用和时延。

- Review effort should match the expected benefit and the available budget.  
   审查投入应与预期收益和可用预算相匹配。

- Limit retries and define when to accept, escalate, or stop.  
   限制重试次数，并定义何时接受、升级或停止。

**Teaching example**  
教学示例

Reviewing every trivial answer with a costly model may consume more resources than it saves.  
使用高成本模型审查每一个简单答案，可能比其节省的资源消耗更多。


---

# Section 4. Code Walkthrough

代码讲解

## Slide 14. Three Query Categories

三类查询

| Label<br>标签 | Definition<br>定义 |
| --- | --- |
| `simple` | A direct question requiring neither complex reasoning nor current information.<br>不需要复杂推理或当前信息的直接问题。 |
| `reasoning` | A question requiring logic, calculation, or multiple inference steps.<br>需要逻辑、计算或多步推断的问题。 |
| `internet_search` | A question requiring current information or external evidence.<br>需要当前信息或外部证据的问题。 |

The source example maps these categories to different models and an optional search step.  
原文示例将这些类别映射到不同模型，并按需加入搜索步骤。

**备注**：统一代码标签为 `internet_search`，修正原文列表中的 `internetsearch`。这些类别是示例中的简化：真实请求可能同时需要搜索和推理。


## Slide 15. The Classifier Prompt

分类器提示词

The classifier returns a machine-readable route label rather than answering the question.  
分类器返回机器可读的路由标签，而不是回答问题。

```python
# 教学节选：供分类模型使用的中文系统提示词
CLASSIFIER_PROMPT = """
你是查询分类器，只负责分类，不回答用户的问题。
请从以下标签中选择一个：simple、reasoning、internet_search。

分类规则：
- simple：直接事实问题，不需要复杂推理或当前信息。
- reasoning：逻辑、数学或多步推断问题。
- internet_search：依赖当前事件、近期数据或外部资料的问题。

只返回 JSON，例如：{"classification": "simple"}。
"""
```

The application must validate the returned JSON and the category before using it.  
应用必须验证返回的 JSON 与类别，然后才能使用它。

**备注**：本页先解释输入输出契约，不展示 SDK 连接细节。JSON 校验是教学补充；提示中要求 JSON 不代表输出必然有效。


## Slide 16. The Routing Decision

路由决策

- Simple queries use the lower-cost path in the source example.  
   简单查询使用原文示例中成本较低的路径。

- Reasoning queries use a reasoning-oriented path.  
   推理查询使用面向推理的路径。

- Search queries retrieve evidence before answer generation.  
   搜索查询在生成答案之前检索证据。

```python
# 教学节选：沿用原文的模型映射，不代表当前选型建议
MODEL_BY_CATEGORY = {
    "simple": "gpt-4o-mini",
    "reasoning": "o4-mini",
    "internet_search": "gpt-4o",
}

def select_route(classification):
    # 新增：拒绝无法识别的标签，避免隐式落入高成本路径
    if classification not in MODEL_BY_CATEGORY:
        raise ValueError("分类结果无效，需要受控处理")
    model = MODEL_BY_CATEGORY[classification]
    needs_search = classification == "internet_search"
    return model, needs_search
```

**备注**：这里只实现标签到执行路径的映射，没有实现预算计量，也不对三款模型作当前能力或价格排名。


## Slide 17. Search Context and Answer Generation

搜索上下文与答案生成

Search results become context for the selected model. The user question remains part of the request.  
搜索结果成为所选模型的上下文。用户问题仍是请求的一部分。

```python
# 教学节选：将搜索结果整理为中文提示词
# results 为搜索步骤返回的列表，query 为用户问题
search_context = "\n\n".join(
    f"标题：{item['title']}\n"
    f"摘要：{item['snippet']}\n链接：{item['link']}"
    for item in results
)

full_prompt = (
    "请结合以下网页检索结果回答用户问题，并标明依据的链接。\n"
    "若结果不足以支持结论，请明确说明信息不足。\n"
    f"检索结果：\n{search_context}\n\n用户问题：{query}"
)
```

A search step adds resource use; retrieved snippets do not guarantee sufficient evidence.  
搜索步骤会增加资源消耗；检索到的摘要并不保证证据充分。

**备注**：原文默认搜索结果数为 1，仅能演示链路。本页增加来源链接和信息不足说明，不能把搜索结果直接等同于已验证事实。


## Slide 18. An Explicit Resource Check

显式资源检查

A more complete policy checks whether a candidate route is feasible before execution.  
更完整的策略会在执行之前检查候选路径是否可行。

```python
# 教学伪代码：演示决策逻辑，不是特定框架的接口
# 成本与时延均为估计值，不是绝对保证

def choose_feasible_route(candidates, budget, deadline, min_quality):
    feasible = [
        route for route in candidates
        if route.estimated_cost <= budget
        and route.estimated_latency <= deadline
        and route.estimated_quality >= min_quality
    ]
    if not feasible:
        return None  # 无可行路径时，交由上层停止或协商目标
    return min(feasible, key=lambda route: route.estimated_cost)
```

Estimated feasibility still requires runtime accounting and a controlled response to overruns.  
估计上的可行性仍需要运行时计量，以及对超限情况的受控响应。

**备注**：质量估计来自评测或历史数据，本段没有实现估计器。剩余预算要扣除已发生的分类、搜索和评议费用；无可行路径时不能默认无限追加调用。


---

# Section 5. Optimization Beyond Routing

路由之外的优化

## Slide 19. Automatic Selection and Sequential Fallback

自动选择与顺序回退

| Mechanism<br>机制 | Description<br>说明 |
| --- | --- |
| Automatic selection<br>自动选择 | Select a model based on the request.<br>根据请求选择模型。 |
| Sequential fallback<br>顺序回退 | Try an ordered list when a previous choice fails.<br>前一个选择失败时，尝试有序列表中的后续模型。 |

```python
# 原文参数结构的教学节选，不是完整请求
# 自动选择：由服务选择模型
selection_payload = {"model": "openrouter/auto"}

# 顺序回退：以下模型标识沿用原文示例
fallback_payload = {
    "models": [
        "anthropic/claude-3.5-sonnet",
        "gryphe/mythomax-l2-13b",
    ]
}
```

Selection addresses suitability; fallback addresses continuity after failure.  
选择处理适配性问题；回退处理失败后的连续性问题。

**备注**：参数用于解释原文，不声称当前服务行为或模型支持情况。回退列表不必然按价格排序，也不保证回答质量不变。原文的费用描述不能推广成“所有失败尝试都免费”。


## Slide 20. Tools, Context, and Resource Prediction

工具、上下文与资源预测

| Mechanism<br>机制 | Description<br>说明 |
| --- | --- |
| Adaptive Tool Use & Selection<br>自适应工具使用与选择 | Choose tools with regard to API cost, latency, and execution time.<br>考虑 API 费用、时延与执行时间来选择工具。 |
| Contextual Pruning & Summarization<br>上下文剪枝与摘要 | Retain relevant information and reduce unnecessary prompt tokens.<br>保留相关信息并减少不必要的提示 token。 |
| Proactive Resource Prediction<br>主动资源预测 | Forecast workloads and resource demand before bottlenecks occur.<br>在瓶颈出现之前预测工作负载与资源需求。 |

Resource decisions apply to the whole execution path, including information processing.  
资源决策适用于整个执行路径，包括信息处理。

**备注**：摘要也消耗资源，且可能丢失关键条件。可用“重复全量发送历史”与“保留相关片段”的对照解释，避免将剪枝讲成无代价压缩。


## Slide 21. Communication, Energy, and Parallelism

通信、能源与并行

| Mechanism<br>机制 | Description<br>说明 |
| --- | --- |
| Cost-Sensitive Exploration<br>成本敏感探索 | Account for communication costs as agents collaborate and share information.<br>在智能体协作与共享信息时计入通信成本。 |
| Energy-Efficient Deployment<br>节能部署 | Reduce energy use in resource-constrained environments.<br>减少资源受限环境中的能源消耗。 |
| Parallelization & Distributed Computing Awareness<br>并行化与分布式计算感知 | Distribute work to improve throughput and completion time.<br>分发工作以改善吞吐量与完成时间。 |

Lower latency from parallel execution can come with greater resource use.  
并行执行带来的较低时延，可能伴随更大的资源消耗。

**备注**：保留英文原文“Cost-Sensitive Exploration”，不直接改名为“成本敏感协同”。原文解释涉及通信成本，因此按此展开。并行不能被无条件等同于省钱。


## Slide 22. Learned Policies and Graceful Degradation

学习型策略与优雅降级

- Learned allocation policies adapt resource choices using feedback and performance metrics.  
   学习型分配策略利用反馈与性能指标调整资源选择。

- Graceful degradation preserves essential functionality under severe constraints or failures.  
   优雅降级在严重约束或故障下保留基本功能。

- Critical-task prioritization directs limited resources toward essential work.  
   关键任务优先处理将有限资源分配给必要工作。

**Teaching example**  
教学示例

If the preferred route is unavailable, provide a supported partial answer and state what could not be completed.  
如果首选路径不可用，提供有依据的部分答案，并说明未能完成的内容。

**备注**：不把降级解释成可以随意降低事实标准；无法满足任务最低要求时，应说明限制。关键任务优先处理出现在英文 Key Takeaways 中，虽未在前面的九项技术列表单列，仍予保留。


---

# Section 6. Evaluation and Discussion

评估与讨论

## Slide 23. A Budget-Constrained Routing Example

预算约束下的路由示例

Assume a minimum quality score of 0.85, a 2-second latency limit, and a budget of 3 hypothetical cost units.  
假设最低质量得分为 0.85、时延上限为 2 秒、预算为 3 个假设费用单位。

| Route<br>路径 | Estimated quality<br>估计质量 | Estimated latency<br>估计时延 | Estimated cost<br>估计费用 |
| --- | --- | --- | --- |
| A: lightweight<br>轻量 | 0.82 | 0.8 s | 1 unit<br>单位 |
| B: intermediate<br>中等 | 0.90 | 1.6 s | 2 units<br>单位 |
| C: high-capacity<br>高能力 | 0.96 | 3.0 s | 5 units<br>单位 |

Route B is the only feasible choice. Route A misses the quality threshold; route C exceeds both time and budget limits.  
路径 B 是唯一可行选择。路径 A 未达到质量阈值；路径 C 同时超出时间与预算限制。

**备注**：全部数字均为教学假设，不对应真实模型性能或价格。此表假定已从总预算、总时间中预留或扣除了路由等其他开销；否则不能直接宣布端到端可行。质量评分需先定义评测方法。


## Slide 24. Measure the Entire Request

度量整个请求

- Quality: task success, correctness, and relevance.  
   质量：任务成功情况、正确性与相关性。

- Resources: total cost, end-to-end latency, and resource use.  
   资源：总费用、端到端时延与资源使用量。

- Reliability: completion rate, fallback use, and constraint violations.  
   可靠性：完成率、回退使用情况与约束违反情况。

**Accounting example**  
计量示例

```text
Total cost = routing + answering + tools + critique + retries
总费用 = 路由 + 回答 + 工具 + 评议 + 重试
```

Compare a fixed-model baseline with a routed system on the same task set and report both quality and resource results.  
在相同任务集上比较固定模型基线与路由系统，同时报告质量和资源结果。

**备注**：原文支持准确性、相关性和资源权衡，本页补充完整验证方法。不虚构“节省百分比”或未进行的实验结果。


## Slide 25. When to Use This Pattern

何时使用此模式

- API or compute spending must stay within a strict budget.  
   API 或计算支出必须保持在严格预算内。

- Response time is critical to the application.  
   响应时间对应用至关重要。

- Hardware has limited compute capacity or battery life.  
   硬件的计算能力或电池续航有限。

- Different workflow steps require different resources.  
   工作流中的不同步骤需要不同资源。

**Discussion**  
讨论

When does the overhead of a router exceed the resources it saves?  
路由器的额外开销在何种情况下会超过它节省的资源？

**备注**：引导同学考虑任务同质、请求很简单、路由器很贵或误路由导致重试等情况，不预设所有系统都必须采用多智能体路由。


## Slide 26. Key Takeaways

核心要点

1. Resource-aware optimization dynamically manages compute, time, and money.  
   资源感知优化动态管理计算、时间与资金。

2. Routing matches requests to models or tools under task and resource constraints.  
   路由在任务与资源约束下，使请求与模型或工具相匹配。

3. Critique provides feedback for correction, monitoring, and routing refinement.  
   评议为纠错、监控和路由改进提供反馈。

4. Optimization also includes tools, context, communication, deployment, and fallback.  
   优化还包括工具、上下文、通信、部署与回退。

5. The result must be judged through output quality and total resource use together.  
   必须结合输出质量与总资源使用量判断结果。

Effective agents need execution choices that fit real-world constraints.  
有效的智能体需要符合真实世界约束的执行选择。


---

# Appendix

附录

## A1. ADK Agent Configuration

ADK 智能体配置

Separate agent configurations make model roles explicit. The source uses Gemini Pro and Flash as illustrative backends.  
独立的智能体配置使模型职责明确。原文以 Gemini Pro 和 Flash 作为示例后端。

```python
# 原文配置示意的中文化节选：未经执行验证
from google.adk.agents import Agent

# 能力较强的模型负责复杂问题
pro_agent = Agent(
    name="GeminiProAgent",
    model="gemini-2.5-pro",
    description="处理复杂查询的高能力智能体。",
    instruction="你是擅长解决复杂问题的专家助手。",
)

# 轻量模型负责直接问题
flash_agent = Agent(
    name="GeminiFlashAgent",
    model="gemini-2.5-flash",
    description="处理简单查询的快速高效智能体。",
    instruction="你是快速回答直接问题的助手。",
)
```

**备注**：原文说明 ADK 可按职责拆分智能体，也可通过 LiteLLM 集成其他模型。本稿不将原文 `BaseAgent` 路由伪代码照搬为可运行代码；其上下文读取、事件构造和调用方式需要在选定版本中另行验证。


## A2. Critic System Prompt

评议者系统提示词

The source prompt defines the critic's role, review criteria, and constructive feedback requirements. The following Chinese version preserves those responsibilities.  
原文提示词定义了评议者的角色、审查标准与建设性反馈要求。以下中文版保留这些职责。

```python
# 原文系统提示词的中文翻译
CRITIC_SYSTEM_PROMPT = """
你是评议智能体，负责协作研究助手系统的质量保障。
你的主要职责是细致审查并质疑研究智能体提供的信息，
以确保准确性、完整性和无偏向的表述。

你的职责包括：
1. 评估研究发现的事实正确性、全面性及潜在倾向。
2. 识别缺失数据或推理中的不一致。
3. 提出能够完善或拓展当前理解的关键问题。
4. 提供建设性的改进建议，或建议探索其他角度。
5. 验证最终输出是否全面且平衡。

所有评议都必须具有建设性。
你的目标是加强研究，而不是否定研究。
请清晰组织反馈，并指出需要修订的具体内容。
你的总体目标是使最终研究成果达到尽可能高的质量标准。
"""
```

A resource-aware extension can add an explicit revision limit and a stopping decision.  
面向资源感知的扩展可以增加明确的修订次数上限和停止决策。

**备注**：修订上限与停止决策为补充建议，不混入“原文翻译”代码。最终系统可以让评议者输出“接受／修订／升级／停止”等受控结果，并由策略层执行预算检查。


## A3. Implementation Gaps in the Source Example

原文示例中的实现缺口

| Observation<br>观察 | Teaching treatment<br>教学处理 |
| --- | --- |
| The ADK routing block is explicitly conceptual.<br>ADK 路由块明确属于概念示意。 | Explain the control flow without claiming it is runnable.<br>解释控制流程，不声称可以直接运行。 |
| `reply` and `return` lose function indentation in the OpenAI block.<br>OpenAI 代码中的 `reply` 和 `return` 丢失函数缩进。 | Restore indentation before any future implementation.<br>后续实现前恢复缩进。 |
| Search declares a list but may return an error dictionary.<br>搜索声明返回列表，但可能返回错误字典。 | Use a consistent result structure and handle errors explicitly.<br>使用一致的结果结构并显式处理错误。 |
| JSON parsing and category access are not protected.<br>JSON 解析与类别访问缺少保护。 | Validate the response before selecting a route.<br>选路之前验证响应。 |
| Query categories may overlap.<br>查询类别可能重叠。 | Allow combined search and reasoning in a fuller policy.<br>在更完整的策略中允许搜索与推理组合。 |
| The example classifies tasks but does not track budgets.<br>示例分类任务，但不跟踪预算。 | Add accounting, feasibility checks, and bounded retries.<br>增加计量、可行性检查及有界重试。 |

The source demonstrates routing structure; it does not provide a complete production resource manager.  
原文演示了路由结构；它没有提供完整的生产级资源管理器。

**备注**：这页用于答疑，不必在主讲中逐行讨论异常处理。也不要将“默认选择 gpt-4o”误当作“首选模型失败后的自动回退”；原代码默认分支主要处理未知类别，未捕获模型调用失败。


# 修改建议清单（供你调整，不上屏）

1. **篇幅**：当前以 26 页正文保留完整章节逻辑。若限时 15–20 分钟，可合并 03–04、06–07、11–12、20–21，并将 18、23 放附录，压缩为 20 页正文。
2. **主线**：建议保留“金融分析导入 → 旅行规划分工 → 三类路由代码 → 预算可行性例子”的递进，不再增加第三套复杂案例。
3. **代码**：主讲重在“分类、选路、搜索上下文、预算检查”四件事，不展示整屏环境变量或 SDK 初始化。模型名称作为原文案例保留。
4. **实验**：当前只有教学假设，无实测结果页。若后续补实验，应使用相同任务集对比固定模型与路由策略，并计入分类、搜索、评议和重试开销。
5. **语言**：可调整中文讲法的流畅度，但不把英文没有的结论悄悄加入中文。所有新论点先补英文，再补对应中文，并明确假设条件。
6. **删改优先级**：优先保留定义、任务与资源双输入、评议反馈、代码决策路径、端到端评估。OpenRouter 参数及完整系统提示可始终留在附录。
