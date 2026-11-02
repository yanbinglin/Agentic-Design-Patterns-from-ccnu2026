# Human in the Loop 人在回路中

> 来源：本目录《HITL.pptx》。本文按主题整理课件中的概念、案例、图表与代码；代码为课件示例的节选与归纳，未在本次整理中执行。

## 1. 什么是人在回路中

**人在回路中（Human-in-the-Loop，HITL）**是把人的判断、监督、反馈与必要干预嵌入 AI 工作流程的设计方式。AI 承担计算、检索和批量处理，人类在关键环节进行审核、指导或决策。

核心流程可以概括为：用户提出任务，Agent 执行并产生结果，人类在预定环节监督、反馈或接管，Agent 根据反馈继续处理。

## 2. 为什么需要人的参与

复杂语境、含糊标准与高风险决策往往需要人的判断。模型给出的结果可能看似合理，却遗漏语境、责任要求或价值权衡。

| 参与目的 | 作用 |
| --- | --- |
| 守住边界 | 在伦理、安全和业务约束内运行 |
| 纠正偏差 | 对不确定或错误输出及时干预 |
| 补充判断 | 处理定性因素、复杂情境与价值取舍 |
| 持续改进 | 通过评价、纠错和标注提供反馈 |
| 明确责任 | 为关键决定保留审核、接管与问责机制 |

例如，信贷分析中 AI 可以整理材料与提出建议，信贷员综合判断管理层素质等因素；司法场景中 AI 可以提供信息支持，最终裁决仍由具备职责的人作出。

## 3. 六种实现方式

| 方式 | 人类参与什么 | 主要作用 |
| --- | --- | --- |
| 人类监督 | 审视行为与产出 | 持续检查是否越界 |
| 干预与纠正 | 对错误或不确定情况介入 | 纠偏并提供方向 |
| 反馈学习 | 提供偏好、评价与标注 | 支持后续训练或系统改进，如 RLHF |
| 决策增强 | 阅读 AI 分析与建议 | 保留最终决策权 |
| 人机协作 | 与 AI 分担不同任务 | AI 批量处理，人负责创造与复杂判断 |
| 升级策略 | 在预定条件下接管任务 | 明确何时、如何移交 |

人工反馈用于当前任务修正，与反馈进入后续训练是两种过程。一次纠错是否形成长期能力更新，取决于是否存在相应的数据和学习流程。

## 4. 典型应用场景

| 场景 | AI 的工作 | 人类的工作 |
| --- | --- | --- |
| 内容审核 | 批量初筛 | 复审隐喻、反讽等边界内容 |
| 自动驾驶 | 能力范围内的感知、规划与执行 | 在设有接管机制的系统中处理超出边界的情形 |
| 反欺诈 | 标记可疑交易 | 核查高风险警告 |
| 法律文档 | 检索与整理材料 | 复核法律风险 |
| 客服 | 处理常见问题 | 接手复杂对话 |
| 数据标注 | 辅助预标注与批量处理 | 提供可靠标注并随迭代更新 |
| 生成式内容 | 起草文案 | 按品牌、受众与事实要求定稿 |
| 自治网络 | 预测与筛选告警 | 审核高危变更 |

### 4.1 内容审核的反馈闭环

海量内容 → 模型初筛 → 边界内容进入人工复审 → 最终判定。

明显违规内容可依据规则快速处理；双关、地域梗、隐喻和反讽等案例交由人工结合语境判断。人工结果可以形成新标注数据，支持后续迭代。

### 4.2 自动驾驶的接管边界

课件讨论设有驾驶员接管机制的系统：常规、可预测情形由系统处理；极端暴雨、大雾、异常障碍物或复杂非常规道路等情况，若超出能力边界，则触发接管请求。

流程设计需要明确触发条件、控制权移交方式以及接管责任。

## 5. HITL 与 HOTL 的区别

**人在环上（Human-on-the-Loop，HOTL）**侧重由人制定政策并监督系统运行，而不是逐个处理所有任务。

| 维度 | HITL：人在回路 | HOTL：人在环上 |
| --- | --- | --- |
| 人的角色 | 参与关键个案的审核和处理 | 制定策略、规则与约束 |
| 介入方式 | 在风险或模糊环节直接复核 | 监督整体执行，必要时干预 |
| AI 的工作 | 初筛及初步处理 | 在政策范围内自主处理高频任务 |
| 课件示例 | 边界内容审核、驾驶接管 | 自动化交易、客服路由 |

两种方式都需要明确边界，并保留监督与接管路径。

### 人定策略、AI 执行的案例

- **自动化交易**：人设置资产比例、单一持仓上限和止损规则，系统按规则持续监控。课件中的 70% 科技股、30% 债券、单只股票不超过 5%、下跌 10% 止损均为假设规则。
- **呼叫中心**：主管制定转接政策，AI 识别“服务中断”等问题并路由至技术专家；识别到客户情绪急躁时，按政策交由人工。

## 6. Google ADK 客服 Agent

课件用客服助手说明 Model、Instruction 和 Tools 的配合：模型理解问题并决定下一步，指令规定角色与处理规则，工具执行排查、建单及人工升级。

### 6.1 Agent 的组成

```python
# 课件结构节选：省略导入及完整指令。
technical_support_agent = Agent(
    name="technical_support_specialist",
    model="gemini-2.0-flash-exp",
    instruction="You are a technical support specialist ...",
    tools=[troubleshoot_issue, create_ticket, escalate_to_human],
)
```

模型名称沿用课件。该节主要解释流程结构，不代表已经接入真实客服系统。

### 6.2 三个工具与人工升级

```python
def troubleshoot_issue(issue: str) -> dict:
    return {
        "status": "success",
        "report": f"Troubleshooting steps for {issue}.",
    }


def create_ticket(issue_type: str, details: str) -> dict:
    return {"status": "success", "ticket_id": "TICKET123"}


def escalate_to_human(issue_type: str) -> dict:
    # 真实系统需要在这里对接人工队列。
    return {
        "status": "success",
        "message": f"Escalated {issue_type} to a human specialist.",
    }
```

以“耳机蓝牙连不上电脑”为例，Agent 先尝试排查，未解决则创建工单，再发起人工转接。

**以上函数只返回占位结果。返回 `success` 不等于已经完成真实排查、建单或人工接管。**

## 7. 个性化回调：将客户背景加入请求

工具负责执行操作，回调则在特定生命周期时机补充处理逻辑。课件的 `personalization_callback` 从会话状态读取客户姓名、等级和近期购买记录，再修改即将发送给模型的请求。

### 7.1 合并后的回调示例

下面将课件中跨页展示的函数合并，保留原示例的处理方式；省略导入和回调注册。

```python
def personalization_callback(
    callback_context: CallbackContext,
    llm_request: LlmRequest,
) -> Optional[LlmRequest]:
    customer_info = callback_context.state.get("customer_info")
    if customer_info:
        customer_name = customer_info.get("name", "valued customer")
        customer_tier = customer_info.get("tier", "standard")
        recent_purchases = customer_info.get("recent_purchases", [])

        personalization_note = (
            f"\nIMPORTANT PERSONALIZATION:\n"
            f"Customer Name: {customer_name}\n"
            f"Customer Tier: {customer_tier}\n"
        )
        if recent_purchases:
            personalization_note += (
                f"Recent Purchases: {', '.join(recent_purchases)}\n"
            )

        if llm_request.contents:
            system_content = types.Content(
                role="system",
                parts=[types.Part(text=personalization_note)],
            )
            llm_request.contents.insert(0, system_content)
    return None
```

### 7.2 处理过程

1. 从 `callback_context.state` 读取 `customer_info`。
2. 使用 `.get()` 为缺失字段提供默认值。
3. 将姓名、等级和购买记录整理为提示文本。
4. 在请求内容非空时，将文本插入请求开头。
5. 返回 `None`，按示例约定继续处理，请求已被原地修改。

输入“小明 / VIP / 耳机”时，补充的文本为：

```text
IMPORTANT PERSONALIZATION:
Customer Name: 小明
Customer Tier: VIP
Recent Purchases: 耳机
```

这是上下文构造示例，不能保证模型产生某种固定回答。实际接入时还需注册并触发回调，核对 ADK 的返回类型和系统指令写法，并控制客户信息的使用权限。

## 8. 局限与落地原则

| 局限 | 影响 | 设计要求 |
| --- | --- | --- |
| 人工规模有限 | 无法逐一覆盖海量任务 | 让自动化承担批量处理，筛选必须人工把关的环节 |
| 依赖专业能力 | 审核与纠错质量取决于经验 | 提供培训、规则和必要上下文 |
| 涉及隐私 | 人工处理可能扩大敏感信息接触范围 | 匿名化、脱敏与权限控制 |
| 升级链路未完成 | 发出请求后可能无人实际接管 | 对接队列、工单和责任人，确认接管状态 |

课件归纳四类需要人参与的情形：错误后果严重、任务边界模糊、需要反馈学习，以及依赖高质量人工数据持续迭代。

## 9. 复习要点

1. HITL 将监督、反馈和关键判断嵌入流程。
2. 人类介入的位置、触发条件和责任需要预先明确。
3. HITL 与 HOTL 的区别主要在参与位置与频率。
4. 客服示例展示工具和回调结构，未完成真实平台接入。
5. 人工反馈只有进入相应更新流程，才可能支持长期改进。
6. 可靠性、适应性与责任机制共同决定人机协作的效果。

原始课件：[[HITL.pptx]]
