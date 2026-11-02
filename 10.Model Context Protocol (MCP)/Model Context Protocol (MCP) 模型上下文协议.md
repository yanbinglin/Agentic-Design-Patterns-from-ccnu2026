# Model Context Protocol (MCP) 模型上下文协议

> 来源：本目录 [MCP.pptx](MCP.pptx)，共 48 页。按原课件页序整理文字、表格与插图；保留代码文本及截图，未执行示例代码。下文为课件内容转录，版本、指标与技术判断沿用课件，未另行核验。

## 内容概览

MCP 将外部工具、资源和提示模板的发现与访问协议化。Host 承载应用与模型，Client 负责协议连接，Server 封装外部能力。Function Calling 表达模型的调用意图，MCP 则提供外部能力的标准化接入。课件依次介绍架构与调用流程、两种机制的配合、选型与工程考量，以及 ADK 文件系统接入和 FastMCP 服务端实践。

## 课件逐页内容

> 每节对应一页幻灯片。文字按课件对象顺序保留，同一文本框内保留换行；流程图中的连接关系请结合原 PPTX 阅读。

### 第 1 页：汇报人：李思琪 代嘉仪 方欣怡 屈家豪 陈志文

汇报人：李思琪 代嘉仪 方欣怡 屈家豪 陈志文

![课件第 1 页原图 1](assets/pptx/slide-01-image-01.jpeg)

Model Context Protocol

二零二六年十一月

模型上下文协议（MCP）

### 第 2 页：CONTENTS/汇报目录

CONTENTS/汇报目录

       MCP模式概览

     MCP与工具调用函数

MCP的额外考量

实践应用与举例

01

02

03

04

李思琪  |  起源、概念、架构

代嘉仪 |  关联、交互机制

方欣怡  |  局限、优化方向

屈家豪、陈志文  |  场景、案例解析

### 第 3 页：MCP模式概览

MCP模式概览

01

讲述人：李思琪

PART ONE

### 第 4 页：MCP模式概览

MCP模式概览

 三大板块 · 介绍

01

MCP 的起源与背景

M×N 集成难题 · USB-C 类比 · 发展时间线

02

总体架构与模块功能

Host / Client / Server 三大角色 · 三大原语

03

MCP 完整调用流程

生命周期四步 · JSON-RPC 报文 · 天气查询案例

贯穿主线

Host 宿主应用

MCP Client 客户端

MCP Server 服务器

外部系统 / 数据源

MCP · Model Context Protocol 模型上下文协议

### 第 5 页：初识--集成 MCP 的 AI coding 客户端

初识--集成 MCP 的 AI coding 客户端

常见MCP广场--百度云、腾讯云、火山云

MCP · Model Context Protocol 模型上下文协议

![课件第 5 页原图 2](assets/pptx/slide-05-image-02.png)

![课件第 5 页原图 3](assets/pptx/slide-05-image-03.png)

Work Buddy客户端

集成了MCP生态广场（入驻了很多第三方）

后续可在交互聊天框下方查看MCP，开启连接后可以使用对应的API，去使用对应的工具。此时就可以实现用户是用少量的调用费用完成一些较为困难与复杂的任务。

### 第 6 页：起源与背景

 起源与背景

MCP: 定义与诞生背景

为什么需要 MCP —— 模型存在能力边界

什么是 MCP ?

Model Context Protocol（模型上下文协议）<br>
—— 2024 年 11 月由 Anthropic 发布的开放标准，为大模型连接外部数据源、工具与服务提供统一的接口规范。

一句话: 给 AI 的能力接入定一套「通用语言」。

类比: AI 界的 USB-C 接口

USB-C 统一了各种设备的充电与传输接口；MCP 统一了 LLM 与外部世界的连接方式 —— 一次接入，处处可用，换模型无需改集成。

有 MCP: 统一协议中枢，M + N 即可全互通

GPT

Claude

Gemini

MCP

GitHub

数据库

Slack

2024.11

Anthropic 发布 MCP

2025.03

OpenAI 宣布采纳

2025.04

Google DeepMind 跟进

2025+

生态爆发: 主流工具全面支持

MCP · Model Context Protocol 模型上下文协议

模型的能力边界

① 知识被冻结--只记得训练时见过的数据，新发生的事一无所知

② 无法行动--不能读文件、查数据库、发信息，只会”说“不会”做“

③ 适配繁琐--每个工具一套接法，换个模型就得重新写一遍集成代码

### 第 7 页：标准分裂

 标准分裂

同一工具,  两套标准

function calling 定义的标准 —— 各项目各自为政

function calling 定义的标准

①

标准 1 · 项目 A

工具的定义 : schema1

工具调用的方式 1

自定义的参数格式与调用约定

②

标准 2 · 项目 B

工具的定义 : schema2

工具调用的方式 2

自定义的参数格式与调用约定

promptA

get_weather

promptB

同一个功能 get_weather —— 项目 A 与项目 B 用不同的 schema、不同的调用方式各自实现一遍

MCP · Function Calling 痛点举例讲解

03 / 8

### 第 8 页：三大痛点

 三大痛点

痛点① : 切换模型, 工具定义全部重写

同一个工具, GPT 与 Claude 要求的格式并不相同

OpenAI GPT · function calling

```text
{
  "name": "get_weather",

"parameters": {
    "type": "object",
    "properties": {
      "city": {
        "type": "string"
      }
    }
  }
}
```

Anthropic Claude · tool use

```text
{
  "name": "get_weather",

"input_schema": {
    "type": "object",
    "properties": {
      "city": {
        "type": "string"
      }
    }
  }
}
```

≠

字段名不同

→ 工具定义需要兼容不同模型的定义方式 —— 每换一次模型, 所有工具都要按新格式重写一遍

MCP · Function Calling 痛点举例讲解

04 / 8

### 第 9 页：三大痛点

三大痛点

痛点② : 不同人定义同一个工具, 效果不同

同名 get_weather, 参数结构 / 返回字段 / 描述方式完全不同

👤 开发者 A 的 get_weather

参数定义

get_weather(city: string)<br>
// 城市名, 如 "北京"

返回格式

{ "temp": 25,<br>
  "cond": "晴" }

工具描述

"查天气, 返回温度与天气状况"

👤 开发者 B 的 get_weather

参数定义

get_weather(location:<br>
  { lat, lon })  // 经纬度

返回格式

{ "weather": "sunny",<br>
  "temperature_c": 25 }

工具描述

"输入经纬度, 返回英文字段"

≠

→ 工具能力需要统一 —— LLM 看到两个「同名工具」却要猜两种格式, 调用效果天差地别

MCP · Function Calling 痛点举例讲解

05 / 8

### 第 10 页：三大痛点

三大痛点

痛点③ : 工具无法跨项目复用

项目 A 定义好的工具, 项目 B 想用必须重做一遍

📦 项目 A

已定义好的工具集

get_weather

send_email

query_db

search_docs

✕

无法直接复用

只能复制 / 重写

📦 项目 B

同样需要这些工具

get_weather

需重新实现

send_email

需重新实现

query_db

需重新实现

……

项目 B 想用这些工具? 只能复制代码、改格式、重新实现一遍 —— 重复造轮子, 同一工具出现 N 个版本

→ 工具需要「一次定义、处处可用」

MCP · Function Calling 痛点举例讲解

06 / 8

### 第 11 页：MCP 解法

 MCP 解法

MCP : 一次定义, 处处可用

三大痛点 → 三大对策, 加一个统一协议层

痛点①    模型各异

GPT / Claude 格式互不兼容, 换模型就要重写

✅    统一开放协议

任何模型按同一标准接入, 无需重写

痛点②   定义不一

同名工具, 参数与返回格式天差地别

✅    标准化能力描述

tools/list 统一 schema 与描述

痛点③   无法复用

跨项目要用就得重新实现一遍

✅    Server 一次发布

所有 Host、项目都能连接使用

各模型

GPT · Claude · Gemini

MCP Server

get_weather

tools/list · tools/call

各项目 / Host

项目 A · 项目 B<br>
Claude Desktop · Cline

同一个 get_weather 只写一次、部署为 Server —— 所有模型、所有项目即插即用

MCP · Function Calling 痛点举例讲解

07 / 8

### 第 12 页：架构与模块

架构与模块

总体架构: 一图看懂 MCP

数据流向: 用户 → Agent → MCP Server → 外部服务 → 原路返回

![课件第 12 页原图 4](assets/pptx/slide-12-image-04.png)

架构四要素解读

👤 用户 & 提示词

自然语言下达任务

🤖 Agent / Client

解析意图 · 构造标准请求

🔌 MCP Server

统一网关 · 暴露能力

🌐 第三方服务

数据库 · API · 工具集

通信协议

JSON-RPC 2.0

传输层

本地 STDIO / 远程 HTTP

核心思想: 把「工具的发现、描述、调用」全部协议化 —— 任何 LLM 都能按同一套标准接入任何外部系统。

MCP · Model Context Protocol 模型上下文协议

### 第 13 页：架构与模块

 架构与模块

三大核心模块: Host · Client · Server

每个结构的定位、职责与真实实例

🖥️ Host 宿主应用

如 Claude Desktop / Cursor

定位

用户直接使用的 AI 应用，整个交互的载体

核心职责

承载 LLM 与对话界面

内嵌一个或多个 MCP Client

统一管理与 Server 的连接

决定暴露哪些能力给用户

🔄 MCP Client 客户端

协议中间层

定位

运行在 Host 内、与 Server 一对一连接的翻译官

核心职责

发起 initialize 协议握手

能力协商与工具发现

将 LLM 意图翻译为标准请求

路由调用结果并回传

🔌 MCP Server 服务器

能力提供方

定位

封装外部系统、对外暴露三类原语的网关

核心职责

实现 Tools / Resources / Prompts

处理认证、鉴权与执行

一个 Server 专注一个领域

通过 STDIO / HTTP 对外服务

连接关系: 1 个 Host 可管理多个 Client，每个 Client 与 1 个 Server 保持一对一连接 —— 标准化让角色彻底解耦。

MCP · Model Context Protocol 模型上下文协议

### 第 14 页：调用流程

调用流程

MCP 完整调用流程: 生命周期四步

从协议握手到结果回传 —— 每一步都是标准化的 JSON-RPC 消息

1

握手初始化

Client 与 Server 协商协议版本、交换双方能力声明

initialize

```text
{
 "method": "initialize",
 "params": {
  "protocolVersion":
   "2025-03-26",
  "capabilities": {...}
 }
}
```

2

能力发现

Client 拉取 Server 暴露的工具清单与参数 Schema

tools/list

```text
{
 "method": "tools/list"
}

← 返回:
[
 { "name": "get_weather",
   "inputSchema": {...} }
]
```

3

工具调用

LLM 决策后，Client 携带参数发起真实调用

tools/call

```text
{
 "method": "tools/call",
 "params": {
  "name": "get_weather",
  "arguments": {
   "city": "北京"
  }
 }
}
```

4

结果回传

Server 执行完毕，结构化结果写回 LLM 上下文

result

```text
{
 "content": [
  {
   "type": "text",
   "text": "晴, 25℃"
  }
 ]
}
```

传输层

本地: STDIO 标准输入输出，子进程直连，低延迟

远程: Streamable HTTP，跨网络共享，可扩展

MCP · Model Context Protocol 模型上下文协议

### 第 15 页：MCP与工具调用函数

MCP与工具调用函数

PART TWO

02

讲述人：代嘉仪

### 第 16 页：2.1  从一个问题开始

2.1  从一个问题开始

“北京明天会下雨吗？要带伞吗？”

用户需要的是出行建议

模型本身并不知道现实天气。先取到实时预报，再根据降水信息回答，才有依据。

1

模型表达要查什么<br>
城市 = 北京；日期 = 明天

2

AI 应用调用天气服务<br>
把真实结果交回模型

Function Calling

模型提出工具名与参数

MCP

标准化外部工具接入

#### 演讲者备注

讲解提示：例子改为天气，因为“明天天气”是自然且需要实时信息的问题。可通过天气 API 获取；普通网页搜索也是另一类工具，但不是本例必经步骤。无论哪种接入方式，必须先有可用且获准调用的数据源。这里不要暗示模型自带天气数据库。两种机制在同一任务中承担不同环节。<br>
资料：所附《MCP.pdf》第1–3页；https://www.runoob.com/ai-agent/ai-agent-function-calling.html；https://www.runoob.com/ai-agent/tools-integration.html。天气工具名称、参数和预测结果均为课堂示意，不代表真实天气。

### 第 17 页：2.2  Function Calling 时序

2.2  Function Calling 时序

模型识别工具并提出调用，客户端完成执行闭环

用户

用户

客户端

客户端

大语言模型

大语言模型

工具函数

工具函数

提出问题

发送问题 + 可用工具列表

返回工具调用请求

执行工具函数

返回结果

发送工具结果

生成最终回答

返回最终答案

模型识别需要工具<br>
并指定参数

#### 演讲者备注

讲解提示：本页完全按用户提供的时序图组织：用户提出问题；客户端把问题和可用工具列表交给大语言模型；模型返回工具调用请求；客户端执行工具函数并接收结果；客户端把工具结果交回模型；模型生成最终回答；客户端再把最终答案返回用户。模型返回的是结构化调用请求，真正执行工具的是客户端或应用代码。图中的“客户端”是通用工具调用方，不能直接等同于 MCP Client。<br>
资料：https://www.runoob.com/ai-agent/ai-agent-function-calling.html；所附《MCP.pdf》第2页。天气工具名称、参数和预测结果均为课堂示意，不代表真实天气。

### 第 18 页：2.3  Function Calling 核心概念回顾

2.3  Function Calling 核心概念回顾

四个核心概念，如何落到实际问题

| 核心概念 | 它解决什么问题 | 举例 |
| --- | --- | --- |
| 工具定义 | 描述工具功能、参数与返回值。 | get_weather：查询天气；输入 city、day_offset；返回天气现象、温度、降水概率。 |
| 工具选择 | LLM 根据用户问题选择合适工具。 | “明天会下雨吗？” → 选择 get_weather，而不是普通文本回答。 |
| 参数提取 | 从自然语言中抽取工具所需参数。 | city = 北京；day_offset = 1；并检查必填项与取值范围。 |
| 结果处理 | 将工具结果交回模型，整合进最终回答。 | 降水概率 70% → 模型据此建议“建议带伞”。 |

Function Calling 让模型“说清楚要调用什么”；执行与权限仍由客户端 / 应用代码负责。

#### 演讲者备注

讲解提示：本页对应用户提供的“核心概念”截图，先列出工具定义、工具选择、参数提取、结果处理四个概念，再用同一个天气问题逐行举例。工具定义需要清楚说明用途、参数类型、必填项、范围和返回值；模型根据问题选择工具并产出参数；应用验证参数、调用天气 API，再把结果回填给模型。70% 仍是课堂示意，不是实际天气预报。<br>
资料：https://www.runoob.com/ai-agent/ai-agent-function-calling.html；https://www.runoob.com/ai-agent/tools-integration.html；所附《MCP.pdf》第2–3页。天气工具名称、参数和预测结果均为课堂示意，不代表真实天气。

### 第 19 页：2.4  从直接集成到 MCP

2.4  从直接集成到 MCP

工具增加时，重复接入会变成负担

固定应用、少量工具

AI 应用

天气 API

在应用里直接定义工具和调用逻辑，路径短、实现简单。

多个应用、多个外部服务

应用 A     应用 B

各写工具描述 / 认证 / 错误处理

天气     日历     地图

MCP 将可复用能力封装成 Server，统一发现与调用的协议接口。

#### 演讲者备注

讲解提示：本页是从 Function Calling 过渡到 MCP 的动机：少量固定工具直接集成足够；当多个 AI 应用需要连接天气、日历、地图等服务时，重复实现工具定义、连接、认证、错误处理会增加维护成本。MCP 统一的是 AI 应用与外部服务之间的协议，并不替代模型侧的 Function Calling。MCP Server 也不自动让底层 API 变好，服务仍需设计合理的查询参数和可读结果。<br>
资料：所附《MCP.pdf》第1–4页；https://www.runoob.com/ai-agent/tools-integration.html。天气工具名称、参数和预测结果均为课堂示意，不代表真实天气。

### 第 20 页：2.5  MCP 的角色分工

2.5  MCP 的角色分工

AI 应用管理连接，Server 对应一个外部能力域

AI 应用（MCP Host）

用户交互、模型编排、权限筛选

大语言模型<br>
选择工具与参数

MCP Client<br>
连接 Server

Host 可以管理多个 Client

MCP Server：把外部服务包装成可发现、可调用的能力

天气 MCP Server

get_weather

天气 API：实时预报

日历 MCP Server

find_events

日历 API：查找日程

地图 MCP Server

plan_route

地图 API：规划路线

MCP 协议

右侧的天气、日历、地图则是被不同 Server 封装的外部服务。

#### 演讲者备注

讲解提示：首次解释术语。AI 应用（MCP Host）是面向用户的完整应用，里面可以有模型、编排逻辑和一个或多个 MCP Client；MCP Client 负责连接某一个 MCP Server。右侧三个例子说明 Server 的来源：天气 Server 包装天气 API，日历 Server 包装日历系统，地图 Server 包装路线 API。它们不是 MCP 自带的工具，而是外部能力的协议化封装。Server 负责暴露工具、校验请求并调用底层系统；实际权限仍需单独设计。<br>
资料：所附《MCP.pdf》第1页、第4–5页；https://www.runoob.com/ai-agent/tools-integration.html。天气工具名称、参数和预测结果均为课堂示意，不代表真实天气。

### 第 21 页：2.7  MCP 工具调用

2.7  MCP 工具调用

Function Calling 的调用意图如何变成 MCP 请求

模型输出：Function Calling

```text
name: get_weather
arguments: {
  city: “北京”, day_offset: 1
}
```

//模型只表达“要调用什么”

AI 应用做映射

① 查找对应 Client<br>
② 组装协议消息<br>
③ 发送 tools/call

MCP Client → MCP Server

method: tools/call<br>
name: get_weather<br>
arguments: 北京, 1

Server 才能访问外部天气 API

Function Calling 解决模型侧的结构化表达；MCP tools/call 解决外部服务侧的标准化调用，两者经常连续出现。

因此：有 MCP 仍然可以使用 Function Calling；MCP 不等于模型直接连接 Server。

前置：Client 通过 initialize 建立连接，并用 tools/list 获取工具定义；调用时再发送 tools/call。

#### 演讲者备注

讲解提示：这页不是额外插入一个无关的调用过程，而是专门回答两者如何衔接。模型先通过 Function Calling 产出工具名和参数；AI 应用根据名称选择对应 MCP Client，把意图包装成 MCP 的 tools/call 请求；MCP Server 再校验权限并访问天气 API。成功或错误结果返回后，AI 应用将其交给模型。讲解重点是两条接口边界：Function Calling 面向模型 API，MCP tools/call 面向 Client—Server 协议。<br>
资料：所附《MCP.pdf》第2–5页；https://www.runoob.com/ai-agent/ai-agent-function-calling.html；https://www.runoob.com/ai-agent/tools-integration.html。天气工具名称、参数和预测结果均为课堂示意，不代表真实天气。

### 第 22 页：2.8  两种机制如何接力

2.8  两种机制如何接力

模型侧提出调用，MCP 侧完成标准化访问

大语言模型

AI 应用(MCP Host)

MCP Client

MCP Server

Function Calling

MCP

准备

应用向模型提供工具定义

Client 从 Server 获取工具清单与 inputSchema

调用

模型返回 get_weather(北京, 1)

Client 发送 tools/call

结果

应用回填结果，模型生成回答

Server 返回天气数据或错误

#### 演讲者备注

讲解提示：这一页把同一轮任务中的两段接口拼起来。MCP Client 从 Server 发现 get_forecast 及 inputSchema，Host 将其适配为模型 API 接受的工具定义。模型利用 Function Calling 返回工具名和参数；Host 把它映射为 MCP tools/call。Server 返回天气数据后，Host 再送给模型。不同模型服务商的函数调用格式可能不同，但 MCP Client 与 Server 的通信协议可以保持一致。模型通常不直接连接 Server。<br>
资料：所附《MCP.pdf》第2–5页；https://www.runoob.com/ai-agent/ai-agent-function-calling.html；https://www.runoob.com/ai-agent/tools-integration.html。天气工具名称、参数和预测结果均为课堂示意，不代表真实天气。

### 第 23 页：2.9  MCP vs Tool Function Calling

2.9  MCP vs Tool Function Calling

按标准化、范围、架构、发现与复用性比较

| 特性 | 工具函数调用 | 模型上下文协议（MCP） |
| --- | --- | --- |
| 标准化 | 专有实现；格式随 LLM 提供商而异。 | 开放标准协议；促进 LLM 与工具互操作。 |
| 范围 | LLM 请求执行预定义函数。 | 规范外部能力的发现与通信。 |
| 架构 | 模型与应用内工具处理逻辑交互。 | 客户端—服务器架构；可连接多个 Server。 |
| 发现 | 对话中显式提供可用工具。 | Client 可查询 Server 能力清单。 |
| 复用性 | 接入常与具体应用和模型耦合。 | 独立 Server 可由合规应用复用。 |

#### 演讲者备注

讲解提示：本表忠实沿用教材第3页的五项维度，并为投影版面缩短句子。原表具体表述：标准化——函数调用“专有、因厂商而异；格式与实现随 LLM 提供商不同”，MCP“开放标准化协议，促进不同 LLM 与工具互操作”；范围——“LLM 请求执行特定预定义函数的直接机制”对比“LLM 与外部工具如何发现与通信的更广泛框架”；架构——“LLM 与应用内工具处理逻辑的一对一交互”对比“客户端—服务器架构”；发现——“在特定对话上下文中显式告知 LLM 有哪些工具”对比“客户端可查询 Server 能力清单”；复用性——“集成常与特定应用和 LLM 紧耦合”对比“独立 Server 可由合规应用复用”。讲解时要补充：教材说函数调用“一对一”不表示模型只能调用一个工具；具体 MCP 消息通常由应用中的 MCP Client 发送，模型不直接实现 MCP；发现工具也不自动授予权限。<br>
资料：所附《MCP.pdf》第3页。天气工具名称、参数和预测结果均为课堂示意，不代表真实天气。

### 第 24 页：2.10  选择与总结

2.10  选择与总结

按工具数量、复用需求和接入方式选择

直接集成 Function Calling

适合：一个应用、少量稳定工具。<br>
做法：应用直接实现天气 API 调用、校验和结果回填。

Function Calling + MCP

适合：多个应用要复用、工具来源多且会变化。<br>
做法：复用 MCP Server，Client 按协议发现并调用。

总结：模型用 Function Calling 表达意图；AI 应用用 MCP 接入外部能力。

#### 演讲者备注

讲解提示：回到天气例子：如果只有一个应用查天气，直接 Function Calling 加天气 API 足够；若多个应用共用天气、日历、地图等服务，把能力暴露为 MCP Server 可减少重复对接。MCP Server 的输入输出仍需对模型友好，权限、错误处理和运维仍需设计。两者是常见组合而非替代关系。最后可提问：模型返回 get_forecast(北京,1) 后，哪一层真正调用天气服务？答案是 AI 应用中的执行逻辑；若采用 MCP，则由 Client 向 Server 发 tools/call，再由 Server 调用底层天气 API。<br>
资料：所附《MCP.pdf》第1–5页、第11页；https://www.runoob.com/ai-agent/ai-agent-function-calling.html；https://www.runoob.com/ai-agent/tools-integration.html。天气工具名称、参数和预测结果均为课堂示意，不代表真实天气。

### 第 25 页：MCP的额外考量

MCP的额外考量

PART THREE

03

讲述人：方欣怡

### 第 26 页：1 MCP 的适用性评估

1 MCP 的适用性评估

MCP 为 AI 应用访问外部工具和数据提供了标准化的交互方式，但 MCP 并非适用于所有场景，因此需要从以下四个方面进行综合评估：

一、业务需求：有没有必要使用MCP？

判断业务是否需要 MCP 提供的能力，以及动态发现机制能否带来实际价值。

二、技术可行性：能否满足业务要求？

判断 MCP 方案在安全性、错误处理和可靠性方面能否满足实际业务要求。

三、架构适配性：采用后如何设计？

确定 MCP Server 的部署位置、执行模式和通信机制，使 MCP 与具体业务架构相匹配。

四、实现成本：使用 MCP 是否值得？

比较开发、集成、测试、部署和维护成本，以及采用 MCP 带来的整体收益。

#### 演讲者备注

首先是第一个部分MCP的适用性评估。前面的同学已经介绍了，MCP为AI 应用访问外部工具和数据提供了标准化的交互方式，是一个强大的协议，但MCP 并非适用于所有场景，因此需要从以下四个方面进行综合评估：第一个方面是业务需求，也就是评估业务上是否需要使用MCP；第二个方面是技术可行性，判断MCP方案是否满足业务要求；第三个方面是架构适配性，也就是采用了MCP后具体应该如何设计；第四个方面是实现成本，评估了使用MCP是否值得。

### 第 27 页：1 MCP 的适用性评估：业务需求

1 MCP 的适用性评估：业务需求

维度1：Tool vs. Resource vs. Prompt

MCP 的核心能力是否适合该业务需求？<br>
· Tool：可执行的操作（如查订单、发邮件）<br>
· Resource：为 AI 提供上下文的数据（订单记录、产品文档）<br>
· Prompt：可复用的提示模板（退款引导话术）<br>
- 业务需要 AI 访问外部功能、数据或可复用的任务指引，可以考虑 MCP。<br>
- 若都不需要，直接调用 API 可能更简单。

维度2：Discoverability(可发现性)

· MCP 客户端可以通过协议查询服务端提供的工具、资源和提示模板，从而了解当前可用的能力。<br>
- 当工具数量较多、能力经常变化时，动态发现可以减少客户端维护成本；<br>
- 当工具固定、接口简单时，动态发现的收益可能较小，直接调用 API 可能更简单。

#### 演讲者备注

接下来看第一个方面，业务需求。<br>
维度1是Tool vs. Resource vs. Prompt，这三者是MCP的核心能力。这个维度主要是用来评估：MCP 的核心能力是否适合该业务需求？举例来说，如果业务需要AI‘去查一下订单或者发一封邮件’，这就属于执行动作的工具，使AI能够操作外部系统或服务；如果是让它‘读一下产品文档’，这是资源，也就是为 AI 提供上下文的数据；如果是‘套用退款话术’，这是提示词，帮助结构化与 AI 的交互。如果业务完全不需要这些，那直接写代码调API最简单。<br>
维度2是可发现性，MCP客户端能在运行时动态查询服务端有哪些可用能力。

### 第 28 页：1 MCP 的适用性评估：技术可行性

1 MCP 的适用性评估：技术可行性

维度3：Scrutiny(安全性)

采用 MCP 后，整个解决方案能否满足业务的安全要求？<br>
当 AI 通过 MCP 访问外部工具和数据时，需要评估身份认证、权限控制、敏感数据保护和高风险操作防护等机制是否能够有效落实。

维度4：Error Handling(错误处理)

采用 MCP 后，系统能否正确处理工具调用过程中的异常？<br>
AI 调用外部工具时，可能遇到参数错误、权限拒绝、服务超时或业务执行失败等情况。因此，需要评估系统能否识别错误、清晰反馈错误，并采取合适的后续处理措施。

#### 演讲者备注

第二个方面是技术可行性。<br>
第一是安全性，评估的重点在于整个解决方案能否满足业务的安全需求，这包括了身份认证、权限控制，以及敏感数据和高风险操作的防护。必须清晰地界定：哪些客户端有权限连接哪些服务器，以及它们被允许执行哪些具体的操作。<br>
第二是错误处理。AI调用工具时，难免会遇到参数错误、超时或权限拒绝，因此需要评估系统有没有完善的容错机制。

### 第 29 页：1 MCP 的适用性评估：架构适配性

1 MCP 的适用性评估：架构适配性

维度5：Local vs. Remote Server

采用 MCP 后，Server 如何部署？<br>
· Local Server(本地服务)：<br>
适合低延迟、隔离敏感数据等场景。<br>
· Remote Server(远程服务)：<br>
适合在组织内共享、可扩展地访问工具等场景。

维度6：On-demand vs. Batch

采用 MCP 后，业务应该采用什么执行模式？<br>
· On-demand(按需处理)：<br>
适用于实时查询、对话等场景。<br>
· Batch(批量处理)：<br>
适用于大规模数据分析、定期业务处理等场景。

维度7：Transportation Mechanism

采用 MCP 后，客户端与服务器应通过什么方式通信？<br>
· 本地交互：<br>
通常采用 STDIO 上的 JSON-RPC，用于本地进程间通信。<br>
· 远程连接：<br>
通常采用面向 Web 的通信方式，如流式 HTTP、SSE，维持持久高效的客户端—服务器通道。

#### 演讲者备注

第三个方面是架构适配性，第一是部署，如果处理财务等敏感数据，适合用Local Server；如果是全员共享的天气查询，可以用Remote Server。第二是执行模式，按需处理适用于实时查询、对话等场景，批量处理适合大规模数据分析、定期业务处理等场景。第三是通信机制：本地进程间通信通常用STDIO上的JSON-RPC，跨网络远程连接可以用流式HTTP或者SSE。这都要看实际场景来定。

### 第 30 页：1 MCP 的适用性评估：实现成本

1 MCP 的适用性评估：实现成本

维度8：Implementation(实现)

引入 MCP 的开发与维护成本，是否划算？<br>
✅  值得投入：<br>
· 实现成本较低：有成熟的 SDK（如 FastMCP）<br>
· 易于集成：现有 API / 业务能力可以较低成本封装为 MCP<br>
· 长期复用价值高：一次实现可供多个 AI 应用复用<br>
❌  可能不划算：<br>
· 实现成本较高：需要额外开发、测试、部署和运维体系<br>
· 复用价值有限：只有单一 Client，未来扩展需求较少

#### 演讲者备注

最后一点是评估是否划算。

### 第 31 页：2 MCP 的四大组件

2 MCP 的四大组件

①LLM

核心智能；处理用户请求、制定计划、决定何时调用外部信息或执行动作。

②MCP Client

LLM 的包装层；作为中介，将 LLM 的意图翻译为符合 MCP 的正式请求，负责发现、连接并与 MCP 服务器通信。

③MCP Server

通向外部世界网关；向授权客户端暴露 tools、resources、prompts；通常每服务器负责一域，如内网数据库、邮件服务、公共 API。

④可选的第三方服务

MCP 服务器管理并暴露的实际外部工具、应用或数据源；是最终执行动作的端点，如查专有库、操作 SaaS、调天气 API 等。

#### 演讲者备注

第二部分是MCP的四大核心组件。<br>
首先是大语言模型,它是整个系统的核心智能中枢,负责理解用户的自然语言，制定计划，并判断当前是否需要调用外部工具，以及具体调用哪个工具。<br>
第二是MCP客户端，扮演的是一个通信适配器的角色。它将LLM的意图翻译成符合MCP规范的正式请求，并负责与后端的服务器建立连接、管理和通信。<br>
第三是MCP服务器，它是通向外部世界的网关，负责向授权的客户端暴露tools、resources、prompts。在实际应用中，服务器通常是按领域划分的。<br>
最后是可选的第三方服务，这些是真正被操作的实际外部工具、应用或数据源。

### 第 32 页：3 四大组件的交互流程

3 四大组件的交互流程

交互流程的完整路径从发现能力到结果回传，共五步：

1

发现

MCP 客户端代表 LLM 查询 MCP 服务器的能力清单；服务器返回 tools、resources、prompts。

2

请求构造

LLM 决定使用某个已发现的工具，构造请求，指定工具与参数。

3

客户端通信

客户端将 LLM 的请求翻译为 MCP 标准格式，并转发给 MCP 服务器。

4

服务器执行

服务器收到请求后认证客户端、校验请求，再通过底层软件执行指定动作。

5

响应与上下文更新

执行后，服务器向客户端返回标准化响应，表明成功与否及输出；客户端将结果写入对话上下文，再将更新后的上下文回传 LLM，以继续下一步。

#### 演讲者备注

最后来看这4大组件的交互流程，完整路径从发现能力到结果回传 共有五步：<br>
第一步：发现。客户端在代表大模型执行任务前，会先向服务器查询‘现在能提供哪些能力？’。服务器会返回当前可用的工具、资源和提示词清单。<br>
第二步：请求构造。大模型基于获取到的清单，进行推理决策，决定使用某个已发现的工具，并生成具体的调用参数。<br>
第三步：客户端通信。客户端将LLM的意图，严格翻译成MCP协议的标准格式，并通过底层通信机制发送给对应的MCP 服务器。<br>
第四步：服务器执行。服务器收到请求后，必须进行严格的认证和校验，确认该客户端有权执行此操作。校验通过后，服务器再通过底层软件去调用真正的第三方服务，执行实质性的动作。<br>
第五步：响应与上下文更新。第三方服务执行完毕后，服务器向客户端返回标准化响应，表明成功与否及输出；客户端将结果写入对话上下文，再将更新后的上下文回传给 LLM。通过这种上下文更新机制，大模型才能准确感知执行状态，从而决定是继续后续步骤，还是向用户输出最终结果。<br>
关于MCP的额外考量这部分我就分享到这里，接下来请下一位同学为大家继续讲解实践应用与实例。

### 第 33 页：实践应用于实例

实践应用于实例

PART FOUR

04

讲述人：屈家豪 陈志文

### 第 34 页：用例总览

用例总览

九大用例（上）：数据与内容基础能力

01

数据库集成

自然语言访问并操作结构化数据：查 BigQuery、生成报表、更新记录

02

生成式媒体编排

串联 Imagen(图) / Veo(视频) / Chirp 3 HD(语音) / Lyria(音乐) 动态创作

03

外部 API 交互

标准化调用任意外部 API：拉天气股价、发邮件、操作 CRM

04

基于推理的信息抽取

不整篇返回——精准抽出直接回答复杂问题的条款、图表或陈述

05

自定义工具开发

内部函数/专有系统以标准格式暴露，无需修改模型本身

### 第 35 页：用例总览

用例总览

九大用例（下）：编排与垂直行业

06

LLM-应用通信标准化

一致通信层降低集成成本，促进不同模型与宿主应用互操作

07

复杂工作流编排

取客户数据→生成营销图→起草邮件→发送：跨服务器组合

模式精髓

08

物联网设备控制

自然语言向智能家居、工业传感器、机器人下达指令

09

金融服务自动化

行情分析、交易执行、个性化建议、监管报送，安全且标准化

一句话收束：让智能体获取实时信息、执行动作、整合多源数据完成复杂任务

### 第 36 页：ADK 动手实践

ADK 动手实践

连接本地文件系统 MCP 服务器：本实践三要素

实践目标<br>
让 ADK 智能体读写本地文件系统<br>
关键三要素<br>
· ADK　Agent Development Kit 智能体开发框架<br>
· 服务器　官方现成包 server-filesystem（消费别人写好的）<br>
· 传输　STDIO 上的 JSON-RPC：同机子进程、低延迟

①  编写 agent.py<br>
               在 tools 列表中实例化 MCPToolset

②  创建 __init__.py<br>
               让 ADK 识别为可发现的 Python 包

③  运行 adk web<br>
                 浏览器中选择智能体并对话测试

接入一个现成 MCP 服务器的全部代码，只有十几行——这就是标准化的价值

### 第 37 页：ADK 动手实践

ADK 动手实践

核心代码：MCPToolset 一行注入整个服务器

```text
from google.adk.agents import LlmAgent
from ...mcp_toolset import (
        MCPToolset, StdioServerParameters)

root_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="filesystem_assistant_agent",
    instruction=f"...{TARGET_FOLDER_PATH}",
    tools=[
      MCPToolset(
        connection_params=StdioServerParameters(
          command="npx",
          args=["-y",
            "@modelcontextprotocol/server-filesystem",
            TARGET_FOLDER_PATH]),
    )],
)
```

1

MCPToolset 放进 tools 列表

整个 MCP 服务器的能力被注入智能体，无需逐个注册函数

2

connection_params 走 STDIO

客户端启动服务器子进程，经标准输入输出通信

3

路径必须是绝对路径

它作为智能体文件操作的根目录＝安全边界；再用 tool_filter 可只开放读取

### 第 38 页：ADK 动手实践

ADK 动手实践

三个工程细节：怎么把服务器"跑起来"

npx<br>
Node Package Execute（npm 5.2+ 自带）<br>
直接从 npm 注册表运行 Node 包，无需全局安装。大量社区 MCP 服务器以 Node 包分发，npx 是最常见的启动方式

uvx<br>
基于 uv 的临时隔离运行器<br>
在临时隔离环境中运行 Python 包，不污染全局也不装入项目环境。Python 生态的对应方案，如 mcp-google-sheets

__init__.py<br>
包注册文件<br>
必须与 agent.py 同目录创建（内容：from . import agent），ADK 才会把该目录识别为可发现的 Python 包

变体：command 可为任意启动命令，如 python3 ./mcp_server.py；敏感配置经 env 注入<br>
SERVICE_ACCOUNT_PATH = 服务账号路径    DRIVE_FOLDER_ID = Drive 目录 ID

### 第 39 页：用 ADK Web 运行

用 ADK Web 运行

启动与端到端验证：提示 → 工具调用 → 真实文件

cd ./adk_agent_samples<br>
adk web<br>
adk_agent_samples/<br>
└─ mcp_agent/<br>
   ├─ agent.py<br>
   └─ __init__.py

Web UI 中选择智能体：filesystem_assistant_agent

"Show me the contents of this folder."

"Read the sample.txt file."

"What’s in another_file.md?"

预期行为链：<br>
用户提示 → LLM 决定用文件工具 → STDIO 调用服务器 → 读真实文件 → 结果回填 → 组织回答<br>
这正是五步交互流程（发现→构造→通信→执行→回填）的实证

![课件第 39 页原图 5](assets/pptx/slide-39-image-05.png)

### 第 40 页：这次实践验证了什么

小结

这次实践验证了什么

✓

接入现成服务器只需一行配置

官方服务器的十几个工具由一个 MCPToolset 全部注入，换模型、换应用都可直接复用

✓

本地文件操作用 STDIO 传输

服务器在本机作为子进程运行，延迟低，文件内容不经过网络

✓

安全边界来自配置，不是协议自带

绝对路径限定目录范围，tool_filter 限定工具权限

思考：以上服务器都是"别人写好的"。如果我们想把自己的内部能力暴露成 MCP 服务器呢？<br>
            → 下一部分：用 FastMCP 从零搭建并消费自建 MCP 服务器

### 第 41 页：0  承上启下：从「使用 MCP」到「创造 MCP」

0  承上启下：从「使用 MCP」到「创造 MCP」

前面讲的是怎么用别人的 MCP 服务器；这一部分回答两个问题：怎么造一个自己的服务器，以及造好之后怎么让智能体用起来。

上半场 · 已讲过

使用 MCP：消费现成服务器

现成的 MCP 服务器（如文件系统 Filesystem）已经能让智能体读文件、列目录；你只是使用者。

Consume（消费）

现成服务器

智能体 = 使用者

下半场 · 本部分

创造 MCP：暴露自己的能力

我自己写的 Python 函数——查实验室数据库、算某个指标——想让智能体也能调用，就得自己造一个服务器

Expose（暴露）

自建服务器

你 = 工具定义者

核心问题：我自己的 Python 函数，怎么才能被智能体调用？—— 答案：自己搭一个 MCP 服务器，FastMCP 把这件事变得很简单。

① 是什么

② 服务端

③ 客户端

④ 原理图与判据

### 第 42 页：1  FastMCP 是什么：定位与三大能力

1  FastMCP 是什么：定位与三大能力

FastMCP 不是协议，而是协议之上的一层高级 Python 封装——它把「写 MCP 服务器」变成「写一个普通的 Python 函数」。

高级 Python 框架

面向 MCP 服务器开发，不是协议本身

抽象层

屏蔽协议细节，你只写核心逻辑

进阶架构能力

服务器组合 composition / 代理 proxying

1

声明式：装饰器即注册

在普通 Python 函数上加一行 @mcp_server.tool，它就变成可被 LLM 调用的 MCP 工具；tools / resources / prompts 都用装饰器声明，无需手写协议细节。

@mcp_server.tool

2

自动生成 Schema

FastMCP 解析函数签名、类型注解与 docstring，自动拼装出模型需要的接口描述（Schema），省掉大量手工配置，也避免把字段名写错。

signature + type hints + docstring

3

高级架构模式

除了基础的工具创建，还支持服务器组合 composition 与代理 proxying，面向分布式、可扩展的部署做了优化。

composition / proxying

一句话记住：FastMCP 的价值 = 装饰器声明 + 自动生成 Schema —— 复杂性留在框架里，业务逻辑留给你

### 第 43 页：2  关键一步：普通 Python 函数 → MCP 工具

2  关键一步：普通 Python 函数 → MCP 工具

加一行装饰器，函数就被「注册」成工具；而这份「工具说明书」（Schema），由 FastMCP 从函数签名、类型注解与 docstring 自动推导出来。

fastmcp_server.py（节选）

```text
from fastmcp import FastMCP

mcp_server = FastMCP()

@mcp_server.tool
def greet(name: str) -> str:
    """
    Generates a personalized greeting.
    Args:
        name: The name of the person to greet.
    Returns:
        A greeting string.
    """
    return f"Hello, {name}! Nice to meet you."
```

FastMCP 自动推导出的工具 Schema

来源　def greet(name: str) 函数名

生成　tool name = greet

来源　name: str 类型注解

生成　输入参数 name：string，必填

来源　docstring 文档字符串

生成　description：给模型看的工具说明

docstring 不是可选装饰——它就是给模型看的说明书。写好 Python，就等于写好文档。

### 第 44 页：3  服务端：fastmcp_server.py（三段结构）

3  服务端：fastmcp_server.py（三段结构）

服务器端核心代码只有十几行，可以拆成三段看：准备 → 注册 → 启动。

fastmcp_server.py

```text
from fastmcp import FastMCP

mcp_server = FastMCP()

@mcp_server.tool
def greet(name: str) -> str:
    """Generates a personalized greeting."""
    return f"Hello, {name}! Nice to meet you."

if __name__ == "__main__":
    mcp_server.run(transport="http",
                   host="127.0.0.1", port=8000)
```

运行效果

$ python fastmcp_server.py<br>
[FastMCP] serving on http://127.0.0.1:8000<br>
> greet("John Doe")  ->  "Hello, John Doe! Nice to meet you."

三处关键点

1

@mcp_server.tool

注册中心：一行装饰器就把函数登记成一个 MCP 工具，不需要写 DTO、路由与序列化代码。

2

类型注解 + docstring

自动生成工具 Schema：工具名、参数类型、返回值与语义说明，全部由 FastMCP 推导。

3

run(transport="http")

启动常驻服务，在 127.0.0.1:8000 上监听请求，因此可以被远程的智能体调用。

### 第 45 页：4  客户端：agent.py —— 让 ADK 智能体用起来

4  客户端：agent.py —— 让 ADK 智能体用起来

客户端只要告诉 ADK 两件事：连哪个服务器（HttpServerParameters）、用哪些工具（tool_filter）

adk_agent_samples/fastmcp_client_agent/agent.py

```text
from google.adk.agents import LlmAgent
from google.adk.tools.mcp_tool.mcp_toolset import (
    MCPToolset, HttpServerParameters)

FASTMCP_SERVER_URL = "http://localhost:8000"

root_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="fastmcp_greeter_agent",
    tools=[
        MCPToolset(
            connection_params=HttpServerParameters(
                url=FASTMCP_SERVER_URL),
            tool_filter=["greet"],
        )
    ],
)
```

KEY: client 与 server 端口必须一致 → localhost:8000

三处关键点

1

MCPToolset

ADK 内置的 MCP 客户端 SDK，封装「发现 — 连接 — 调用」全过程。

2

HttpServerParameters

只要声明服务器地址，ADK 就会自动完成握手与能力协商。

3

tool_filter=["greet"]

可选参数：收窄暴露给智能体的工具集合，越少越安全。

连接关系

ADK 智能体<br>
LlmAgent

MCPToolset<br>
HttpServer<br>
Parameters

FastMCP 服务器<br>
:8000

客户端连的是 localhost:8000，正好是服务端监听的地址 —— 两边端口必须一致。

### 第 46 页：5  跑起来：两个终端 + 一个 __init__.py

5  跑起来：两个终端 + 一个 __init__.py

服务端与客户端是两个独立进程——服务必须先启动，客户端才能连上。

1

建包

在 fastmcp_client_agent/ 下创建 __init__.py，让 ADK 能发现这个 agent。

2

终端 A

python fastmcp_server.py —— 启动 FastMCP 服务器，常驻监听 localhost:8000。

3

终端 B

先 cd ./adk_agent_samples，再执行 adk web —— 启动 ADK 的 Web 界面。

4

浏览器

在智能体菜单里选择 fastmcp_greeter_agent，输入「Greet John Doe」来测试。

终端 A · 服务端（先启动）

$ python fastmcp_server.py<br>
[FastMCP] server listening on 127.0.0.1:8000<br>
waiting for requests ...  (Ctrl+C to stop)

终端 B · 客户端 / Web UI

$ cd ./adk_agent_samples<br>
$ adk web<br>
ADK Web UI  ->  http://127.0.0.1:8001<br>
[tool call] greet(name="John Doe")

结果：智能体自动发现并调用 greet 工具，返回「Hello, John Doe! Nice to meet you.」

### 第 47 页：6  原理图：Fig.1 Model Context Protocol（教材 p.149）

6  原理图：Fig.1 Model Context Protocol（教材 p.149）

把前面的代码抽象成一张图：智能体（MCP 客户端）通过 MCP 服务器，接入五类外部能力。

![课件第 47 页原图 6](assets/pptx/slide-47-image-06.png)

图 1  模型上下文协议（Visual summary）

四条主链路

1

User → Prompt → Agent

用户给出提示词，交给智能体。

2

Agent（MCP Client）→ MCP Server

智能体扮演客户端角色，通过 MCP 与服务器通信。

3

MCP Server → 五类外部能力

Tools（我们的 greet 属于这类）/ APIs / Legacy Services / Data Services（网页、外部数据库）/ Wrappers。

4

Agent → Output → User

结果最终回到用户手中。

### 第 48 页：感谢大家批评指正

感谢大家批评指正

THANK YOU FOR YOUR  CRITICISM

## 小组成员与分工

> 依据课件中的目录、署名与分工页整理；按课件分工顺序列出。

- 李思琪：MCP 模式概览，起源、概念与架构

- 代嘉仪：MCP 与工具调用函数的关联、交互机制

- 方欣怡：MCP 的额外考量、局限与优化方向

- 屈家豪：实践应用与举例，场景与案例解析（与陈志文共同负责）

- 陈志文：实践应用与举例，场景与案例解析（与屈家豪共同负责）
