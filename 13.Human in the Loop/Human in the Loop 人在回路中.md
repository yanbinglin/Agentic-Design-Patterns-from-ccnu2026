# Human in the Loop 人在回路中

> 来源：本目录 [HITL.pptx](HITL.pptx)，共 28 页。按原课件页序整理文字、表格与插图；保留代码文本及截图，未执行示例代码。下文为课件内容转录，版本、指标与技术判断沿用课件，未另行核验。

## 内容概览

人在回路中将人的判断、监督与反馈嵌入智能体流程。课件介绍基础概念、六种实现方式、应用与局限，对比 HITL 与 HOTL，并以 Google ADK 客服 Agent 展示工具调用、个性化回调和人工升级。示例工具返回占位结果，真实工单及人工平台仍需接入。

## 课件逐页内容

> 每节对应一页幻灯片。文字按课件对象顺序保留，同一文本框内保留换行；流程图中的连接关系请结合原 PPTX 阅读。

### 第 1 页：人在回路中_HITL

人在回路中_HITL

组长：徐梦雪<br>
小组成员：何毓欣 田瑞洁 胡依婷 董珊

2026 年 11 月 2 日

![课件第 1 页原图 1](assets/pptx/slide-01-image-01.jpeg)

Human-in-the-Loop

#### 演讲者备注

建议讲述时间：25 秒<br>
讲稿：<br>
今天汇报 MINTQA，它是 ACL 2026 主会长论文，研究大模型在新知识与长尾知识上的多跳问答。汇报重点是：模型什么时候应该拆题，什么时候应该检索，以及这些流程选择为什么有时会降低准确率。最后我会联系数学智能体，提出一个可检验的流程诊断思路。<br>
来源：正文第 1 页<br>
https://aclanthology.org/2026.acl-long.18/

### 第 2 页：Human-in-the-Loop  汇报目录

目录

Human-in-the-Loop  汇报目录

2 / 28

01

基础概念

HITL 的定义、必要性、实现方式与落地原则

何毓欣

03–08

02

应用案例

典型应用、内容审核、自动驾驶与 HITL / HOTL

田瑞洁

09–14

03

代码实践

Agent 组成、工具调用与个性化回调

徐梦雪

15–20

04

总结回顾

核心理念、介入时机与人机双向协作

胡依婷

21–26

### 第 3 页：基础概念总览

基础概念总览

Human-in-the-Loop  基础概念

3 / 28

01

基础概念

理解 HITL 的含义与适用边界

HITL 的定义与必要性

六种实现方式与典型场景

局限与落地原则

演讲及文字稿：何毓欣

### 第 4 页：人在回路中（HITL）

人在回路中（HITL）

Human-in-the-Loop  基本概念

4 / 28

将人类的判断、创造力与语境理解，<br>
与 AI 的计算力和效率融入同一工作流程。

人类判断

HUMAN JUDGMENT

判断与创造力<br>
理解语境与价值取舍

AI 计算与效率

AI CAPABILITY

计算与数据处理<br>
高效完成重复性任务

核心理念：以人机协同放大人的能力

#### 演讲者备注

HITL 是智能体开发与部署中的关键策略，将人的判断、创造力与语境把握，与 AI 的计算力和效率结合起来。其底层逻辑是人机协同：AI 作为放大与延伸人类能力的工具，人与智能体各展所长，完成任何一方单独都难以达成的任务。

### 第 5 页：HITL 的必要性

01

02

03

HITL 的必要性

Human-in-the-Loop  基本概念

5 / 28

复杂、含糊或高风险的任务，更需要人的参与

守住边界

让 AI 在伦理与安全<br>
边界内运行，与业务<br>
目标和社会预期<br>
保持一致。

纠正偏差

识别错误，处理<br>
不确定性，降低<br>
关键决策中的<br>
误判代价。

保留掌控

由人监督与介入，<br>
让关键裁决建立在<br>
人类理解与判断<br>
之上。

最佳表现往往来自“自动化处理＋人类判断”

#### 演讲者备注

HITL 的核心，是让 AI 在伦理与安全边界内运转，并尽可能高效地达成目标。在复杂、含糊或高风险场景中，误判代价高昂，追求零人工、全自主往往并不审慎。人的监督、战略性参与与协同仍然必要。

### 第 6 页：HITL 的六种实现方式

01

02

03

04

05

06

HITL 的六种实现方式

Human-in-the-Loop  基本概念

6 / 28

将人的监督、干预与反馈嵌入 AI 流程

人类监督

持续审视行为与产出，<br>
守住边界。

干预与纠正

在错误或不确定时介入，<br>
纠偏并提供方向。

反馈学习

将人的偏好与标注回流，<br>
支持学习，如 RLHF。

决策增强

AI 提供分析与建议，<br>
人保留最终决策权。

人机协作

AI 处理批量事务，<br>
人负责创造与复杂判断。

升级策略

预先明确何时、如何<br>
将任务移交给人。

#### 演讲者备注

人类监督可通过日志复盘、实时看板持续审视智能体。干预与纠正在模型陷入错误、判断含糊或主动请求帮助时进行，人工可补充数据和方向。反馈学习将人的偏好和标注回流训练流程，RLHF 是典型范式。决策增强由模型提供建议，人保留最终裁决权。人机协作由 AI 处理批量化、结构化事务，人负责创造性问题求解和复杂谈判。升级策略预先明确何时、如何将任务移交给人，避免系统在能力边界之外勉强运行。

### 第 7 页：HITL 的典型应用场景

HITL 的典型应用场景

Human-in-the-Loop  基本概念

7 / 28

涉及高风险、定性判断或价值权衡时，<br>
人类应保留关键裁决权

金融信贷

AI 辅助分析

为信贷评估提供分析与建议。

信贷员综合判断

评估管理层素质等定性因素，<br>
并作出审批判断。

司法场景

AI 提供辅助信息

为司法工作提供信息支持。

法官保留最终裁决权

承担价值判断与最终裁决，<br>
维护正义与问责。

人的参与，为关键决策保留责任与问责机制

#### 演讲者备注

应用示例来自原始文案。在金融业中，大额对公贷款的最终审批通常仍需信贷员评估管理层素质等定性因素。在司法场景中，量刑等涉及深层价值判断的决定，应由法官保留最终裁决权，以维护正义与问责。HITL 有助于智能体进入完全自主不现实或不符合合规要求的敏感场景。

### 第 8 页：HITL 的局限与落地原则

01

02

03

HITL 的局限与落地原则

Human-in-the-Loop  基本概念

8 / 28

HITL 的效果取决于流程设计与人员能力

规模限制

人工审核难以覆盖<br>
百万级任务，需要<br>
明确哪些环节必须<br>
由人把关。

专业门槛

有效纠错依赖人员<br>
经验，训练数据标注<br>
还需要专门培训。

隐私成本

敏感信息需匿名化<br>
或脱敏，这会增加<br>
流程复杂度与<br>
治理成本。

自动化承担规模，人工把关关键环节

#### 演讲者备注

HITL 并非万能方案，最大短板在于可扩展性。人工把关难以覆盖百万级任务，常见折中是自动化承担规模、人工把关关键环节。效果高度依赖执行人员的专业水平，例如经验丰富的工程师才能识别隐蔽代码缺陷。若用于生成训练数据，标注人员需要学习有利于模型学习的纠正方式。隐私治理同样增加成本，敏感信息在人工处理前应完成严格匿名化或脱敏。

### 第 9 页：应用案例总览

应用案例总览

Human-in-the-Loop  应用案例

9 / 28

02

应用案例

观察人类介入的位置与方式

八类典型应用

内容审核与自动驾驶案例

HITL 与 HOTL 的区别及示例

演讲：田瑞洁    文字稿：董珊

### 第 10 页：HITL 的八类典型应用

HITL 的八类典型应用

Human-in-the-Loop  案例分析

10 / 28

AI 承担规模化初筛，人处理高风险与模糊环节

01

内容审核

模型初筛，人工复审边界内容

02

自动驾驶

常规任务自动执行，异常时请求接管

03

反欺诈

AI 标记可疑交易，人核查高风险警告

04

法律文档

AI 检索材料，律师复核法律风险

05

客服

机器人处理常见问题，复杂对话转人工

06

数据标注

人提供可靠标注，并随模型迭代更新

07

生成式内容

LLM 起草，编辑按品牌与受众定稿

08

自治网络

AI 预判告警，高危变更由人审核

共同原则：自动化负责规模，人类负责边界与复核

### 第 11 页：内容审核：模糊案例进入人工复审

内容审核：模糊案例进入人工复审

Human-in-the-Loop  案例分析

11 / 28

机器先筛批量内容，人判断规则边界

海量图文、视频<br>
与评论

模型初筛

边界内容<br>
人工复审

最终判定

明显违规内容：依据规则快速识别与处理

边界案例：双关、隐喻、地域梗与反讽

反馈闭环

人工判定形成<br>
新标注数据

反馈至模型训练<br>
改善边界识别

机器筛批量，人判断模糊，判定结果支持后续迭代

### 第 12 页：自动驾驶：按风险边界请求接管

自动驾驶：按风险边界请求接管

Human-in-the-Loop  案例分析

12 / 28

适用于设有驾驶员接管机制的系统

常规、可预测场景

处于系统能力范围内

系统感知、规划与执行

能力边界外

置信不足<br>
或环境难以预测

触发接管请求

驾驶员介入

例如：极端暴雨、大雾、异常障碍物或复杂非常规道路

风险分层的关键，是明确何时移交控制权

### 第 13 页：人在回路与人在环上

人在回路与人在环上

Human-in-the-Loop  案例分析

13 / 28

区别在于人介入的位置与频率

HITL  人在回路

HOTL  人在环上

人的角色

参与关键个案审核与处理

人的角色

制定策略、规则与约束

介入方式

遇到风险或模糊情况时复核

介入方式

不必逐案审批，监督执行边界

AI 的工作

初筛并完成初步处理

AI 的工作

按政策自主处理高频任务

示例

边界内容审核、驾驶接管

示例

自动化交易、客服路由

两种方式都需明确边界，并保留监督与接管路径

### 第 14 页：人在环上：人定策略，AI 执行

人在环上：人定策略，AI 执行

Human-in-the-Loop  案例分析

14 / 28

自动化交易

呼叫中心

人：设定策略与风险边界

原文中的假设规则：<br>
70% 科技股、30% 债券<br>
单只股票持仓不超过 5%<br>
跌破成本价 10% 自动止损

AI 按规则持续监测与执行

主管：制定服务政策

提到“服务中断”<br>
优先转接技术专家<br>
识别到客户情绪急躁<br>
按政策转交人工

AI 理解需求并完成路由分发

交易比例与阈值为文字稿中的示例规则，不代表投资建议。

### 第 15 页：代码实践总览

代码实践总览

Human-in-the-Loop  代码实践

15 / 28

03

代码实践

通过客服 Agent 理解 HITL 的实现

Agent 的基本组成与工具调用

个性化回调与请求上下文

代码实践的意义与局限

演讲及文字稿：徐梦雪

### 第 16 页：Agent 的基本组成

Agent 的基本组成

Human-in-the-Loop  代码实践

16 / 28

Google ADK 是用于开发智能体的工具包

M

Model

模型

理解问题并分析情况，<br>
决定下一步操作。

I

Instruction

指令

规定行为规则、处理顺序<br>
与任务要求。

T

Tools

工具

执行排查、创建工单、<br>
转交人工等操作。

用户提出问题

Model + Instruction<br>
理解与决策

选择并调用 Tools

返回处理结果

模型负责理解与决策，指令规定行为，工具执行任务

```text
technical_support_agent = Agent(                                    # 创建一个 Agent 实例
```

    name="technical_support_specialist",                            # 名称：标识这个智能体

    model="gemini-2.0-flash-exp",                                   # 模型：理解问题、生成回答

    instruction="You are a technical support specialist ...",       # 指令：规定角色与处理规则

    tools=[troubleshoot_issue, create_ticket, escalate_to_human],   # 工具：可调用的能力

)<br>

#### 演讲者备注

来源：用户提供的第13章代码截图。结构图为该示例的简化概括。

### 第 17 页：三个工具：从自动处理到人工介入

三个工具：从自动处理到人工介入

Human-in-the-Loop  代码实践

17 / 28

01

troubleshoot_issue

故障排查

分析问题，尝试解决<br>
常见故障。

02

create_ticket

创建工单

记录尚未解决的问题，<br>
便于后续跟进。

03

escalate_to_human

转交人工

遇到复杂问题时，<br>
发起人工处理请求。

案例：“我的耳机蓝牙连不上电脑”

用户反馈

尝试排查

仍未解决

创建工单

转交人工

以上是处理意图示意；示例工具仅返回占位结果，未接入真实客服系统

简单问题优先自动处理，复杂问题及时转交人工

```text
def troubleshoot_issue(issue: str) -> dict:                       ← ① 故障排查
```

```text
    return {"status": "success", "report": f"Troubleshooting steps for {issue}."}
```

```text
def create_ticket(issue_type: str, details: str) -> dict:         ← ② 创建工单
```

```text
    return {"status": "success", "ticket_id": "TICKET123"}
```

```text
def escalate_to_human(issue_type: str) -> dict:                   ← ③ 转交人工
```

    # 真实系统中会转交到人工队列

```text
    return {"status": "success", "message": f"Escalated {issue_type} to a human specialist."}
```

#### 演讲者备注

来源：用户提供的第13章工具定义与 instruction 截图。三个工具均为占位实现。流程图描述设计意图，不代表已经运行真实工单或人工客服流程。

### 第 18 页：个性化回调：让回答结合客户背景

个性化回调：让回答结合客户背景

Human-in-the-Loop  代码实践

18 / 28

personalization_callback

```text
def personalization_callback(
```

    callback_context: CallbackContext,

    llm_request: LlmRequest

) -> Optional[LlmRequest]:

    customer_info = callback_context.state.get(

        "customer_info"

    )

    if customer_info:

        customer_name = customer_info.get(

            "name", "valued customer"

        )

        customer_tier = customer_info.get(

            "tier", "standard"

        )

        recent_purchases = customer_info.get(

            "recent_purchases", []

        )

def：定义函数

在特定生命周期时机执行。

callback_context

从 state 获取客户状态信息。

llm_request

即将发送给大模型的请求。

.get()：读取与默认值

缺少 name 字段时，<br>
使用 "valued customer"。

Optional[LlmRequest]

原示例类型提示：<br>
请求对象或 None。

工具执行操作；回调在特定时机补充逻辑，函数后半段见下一页

#### 演讲者备注

代码来源：用户提供的第13章 personalization_callback 截图，展示函数前半段，按括号允许的方式换行，逻辑未改。类型提示沿用原示例。当前 ADK 的 before_model_callback 约定返回 Optional[LlmResponse]，返回 None 继续流程，实际适配需核对版本。官方说明：https://adk.dev/callbacks/types-of-callbacks/ 。

### 第 19 页：从客户状态到个性化回答

从客户状态到个性化回答

Human-in-the-Loop  代码实践

19 / 28

补充上下文，原地修改请求（接上页函数）

```text
        personalization_note = (
```

            f"\nIMPORTANT PERSONALIZATION:\n"

            f"Customer Name: {customer_name}\n"

            f"Customer Tier: {customer_tier}\n"

        )

        if recent_purchases:

            personalization_note += (

                f"Recent Purchases: "

                f"{', '.join(recent_purchases)}\n"

            )

        if llm_request.contents:

            system_content = types.Content(

                role="system",

                parts=[types.Part(text=personalization_note)],

            )

            llm_request.contents.insert(0, system_content)

```text
    return None
```

读取客户信息

提取姓名、等级与购买记录

f-string 插值，\n 换行

检查 contents 是否非空

构造系统消息，插入请求开头

模型结合可用上下文生成回答

请求为空：跳过插入并继续处理

示例输入：小明 / VIP / 耳机

IMPORTANT PERSONALIZATION:<br>
Customer Name: 小明<br>
Customer Tier: VIP<br>
Recent Purchases: 耳机

```text
return None：按回调约定继续处理，请求已原地修改
```

这是提示信息示例，不保证特定模型回答

#### 演讲者备注

代码来源：用户提供的第13章 personalization_callback 截图，展示函数后半段。这里的 types.Content(role="system", ...) 沿用原示例，解释其意图，不代表已通过当前 Gemini 后端验证。当前 ADK 通常修改 llm_request.config.system_instruction，before_model_callback 返回 None 表示继续模型调用，返回 LlmResponse 可替代模型结果。官方来源：https://adk.dev/callbacks/types-of-callbacks/ ；https://github.com/googleapis/python-genai#system-instructions-and-other-configs 。例中将 system_content 插入 contents 的索引 0；请求为空则跳过。

### 第 20 页：代码实践的意义与局限

代码实践的意义与局限

Human-in-the-Loop  代码实践

20 / 28

设计意义

实现局限

01

Agent 通过工具执行具体任务。

02

回调在特定时机补充客户背景。

03

自动处理与人工介入衔接，<br>
形成问题处理流程。

01

工具实现简化，返回占位结果。

02

工单与人工转交尚未接入真实平台。

03

回调须注册、触发，并核对<br>
ADK 返回类型与系统指令写法。

04

客户信息需权限控制与隐私保护。

HITL 的关键：在适当的时候，让人类参与决策和处理

#### 演讲者备注

来源：用户提供的第13章代码截图。兼容性核对：当前 ADK 的 before_model_callback 返回类型为 Optional[LlmResponse]，原截图使用 Optional[LlmRequest]；系统指令接口通常为 llm_request.config.system_instruction。参考：https://adk.dev/callbacks/types-of-callbacks/ ；https://github.com/googleapis/python-genai#system-instructions-and-other-configs 。本报告未运行真实模型、真实工单或人工客服系统。

### 第 21 页：总结回顾总览

总结回顾总览

Human-in-the-Loop  总结回顾

21 / 28

04

总结回顾

回顾核心理念与人类介入的时机

HITL 的价值与人机分工

引入 HITL 的四类判断情形

Agent 与 Human 的双向协作

演讲及文字稿：胡依婷

### 第 22 页：At a Glance：HITL 是什么？

At a Glance：HITL 是什么？

Human-in-the-Loop  章节总结

22 / 28

完全自主的 AI 在高风险场景中并不审慎，<br>
这正是 HITL 存在的理由。

大模型的固有短板

需要细致判断、伦理推理的任务上暴露短板

对复杂、含糊语境的理解深度不足

缺乏人类与生俱来的常识推理能力

不具备真正的创造力与价值判断

完全自主的代价

高风险场景中失误可能造成严重安全事故

财务损失与声誉代价极其高昂

伦理层面的误判难以挽回

关键决策链路中纯自动化既不安全也不可靠

在关键决策中完全依赖自动化，往往既不审慎，也不安全

### 第 23 页：At a Glance：为什么需要 HITL？

At a Glance：为什么需要 HITL？

Human-in-the-Loop  章节总结

23 / 28

人机协同不是替代关系，而是各展所长的共生伙伴关系。

AI 负责 · 计算重活

大规模数据处理与运算

重复性、结构化任务自动化

快速检索与信息整合

批量内容生成与初筛

人类负责 · 判断与监督

伦理判断与价值取舍

复杂语境下的细腻理解

创造性问题求解与决策

实时监督、干预与纠偏

最终目标：构建协作生态，让人与智能体各展所长，<br>
完成任何一方单独都难以达成的任务。

### 第 24 页：Rule of Thumb：什么时候应该把人放进回路？

Rule of Thumb：什么时候应该把人放进回路？

Human-in-the-Loop  章节总结

24 / 28

当任务存在高风险、判断模糊、需要人类反馈或持续优化时，就应该引入 HITL

01

高风险场景：
错误可能造成严重后果

典型领域：医疗诊断、金融风控、
自动驾驶、工业机器人等

核心原因：决策失误可能危及生命安全、
造成经济损失或引发严重社会影响，
需要人工审核与最终把关

02

边界模糊：
任务涉及复杂语境与价值判断

典型任务：内容审核、复杂客服、
争议性信息处理等

核心原因：任务缺乏明确标准，
模型难以准确理解语境、情绪和价值取向，
需要人类进行综合判断

03

持续学习：
模型需要人类反馈来提升能力

典型场景：RLHF 人类反馈强化学习、
人工数据标注、模型输出评价

核心原因：人类通过评价、纠错和反馈，
为模型提供学习信号，帮助模型不断改进。

04

需要迭代：
依靠高质量人工数据持续优化模型

典型场景：生成式 AI 内容打磨

核心原因：通过人工筛选、修改和完善模型输出，
积累高质量训练数据，
持续提升模型的准确性与实用性。

### 第 25 页：HITL 的核心：Agent 与 Human 保持双向协作

HITL 的核心：Agent 与 Human 保持双向协作

Human-in-the-Loop  章节总结

25 / 28

人在回路设计模式

Prompt

任务 / 指令输入

Agent

AI 智能体

Human

审核 / 指导

User

用户

Output

输出结果

人工提供监督、反馈与必要干预

重点：人并非替代 Agent，而是在关键处把关，并与 Agent 双向协作。

用户发出 Prompt，Agent 生成 Output；Human 可监督并指导 Agent，形成 HITL 协作。

### 第 26 页：总结：让 AI 更高效，也让决策更可靠

总结：让 AI 更高效，也让决策更可靠

Human-in-the-Loop  章节总结

26 / 28

HITL 不是对自动化的否定，而是为智能体系统<br>
建立必要的人类监督与责任机制。

AI 执行与扩展能力，人类判断与承担责任

可靠性

降低高风险错误，<br>
提升关键输出的可信度

适应性

通过人工反馈持续<br>
修正与改进系统

责任性

保留价值判断、<br>
人工接管与最终问责

在医疗辅助、金融风控、内容审核和复杂客服等场景中，<br>
关键是“何时需要人类介入”。

### 第 27 页：小组分工

小组分工

Human-in-the-Loop  小组分工

27 / 28

汇报顺序与内容分工如下：

01

何毓欣

基础概念

基础概念部分演讲及文字稿

02

田瑞洁

案例与 PPT

PPT 制作与排版，案例部分演讲

03

徐梦雪

代码实践

代码部分演讲及文字稿

04

胡依婷

总结回顾

总结部分文字稿及演讲

05

董珊

案例文字稿

案例部分文字稿

### 第 28 页：Human-in-the-Loop  感谢聆听

尾页

Human-in-the-Loop  感谢聆听

28 / 28

感谢聆听

Thanks for listening!

Human-in-the-Loop

## 小组成员与分工

> 依据课件中的目录、署名与分工页整理；按课件分工顺序列出。

- 何毓欣：基础概念部分演讲及文字稿

- 田瑞洁：PPT 制作与排版，案例部分演讲

- 徐梦雪（组长）：代码部分演讲及文字稿

- 胡依婷：总结部分文字稿及演讲

- 董珊：案例部分文字稿
