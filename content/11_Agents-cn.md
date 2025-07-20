# AI Agents
# AI 代理

## Overview: From Manual to Autonomous
## 概述：从手动到自主

Most software today requires humans to click through interfaces, or make decisions, or manually execute workflows.
今天的大多数软件都需要人类点击界面、做出决策或手动执行工作流程。

Think of customer service teams manually reviewing refund requests.
想想客户服务团队手动审查退款请求。

We tried to solve this problem with traditional automation tools, but they only work on perfectly predictable workflows, as it relies on a series of if and else statements.
我们试图用传统的自动化工具来解决这个问题，但它们只适用于完全可预测的工作流程，因为它依赖于一系列的 if 和 else 语句。

A lot of business processes though, are complex, unpredictable, and require dynamic decision making.
然而，许多业务流程是复杂的、不可预测的，并且需要动态决策。

Many people define AI agents as LLMs that can take action. Historically, LLMs were about generating a response, agents are about doing.
许多人将 AI 代理定义为可以采取行动的 LLM。从历史上看，LLM 是关于生成响应，而代理是关于执行。

**But from first principles, an AI agent is simply an application that uses an LLM as its decision making engine to execute actions independently.** The key word is independently — it doesn't rely on you to take actions (telling it what to do).
**但从第一性原理来看，AI 代理只是一个使用 LLM 作为其决策引擎来独立执行操作的应用程序。** 关键词是独立——它不依赖于您来采取行动（告诉它该做什么）。

A refund approval AI agent is an application that can take actions (just like all applications) like approving a refund, forwarding it to a human, executing the refund, etc. The only difference is that it is not the human that makes these decisions, it is the LLM.
退款批准 AI 代理是一个可以采取行动的应用程序（就像所有应用程序一样），例如批准退款、将其转发给人工、执行退款等。唯一的区别是，做出这些决定的不是人，而是 LLM。

### Traditional Automation vs. AI Agents
### 传统自动化与 AI 代理

You can build a refund approval process using traditional automation with simple logic such as:
您可以使用传统自动化和简单逻辑来构建退款批准流程，例如：
```
If amount > $500 AND no manager approval THEN reject
如果金额 > 500 美元且未经经理批准，则拒绝
```

With an AI agent, it can review customer's purchase history, considers the reason for return, evaluates the company policy in context, then makes the decision. The agent can recognize workflow completion (refund is processed and customer is notified) and can self-correct (I sent the wrong refund amount, let me issue a correction).
有了 AI 代理，它可以审查客户的购买历史，考虑退货原因，在上下文中评估公司政策，然后做出决定。代理可以识别工作流程的完成（退款已处理并通知客户），并且可以自我纠正（我发送了错误的退款金额，让我发布更正）。

![image](https://github.com/user-attachments/assets/04699a4f-256b-4a74-9986-60e2b550094e)
![图片](https://github.com/user-attachments/assets/04699a4f-256b-4a74-9986-60e2b550094e)


## Why AI Agents Suddenly Exploded
## 为什么 AI 代理突然爆发

1. **Decision making capability:** The rise of reasoning models made AI agents capable of making complex decisions.
1. **决策能力：** 推理模型的兴起使 AI 代理能够做出复杂的决策。
2. **Communication protocols:** The rise of communication protocols like MCP (more on that in the next module) made the process of building complex agents a lot simpler.
2. **通信协议：** 像 MCP 这样的通信协议的兴起（下一模块将详细介绍）使构建复杂代理的过程变得简单得多。

### Microsoft's Agent Definition and Vision
### 微软的代理定义和愿景

The way we define agents at Microsoft, and what we use as our frame of reference to build products and capabilities for customers, is **a system that humans can delegate tasks to, and these tasks will only become more complex over time**.
我们在微软定义代理的方式，以及我们用来为客户构建产品和功能的参考框架，是**一个人类可以委派任务的系统，而这些任务只会随着时间的推移而变得更加复杂**。

**Reasoning:** Because of the rise of reasoning models, agents are now able to do increasingly complex tasks, especially software engineering.
**推理：** 由于推理模型的兴起，代理现在能够完成越来越复杂的任务，尤其是软件工程。

### The Agentic Web
### 代理网络

**Runtime Components:**
**运行时组件：**
- **Reasoning layer:** The core decision-making capability that determines what actions to take
- **推理层：** 决定采取何种行动的核心决策能力
- **Memory:** This is fundamental software engineering. We've explored solutions like RAG and long context windows, and there's ongoing development like Talk agents. The goal is to remember interactions and solve problems the same way it did last time — just like what you would expect from a human you delegate tasks to
- **记忆：** 这是基础软件工程。我们已经探索了像 RAG 和长上下文窗口这样的解决方案，并且还有像 Talk 代理这样的持续开发。目标是记住交互并像上次一样解决问题——就像您期望您委派任务的人一样
- **Tools:** The actual capabilities that allow agents to take actions in the world
- **工具：** 允许代理在世界上采取行动的实际能力

**Protocols:** 
**协议：**
- **MCP (Model Context Protocol):** You need agents to take actions, which means they have to communicate with other systems. MCP is like HTTP for AI agents — it provides a standardized way for agents to interact with different services and tools
- **MCP（模型上下文协议）：** 您需要代理来采取行动，这意味着它们必须与其他系统通信。 MCP 就像 AI 代理的 HTTP——它为代理与不同服务和工具的交互提供了一种标准化的方式
- **A2A (Agent-to-Agent):** Communication protocols that enable agents to work together
- **A2A（代理到代理）：** 使代理能够协同工作的通信协议

## When to Build AI Agents
## 何时构建 AI 代理

### 1. Complex Decision Making
### 1. 复杂决策
When workflows require judgement calls that can't be captured in rules. For instance, a traditional system might automatically approve refunds under $50, but an agent can evaluate a $45 refund request for a product the customer has returned 3 times before.
当工作流程需要无法用规则捕捉的判断时。例如，传统系统可能会自动批准 50 美元以下的退款，但代理可以评估客户之前已退货 3 次的产品的 45 美元退款请求。

### 2. Unmaintainable Rule System
### 2. 无法维护的规则系统
If your rule system has grown to hundreds of conditional statements.
如果您的规则系统已发展到数百个条件语句。

### 3. Unstructured Data Workflows
### 3. 非结构化数据工作流
If you are processing documents, emails, or natural language. For instance, insurance claim agents can read accident reports, medical records, etc. Something impossible with traditional parsing.
如果您正在处理文档、电子邮件或自然语言。例如，保险理赔代理可以阅读事故报告、医疗记录等。这在传统解析中是不可能的。

### Economic Calculation
### 经济计算
Agents of course cost more per operation than traditional automation, but they eliminate the human labor required for complex decisions. If you're paying people $50/hour to review cases that an agent can handle for $2 per case, and you process 1000 cases monthly, you save $48,000 per month minus $2000 in agent costs.
代理每次操作的成本当然比传统自动化高，但它们消除了复杂决策所需的人力。如果您付给人们 50 美元/小时来审查代理可以以每例 2 美元处理的案件，并且您每月处理 1000 个案件，那么您每月可以节省 48,000 美元，减去 2000 美元的代理成本。

## Components of Agents
## 代理的组成部分

### 1. Model (The Brain)
### 1. 模型（大脑）
This is the brain. Different models have different capabilities and different costs. Always build your prototype with the most capable model to establish a performance baseline, set up evaluation metrics (accuracy and task completion rates), and replace with smaller models where acceptable results are maintained.
这是大脑。不同的模型具有不同的功能和不同的成本。始终使用最强大的模型构建您的原型，以建立性能基准，设置评估指标（准确率和任务完成率），并在保持可接受结果的情况下用较小的模型替换。

Usually, larger models are used for complex decision points. As an example, use an SLM for simple data retrieval (get customer order history) but an LLM for refund decisions (should we approve this $500 refund for a customer who's returned 5 items this month).
通常，较大的模型用于复杂的决策点。例如，使用 SLM 进行简单的数据检索（获取客户订单历史记录），但使用 LLM 进行退款决策（我们是否应该批准这位本月已退货 5 件商品的客户的 500 美元退款）。

### 2. Tools
### 2. 工具
They could be data tools like database queries, they could be communication tools like sending emails or data modification (updating a CRM record), or could be orchestration tools like calling another agent. Each tool needs standardized definitions in your code with clear parameters and return values. Poor tool documentation causes agents to misuse APIs.
它们可以是像数据库查询这样的数据工具，也可以是像发送电子邮件或数据修改（更新 CRM 记录）这样的通信工具，或者可以是像调用另一个代理这样的编排工具。每个工具都需要在您的代码中进行标准化定义，并具有清晰的参数和返回值。糟糕的工具文档会导致代理滥用 API。

**Note:** You might have heard terms like function calling. The difference between function calling and tools, is that tools are the actual implementation (like the python functions that do the work) while function calling is the LLM's ability to tell your app "I need access to the weather function". You need both in an agent, but Semantic Kernel abstracts away function calling so they can appear to be the same thing.
**注意：** 您可能听说过诸如函数调用之类的术语。函数调用和工具之间的区别在于，工具是实际的实现（例如执行工作的 python 函数），而函数调用是 LLM 告诉您的应用程序“我需要访问天气函数”的能力。您在代理中需要两者，但语义内核抽象了函数调用，因此它们看起来是同一回事。

**Example:**
**示例：**
```python
@function_tool
def process_refund(customer_id: str, order_id: str, amount: float, reason: str):
    """
    Process a refund for a customer order.
    为客户订单处理退款。
    
    Args:
        customer_id: Unique customer identifier
        customer_id：唯一的客户标识符
        order_id: Order to refund
        order_id：要退款的订单
        amount: Refund amount in dollars (must be <= original order total)
        amount：退款金额（美元）（必须 <= 原始订单总额）
        reason: Business reason for refund (required for accounting)
        reason：退款的业务原因（会计要求）
    
    Returns:
        {"success": bool, "refund_id": str, "message": str}
    """
    # Implementation here
    # 此处为实现
    return {"success": True, "refund_id": "REF123", "message": "Refund processed"}
```

### 3. Instructions
### 3. 指示
This defines how agent behaves, what it's allowed to do, and how it should handle edge cases. Instead of writing new procedures, adapt your current policies. If your customer service team follows a 12-step refund process, that becomes your agent's instruction set.
这定义了代理的行为方式、允许做什么以及如何处理边缘情况。不要编写新的程序，而是调整您当前的策略。如果您的客户服务团队遵循 12 步退款流程，那么这将成为您代理的指令集。

Capture edge cases as well like: What if customer provides partial information? What if the API call fails? What if the customer asks something outside your domain?
还要捕获边缘情况，例如：如果客户提供部分信息怎么办？如果 API 调用失败怎么办？如果客户问了一些超出您领域范围的问题怎么办？

## Orchestrator
## 编排器

Once you have an agent with a model, tools, and instructions, you need to orchestrate these workflows from start to finish. A single LLM can't handle multi-step processes that require checking conditions, calling multiple tools, and adapting based on results.
一旦您拥有一个带有模型、工具和指令的代理，您就需要从头到尾地编排这些工作流程。单个 LLM 无法处理需要检查条件、调用多个工具并根据结果进行调整的多步骤流程。

### Single Agent Systems
### 单代理系统
Easier to build, and typically relies on prompt templates, where instead of creating separate agents for different use cases, you use a single flexible prompt that accepts variables.
更易于构建，并且通常依赖于提示模板，其中您不为不同的用例创建单独的代理，而是使用一个接受变量的灵活提示。

### Multiple Agent Systems
### 多代理系统
Used when complex logic is required and when the number of tools become large. There are two main patterns:
当需要复杂逻辑以及工具数量变大时使用。主要有两种模式：

1. **Hub and Spoke:** A central manager agent coordinates specialized worker agents
1. **中心辐射型：** 一个中央管理代理协调专门的工作代理
2. **Decentralized Pattern (Peer to Peer):** There is no central coordinator
2. **去中心化模式（点对点）：** 没有中央协调器

The former is used when you need to synthesize the results from multiple agents or when you need one agent to maintain conversation context with a user.
前者用于需要综合来自多个代理的结果或需要一个代理来维护与用户的对话上下文时。

## Handling Memory
## 处理内存

One of the biggest challenges in building production AI agents is managing memory. We already explored how we solve the problem of conversation memory, such as summarizing past conversations, or even using RAG for more long-term storage. Remember, LLMs are stateless — they don't remember anything between API calls as every request is independent.
构建生产 AI 代理的最大挑战之一是管理内存。我们已经探讨了如何解决对话内存问题，例如总结过去的对话，甚至使用 RAG 进行更长期的存储。请记住，LLM 是无状态的——它们在 API 调用之间不记得任何事情，因为每个请求都是独立的。

The other type of memory is **learning memory**. Ideally, we want the agent to learn from all past interactions so it becomes better. An example of how this is implemented is using RAG, so we store successful resolution examples and retrieve them for similar cases.
另一种类型的内存是**学习内存**。理想情况下，我们希望代理从所有过去的交互中学习，以便变得更好。如何实现这一点的一个例子是使用 RAG，因此我们存储成功的解决方案示例，并在类似情况下检索它们。

## Agentic RAG: Beyond Traditional Retrieval
## 代理 RAG：超越传统检索

We already explored a traditional RAG system. The user sends the prompt, we check the vector store for data related to the prompt, that is combined with the prompt and sent to the LLM. LLM in this case literally just summarizes the answer, nothing more.
我们已经探讨了传统的 RAG 系统。用户发送提示，我们检查向量存储中与提示相关的数据，该数据与提示相结合并发送给 LLM。在这种情况下，LLM 实际上只是总结答案，仅此而已。

But LLMs have much more capabilities than just summarizing the prompt & the relevant chunks retrieved from the vector store. **Agentic RAG** is a more sophisticated RAG implementation, where when the user asks a question, the agent analyzes the question, then decides the retrieval strategy (maybe I need to bring data from vector database 2 and 3), then reasons on top of the retrieved information, then retrieves more, until they get to the final answer. So it is a more sophisticated form of RAG.
但是 LLM 的功能远不止总结提示和从向量存储中检索到的相关块。**代理 RAG** 是一种更复杂的 RAG 实现，当用户提出问题时，代理会分析问题，然后决定检索策略（也许我需要从向量数据库 2 和 3 中获取数据），然后在检索到的信息之上进行推理，然后检索更多，直到得到最终答案。所以它是一种更复杂的 RAG 形式。

## Guardrails
## 护栏

AI agents can take actions that affect real customers, they can access sensitive data, and they can represent your brand. Unlike chatbots that only generate text, agents need protection against dangerous output actions.
AI 代理可以采取影响真实客户的行动，它们可以访问敏感数据，并且可以代表您的品牌。与只生成文本的聊天机器人不同，代理需要保护免受危险的输出操作。

You need a balance. Overly restrictive guardrails can block legitimate user requests. But the business reality is that a single leaked customer database or an unauthorized $10k refund can cost more than months of guardrail development.
你需要一个平衡。过于严格的护栏可能会阻止合法的用户请求。但商业现实是，一个泄露的客户数据库或一笔未经授权的 1 万美元退款，其成本可能超过数月的护栏开发成本。

### Types of Guardrails
### 护栏类型

1. **Relevance Classifier:** This keeps agents working within their intended scope. So it prevents customer service agents from answering random questions instead of solving customer problems. SLMs are typically used to classify the input relevance.
1. **相关性分类器：** 这可以使代理在其预期范围内工作。因此，它可以防止客户服务代理回答随机问题，而不是解决客户问题。 SLM 通常用于对输入相关性进行分类。

2. **Safety Classifier:** Detects prompt injection and jailbreak attempts. Typically, fine-tuned models trained on prompt injection patterns are used.
2. **安全分类器：** 检测提示注入和越狱尝试。通常，使用在提示注入模式上训练的微调模型。

3. **PII Filter:** Prevents accidental exposure of PII information.
3. **PII 过滤器：** 防止意外泄露 PII 信息。

4. **Moderation Filter:** Blocks harmful content including hate speech or harassment. OpenAI has moderation APIs that can be used.
4. **审核过滤器：** 阻止有害内容，包括仇恨言论或骚扰。 OpenAI 具有可用的审核 API。

5. **Human in the Loop:** For low or medium risk tasks (like sending emails, updating customer records) that might not need human intervention. Human approval can be used for high risk like processing refunds or cancelling orders.
5. **人在环路中：** 对于可能不需要人工干预的低风险或中等风险任务（例如发送电子邮件、更新客户记录）。对于处理退款或取消订单等高风险任务，可以使用人工批准。

6. **Rule Based Protection:** This is to quickly block known threats. Like SQL injection filters, input length limit, character encoding filters (prevent unicode based attacks).
6. **基于规则的保护：** 这是为了快速阻止已知威胁。例如 SQL 注入过滤器、输入长度限制、字符编码过滤器（防止基于 unicode 的攻击）。

7. **Output Validation:** Ensure agent responses align with brand guidelines and business policies. Things like tone analysis, policy compliance, legal compliance, etc.
7. **输出验证：** 确保代理响应符合品牌准则和业务策略。例如语气分析、策略合规性、法律合规性等。

### Strategy for Building Guardrails
### 构建护栏的策略

**Phase 1:** Focus on data privacy and content safety. These are highest impact risks that can cause immediate business damage (PII exposure, prompt injection, harmful content that damages brand image).
**第一阶段：** 专注于数据隐私和内容安全。这些是可能造成直接业务损害的最高影响风险（PII 泄露、提示注入、损害品牌形象的有害内容）。

**Phase 2:** Add guardrails based on real-world failures. Monitor your deployed agent and implement guardrails for actual problems encountered like user attempts to get free products through social engineering.
**第二阶段：** 根据实际故障添加护栏。监控您部署的代理，并为遇到的实际问题实施护栏，例如用户试图通过社交工程获得免费产品。

**Phase 3:** Optimize for security and UX. Reduce false positives that block legitimate requests, customize guardrail sensitivity based on user context (VIP customers get different treatment).
**第三阶段：** 优化安全性和用户体验。减少阻止合法请求的误报，根据用户上下文自定义护栏敏感度（VIP 客户获得不同的待遇）。

### Human Intervention
### 人工干预
Guardrails are great, but agents at the moment still need human in the loop. Even for fully autonomous agents, you need humans to be available just in case, and this is triggered when you exceed failure thresholds or for high-risk actions.
护栏很棒，但目前代理仍然需要人工干预。即使对于完全自主的代理，您也需要有人以防万一，这在您超过故障阈值或执行高风险操作时触发。

## Building Agents from scratch vs using a "managed" Agent such as Azure AI Agent
## 从头开始构建代理与使用“托管”代理（例如 Azure AI 代理）

You can build AI agents using Semantic Kernel from scratch, where you manage things like tool access (you write the code yourself) or memory. Or you can use Azure AI Agent which is a managed service. It is infrastructure that runs on Azure. Microsoft manages the servers, persistent storage (things like file storage, conversation threads, and vector databases are managed server-side), built-in tools like code interpreter all running on Microsoft's secure environment, and enterprise integration to AI search, functions, etc.
您可以使用语义内核从头开始构建 AI 代理，在其中管理工具访问（您自己编写代码）或内存等。或者您可以使用 Azure AI 代理，它是一种托管服务。它是在 Azure 上运行的基础设施。微软管理服务器、持久存储（文件存储、对话线程和向量数据库等都在服务器端管理）、代码解释器等内置工具都在微软的安全环境中运行，以及与 AI 搜索、函数等的企业集成。

You can certainly use Semantic Kernel with standard LLM APIs but you need to manage conversation state management, tool execution & security, scaling, vector databases, etc. Azure AI Agent can handle all that for you but you may be concerned about higher costs or vendor lock-in. Of course, Semantic Kernel can be used to connect to Azure AI Agent service. 
您当然可以将语义内核与标准 LLM API 结合使用，但您需要管理对话状态管理、工具执行和安全性、扩展、向量数据库等。Azure AI 代理可以为您处理所有这些，但您可能会担心更高的成本或供应商锁定。当然，语义内核可以用来连接 Azure AI 代理服务。