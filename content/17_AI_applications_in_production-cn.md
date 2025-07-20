# Building Production AI Applications
# 构建生产 AI 应用程序

So far, we have explored all of the important concepts when it comes to building AI applications.
到目前为止，我们已经探讨了构建 AI 应用程序时所有重要的概念。

This section of the course will focus on what it takes to build AI applications in real production environments and some best practices.
本课程的这一部分将重点介绍在实际生产环境中构建 AI 应用程序需要什么以及一些最佳实践。

## 1. Guardrails: Protecting Your AI System
## 1. 护栏：保护您的 AI 系统

We already explained previously that your AI applications might be susceptible to malicious attacks using AI, with things like prompt injection or jailbreaking. Your users might expose private information that you don't want to leak. Note that many third-party APIs (like OpenAI) provide many guardrails out of the box for you.
我们之前已经解释过，您的 AI 应用程序可能容易受到使用 AI 的恶意攻击，例如提示注入或越狱。您的用户可能会泄露您不想泄露的私人信息。请注意，许多第三方 API（如 OpenAI）为您提供了许多开箱即用的护栏。

### Input Guardrails: First Line of Defense
### 输入护栏：第一道防线

These protect against leaking private information to external APIs and executing bad prompts that might compromise your system. There are many tools that can automatically detect sensitive data (specified by you) which usually use AI and you can block that from being sent to the model.
这些可以防止将私人信息泄露给外部 API 以及执行可能危及您系统的恶意提示。有许多工具可以自动检测（由您指定的）敏感数据，这些工具通常使用 AI，您可以阻止这些数据发送到模型。

**Real Example - Healthcare Platform:** A medical AI assistant needed to prevent PHI (Protected Health Information) from being sent to external LLM APIs:
**真实示例 - 医疗保健平台：** 一个医疗 AI 助手需要防止 PHI（受保护的健康信息）被发送到外部 LLM API：

**Implementation:**
**实施：**
- Built custom NER (Named Entity Recognition) models to detect patient names, SSNs, medical record numbers
- 构建了自定义 NER（命名实体识别）模型来检测患者姓名、SSN、病历号
- Used regex patterns for structured data like phone numbers and addresses
- 对电话号码和地址等结构化数据使用正则表达式模式
- Implemented keyword blocking for medication names and diagnosis codes
- 对药物名称和诊断代码实施了关键字阻止
- Set up real-time scanning with 50ms latency requirement
- 设置了具有 50 毫秒延迟要求的实时扫描

**Detection Rules:**
**检测规则：**
- Social Security Numbers: `\d{3}-\d{2}-\d{4}` pattern matching
- 社会安全号码：`\d{3}-\d{2}-\d{4}` 模式匹配
- Patient IDs: Custom format recognition for hospital-specific numbering
- 患者 ID：针对医院特定编号的自定义格式识别
- Medical terms: Blocked 2,847 specific medication names and 1,203 diagnosis codes
- 医学术语：阻止了 2,847 种特定药物名称和 1,203 种诊断代码
- Personal identifiers: Names cross-referenced against patient database
- 个人标识符：与患者数据库交叉引用的姓名

**Results:** Prevented 234 PHI leaks in first month, reduced compliance risk, enabled use of external AI while maintaining HIPAA compliance.
**结果：** 第一个月防止了 234 次 PHI 泄露，降低了合规风险，在保持 HIPAA 合规性的同时启用了外部 AI 的使用。

**Common Input Guardrail Categories:**
**常见的输入护栏类别：**
- **PII Detection:** Names, addresses, phone numbers, email addresses, SSNs
- **PII 检测：** 姓名、地址、电话号码、电子邮件地址、SSN
- **Financial Data:** Credit card numbers, account numbers, routing numbers
- **财务数据：** 信用卡号、帐号、路由号码
- **Corporate Secrets:** Product names, financial figures, strategic plans
- **公司机密：** 产品名称、财务数据、战略计划
- **Prompt Injection:** Attempts to override system instructions
- **提示注入：** 试图覆盖系统指令
- **Malicious Content:** Attempts to generate harmful or illegal content
- **恶意内容：** 试图生成有害或非法内容

### Output Guardrails: Quality Control for AI Responses
### 输出护栏：AI 响应的质量控制

This is to catch outputs that fail to meet your standards. Like a model returning inappropriate information or even hateful or illegal information. One thing to note is that output guardrails might not work well in streaming completion mode.
这是为了捕获不符合您标准的输出。例如模型返回不当信息，甚至是仇恨或非法信息。需要注意的一点是，输出护栏在流式完成模式下可能效果不佳。

**Real Example - Customer Service AI:** A telecommunications company implemented output filtering for their customer service chatbot:
**真实示例 - 客户服务 AI：** 一家电信公司为其客户服务聊天机器人实施了输出过滤：

**Multi-Layer Filtering:**
**多层过滤：**
1. **Content Safety:** Used Azure Content Safety API to detect hate speech, violence, self-harm content
1. **内容安全：** 使用 Azure 内容安全 API 检测仇恨言论、暴力、自残内容
2. **Brand Compliance:** Custom classifier trained on 10,000 examples of appropriate vs. inappropriate responses
2. **品牌合规性：** 在 10,000 个适当与不当响应的示例上训练的自定义分类器
3. **Accuracy Validation:** RAG citation checker to ensure responses are grounded in knowledge base
3. **准确性验证：** RAG 引文检查器，以确保响应基于知识库
4. **Tone Analysis:** Sentiment analysis to ensure responses maintain professional, helpful tone
4. **语气分析：** 情绪分析，以确保响应保持专业、有益的语气

**Implementation Details:**
**实施细节：**
- Real-time processing with 80ms average latency
- 实时处理，平均延迟 80 毫秒
- Confidence thresholds: Block responses with >70% toxicity score
- 置信度阈值：阻止毒性得分 >70% 的响应
- Fallback responses: Pre-written safe responses when AI output is blocked
- 后备响应：当 AI 输出被阻止时，预先编写的安全响应
- Human escalation: Flagged conversations where multiple responses were blocked
- 人工升级：标记多个响应被阻止的对话

**Results:** 
**结果：**
- Reduced inappropriate responses by 94%
- 不当响应减少了 94%
- Customer satisfaction increased from 3.2 to 4.1 (out of 5)
- 客户满意度从 3.2 提高到 4.1（满分 5 分）
- Prevented potential brand damage incidents
- 防止了潜在的品牌损害事件
- Saved estimated $1.2M in reputation management costs
- 估计节省了 120 万美元的声誉管理成本

**Streaming Challenge:** Output guardrails are harder to implement with streaming responses because you can't analyze the complete response before sending it. Solutions include:
**流式处理挑战：** 输出护栏在流式响应中更难实现，因为您无法在发送之前分析完整的响应。解决方案包括：
- Sentence-level filtering (check each sentence as it's generated)
- 句子级过滤（在生成时检查每个句子）
- Sliding window analysis (analyze last N tokens continuously)
- 滑动窗口分析（连续分析最后 N 个令牌）
- Conservative settings (higher blocking thresholds for streaming)
- 保守设置（流式处理的更高阻止阈值）

## 2. Model Router and Gateway: Intelligent Traffic Management
## 2. 模型路由器和网关：智能流量管理

### Router: The Right Model for the Right Task
### 路由器：为正确的任务选择正确的模型

This is a common pattern in AI applications. Instead of using one model for all queries, you use different ones. In practice, and speaking from experience, most AI applications use different models for different tasks. This can help save costs where you can use a less expensive model for simpler queries.
这是 AI 应用程序中的一种常见模式。您不使用一个模型来处理所有查询，而是使用不同的模型。在实践中，根据经验，大多数 AI 应用程序针对不同的任务使用不同的模型。这有助于节省成本，您可以使用较便宜的模型来处理较简单的查询。

A router consists of logic that predicts what the user is trying to do, and based on that intent, the query is routed to the appropriate model (again that typically uses AI). There are some out of the box, or you can build your own with a small language model.
路由器包含预测用户意图的逻辑，并根据该意图将查询路由到适当的模型（通常也使用 AI）。有一些开箱即用的路由器，您也可以使用小型语言模型构建自己的路由器。

**Real Example - E-commerce Platform:** An online retailer built a sophisticated routing system for their customer queries:
**真实示例 - 电子商务平台：** 一家在线零售商为其客户查询构建了一个复杂的路由系统：

**Intent Classification:**
**意图分类：**
- **Simple Questions** (43% of queries): "What's my order status?" → Route to fast, cheap model (GPT-3.5)
- **简单问题**（43% 的查询）：“我的订单状态是什么？” → 路由到快速、廉价的模型 (GPT-3.5)
- **Product Recommendations** (31% of queries): "Find me a laptop for gaming" → Route to specialized RAG system
- **产品推荐**（31% 的查询）：“给我找一台适合玩游戏的笔记本电脑” → 路由到专门的 RAG 系统
- **Complex Support** (18% of queries): "I need to return this but the website won't let me" → Route to premium model (GPT-4) + human escalation
- **复杂支持**（18% 的查询）：“我需要退货，但网站不允许” → 路由到高级模型 (GPT-4) + 人工升级
- **Technical Questions** (8% of queries): "How do I set up this router?" → Route to technical documentation model
- **技术问题**（8% 的查询）：“如何设置此路由器？” → 路由到技术文档模型

**Router Implementation:**
**路由器实施：**
- Built lightweight classifier using 500-parameter model (inference cost: $0.0001 per query)
- 使用 500 参数模型构建轻量级分类器（推理成本：每次查询 0.0001 美元）
- Training data: 50,000 labeled customer queries from support tickets
- 训练数据：来自支持工单的 50,000 个标记的客户查询
- Real-time prediction with 25ms latency
- 实时预测，延迟 25 毫秒
- Confidence thresholds: Route to premium model if classification confidence <85%
- 置信度阈值：如果分类置信度 <85%，则路由到高级模型

**Cost Impact:**
**成本影响：**
- Before routing: Average $0.012 per query (all queries to GPT-4)
- 路由前：平均每次查询 0.012 美元（所有查询都发送到 GPT-4）
- After routing: Average $0.004 per query (67% cost reduction)
- 路由后：平均每次查询 0.004 美元（成本降低 67%）
- Monthly savings: $89,000 with same or better response quality
- 每月节省：89,000 美元，响应质量相同或更好
- Customer satisfaction improved due to faster responses for simple queries
- 由于简单查询的响应速度更快，客户满意度得到提高

**Advanced Routing Strategies:**
**高级路由策略：**
- **Load-based routing:** Route to different models based on current API load
- **基于负载的路由：** 根据当前 API 负载路由到不同的模型
- **User-tier routing:** Premium customers get better models
- **用户分层路由：** 高级客户获得更好的模型
- **Context-aware routing:** Route based on conversation history and complexity
- **上下文感知路由：** 根据对话历史和复杂性进行路由
- **A/B testing integration:** Route percentage of traffic to experimental models
- **A/B 测试集成：** 将一定比例的流量路由到实验模型

### Gateway: Unified Model Management
### 网关：统一模型管理

This allows you to connect to different models in a secure manner. It is a unified interface to different models, which makes it easy to maintain your code. You can typically apply throttling at the gateway level, track usage, etc.
这使您能够以安全的方式连接到不同的模型。它是不同模型的统一接口，便于维护代码。您通常可以在网关级别应用限制、跟踪使用情况等。

**Real Example - Financial Services Company:** A investment firm built a model gateway to manage 12 different AI models across their organization:
**真实示例 - 金融服务公司：** 一家投资公司构建了一个模型网关来管理其组织内的 12 个不同 AI 模型：

**Gateway Architecture:**
**网关架构：**
- **Single API Endpoint:** All applications connect to one URL regardless of underlying model
- **单一 API 端点：** 所有应用程序都连接到一个 URL，而不管底层模型如何
- **Authentication:** JWT tokens with role-based model access
- **身份验证：** 具有基于角色的模型访问权限的 JWT 令牌
- **Rate Limiting:** Different limits for different user tiers and models
- **速率限制：** 针对不同用户层和模型的不同限制
- **Model Abstraction:** Same request format works with OpenAI, Anthropic, Google, and internal models
- **模型抽象：** 相同的请求格式适用于 OpenAI、Anthropic、Google 和内部模型

**Features Implemented:**
**实现的功能：**
- **Automatic Failover:** If primary model is down, automatically route to backup
- **自动故障转移：** 如果主模型出现故障，则自动路由到备份模型
- **Usage Tracking:** Track costs per department, user, and model
- **使用情况跟踪：** 按部门、用户和模型跟踪成本
- **Request Logging:** Store all requests/responses for audit and training
- **请求日志记录：** 存储所有请求/响应以供审计和培训
- **Model Versioning:** Support multiple versions of same model with gradual rollouts
- **模型版本控制：** 支持同一模型的多个版本，并逐步推出

**Business Impact:**
**业务影响：**
- **Developer Productivity:** Reduced integration time for new models from 2 weeks to 2 hours
- **开发人员生产力：** 将新模型的集成时间从 2 周减少到 2 小时
- **Cost Management:** Centralized billing and usage analytics across all teams
- **成本管理：** 所有团队的集中计费和使用情况分析
- **Compliance:** Single point for audit logging and security controls
- **合规性：** 审计日志记录和安全控制的单点
- **Reliability:** 99.7% uptime with automatic failover (vs 94% with direct API calls)
- **可靠性：** 自动故障转移可实现 99.7% 的正常运行时间（而直接 API 调用为 94%）

**Technical Implementation:**
**技术实施：**
```python
# Example gateway request - same format regardless of model
# 示例网关请求 - 无论模型如何，格式都相同
POST /gateway/v1/chat/completions
{
    "model": "best-for-finance",  # Gateway resolves to actual model
                                  # 网关解析为实际模型
    "messages": [...],
    "user_tier": "premium",       # Affects routing and rate limits
                                  # 影响路由和速率限制
    "department": "trading"       # For cost tracking
                                  # 用于成本跟踪
}
```

## 3. Caching: Speed and Cost Optimization
## 3. 缓存：速度和成本优化

Caching is not new in software development. It has been used to reduce latency and cost (to avoid constant database querying). There are different caching techniques when it comes to AI.
缓存在软件开发中并不新鲜。它已被用于减少延迟和成本（以避免持续的数据库查询）。在 AI 方面有不同的缓存技术。

### Exact Caching: Perfect Matches
### 精确缓存：完全匹配

For instance, if a user asks the model to summarize a product, the system checks the cache to see if a summary of this exact product exists. This is used to avoid constant vector search. This can be implemented using in-memory storage like Redis.
例如，如果用户要求模型总结一个产品，系统会检查缓存中是否存在该确切产品的摘要。这用于避免持续的向量搜索。这可以使用像 Redis 这样的内存存储来实现。

**Real Example - News Aggregation Platform:** A news website implemented exact caching for article summaries:
**真实示例 - 新闻聚合平台：** 一家新闻网站为其文章摘要实施了精确缓存：

**Implementation:**
**实施：**
- **Cache Key:** SHA-256 hash of article URL + summary length parameter
- **缓存键：** 文章 URL 的 SHA-256 哈希 + 摘要长度参数
- **Storage:** Redis cluster with 72 hours TTL (news becomes stale quickly)
- **存储：** 具有 72 小时 TTL 的 Redis 集群（新闻很快就会过时）
- **Cache Size:** 500GB storing ~2M article summaries
- **缓存大小：** 500GB，存储约 200 万篇文章摘要
- **Hit Rate:** 67% of summary requests served from cache
- **命中率：** 67% 的摘要请求从缓存中提供

**Performance Impact:**
**性能影响：**
- **Latency:** Cache hits respond in 15ms vs. 2.3 seconds for AI generation
- **延迟：** 缓存命中响应时间为 15 毫秒，而 AI 生成为 2.3 秒
- **Cost Savings:** $23,000/month saved on LLM API calls
- **成本节约：** 每月在 LLM API 调用上节省 23,000 美元
- **User Experience:** Instant loading for popular articles
- **用户体验：** 热门文章即时加载
- **Scalability:** Handled 10x traffic spike during major news events
- **可扩展性：** 在重大新闻事件期间处理了 10 倍的流量高峰

**Cache Strategy:**
**缓存策略：**
- Proactive caching for trending articles (cache before users request)
- 对热门文章进行主动缓存（在用户请求之前缓存）
- Intelligent eviction: Remove summaries for articles with <10 views
- 智能驱逐：删除浏览量 <10 的文章摘要
- Geographic distribution: Cache popular articles in multiple regions
- 地理分布：在多个区域缓存热门文章

### Semantic Caching: Similar Intent Matching
### 语义缓存：相似意图匹配

Cached items are used even if they are semantically similar (not identical query). This requires a vector database to store embeddings of cached queries so it is more complicated and is compute intensive. The speed and cost might not be worth it.
即使语义相似（查询不完全相同），也会使用缓存项。这需要一个向量数据库来存储缓存查询的嵌入，因此它更复杂且计算密集。速度和成本可能不值得。

**Real Example - Legal Research Platform:** A legal AI assistant implemented semantic caching for case law research:
**真实示例 - 法律研究平台：** 一个法律 AI 助手为判例法研究实施了语义缓存：

**Challenge:** Lawyers often ask similar questions in different ways:
**挑战：** 律师经常以不同的方式提出类似的问题：
- "What are the precedents for breach of contract in California?"
- “加利福尼亚州违约的先例是什么？”
- "Show me California case law on contract violations"
- “向我展示加利福尼亚州关于合同违规的判例法”
- "Find breach of contract cases from CA courts"
- “从加州法院查找违约案件”

**Implementation:**
**实施：**
- **Vector Storage:** Pinecone database storing query embeddings
- **向量存储：** 存储查询嵌入的 Pinecone 数据库
- **Similarity Threshold:** Cache hit if cosine similarity >0.85
- **相似度阈值：** 如果余弦相似度 >0.85，则缓存命中
- **Embedding Model:** text-embedding-ada-002 for consistent representations
- **嵌入模型：** 用于一致表示的 text-embedding-ada-002
- **Cache Scoring:** Weighted by query popularity and recency
- **缓存评分：** 按查询受欢迎程度和新近度加权

**Performance Analysis:**
**性能分析：**
- **Cache Hit Rate:** 34% of queries matched semantically similar previous queries
- **缓存命中率：** 34% 的查询与语义相似的先前查询匹配
- **Cost per Hit:** $0.15 (embedding lookup + vector search)
- **每次命中成本：** 0.15 美元（嵌入查找 + 向量搜索）
- **Cost per Miss:** $3.20 (full legal research with RAG)
- **每次未命中成本：** 3.20 美元（使用 RAG 进行全面法律研究）
- **Break-even:** Semantic caching profitable with >4.6% hit rate
- **盈亏平衡：** 语义缓存的命中率 >4.6% 时才有利可图

**Results:**
**结果：**
- Average response time reduced from 8.2 seconds to 1.7 seconds for cache hits
- 缓存命中的平均响应时间从 8.2 秒减少到 1.7 秒
- 31% reduction in overall compute costs
- 总体计算成本降低 31%
- Improved user experience with faster research results
- 通过更快的​​研究结果改善了用户体验
- Challenges: Complex cache invalidation when legal precedents change
- 挑战：当法律先例发生变化时，复杂的缓存失效

**When Semantic Caching Makes Sense:**
**语义缓存何时有意义：**
- High-cost AI operations (complex RAG, long documents)
- 高成本的 AI 操作（复杂的 RAG、长文档）
- Repetitive user patterns (customer support, research)
- 重复的用户模式（客户支持、研究）
- Stable knowledge domains (legal, medical, technical documentation)
- 稳定的知识领域（法律、医疗、技术文档）

**When to Avoid Semantic Caching:**
**何时避免语义缓存：**
- Low-cost AI operations (simple chat completions)
- 低成本的 AI 操作（简单的聊天完成）
- Highly dynamic content (real-time data, breaking news)
- 高度动态的内容（实时数据、突发新闻）
- Privacy-sensitive queries (each user needs fresh results)
- 对隐私敏感的查询（每个用户都需要新的结果）

## 4. Observability: Monitoring AI Performance
## 4. 可观察性：监控 AI 性能

Again, observability is a universal practice across software engineering. Specific to AI, it is typically number of input and output tokens per request, format failures (if you expect JSON outputs, track how often the model gives invalid JSON). For open-ended generations, consider things like conciseness, creativity, or positivity - many of these metrics can be computed using AI judges.
同样，可观察性是整个软件工程领域的普遍实践。具体到 AI，它通常是每个请求的输入和输出令牌数、格式失败（如果您期望 JSON 输出，则跟踪模型给出无效 JSON 的频率）。对于开放式生成，请考虑简洁性、创造力或积极性等因素——许多这些指标都可以使用 AI 评委来计算。

### Core AI Metrics
### 核心 AI 指标

**Real Example - SaaS Platform:** A project management tool with AI features tracks comprehensive metrics:
**真实示例 - SaaS 平台：** 一个具有 AI 功能的项目管理工具跟踪全面的指标：

**Token and Cost Metrics:**
**令牌和成本指标：**
- **Input tokens per request:** Average 342 tokens, 95th percentile 1,247 tokens
- **每个请求的输入令牌：** 平均 342 个令牌，第 95 个百分位为 1,247 个令牌
- **Output tokens per request:** Average 156 tokens, max capped at 500
- **每个请求的输出令牌：** 平均 156 个令牌，最大上限为 500
- **Cost per request:** $0.0034 average, trending down 12% month-over-month
- **每个请求的成本：** 平均 0.0034 美元，环比下降 12%
- **Model utilization:** GPT-4: 23%, GPT-3.5: 61%, Claude: 16%
- **模型利用率：** GPT-4：23%，GPT-3.5：61%，Claude：16%

**Quality Metrics:**
**质量指标：**
- **Format compliance:** 94.3% of JSON responses are valid (target: >95%)
- **格式合规性：** 94.3% 的 JSON 响应有效（目标：>95%）
- **Response relevance:** AI judge scores responses 1-5, average 4.2
- **响应相关性：** AI 评委对响应进行 1-5 分评分，平均 4.2 分
- **Citation accuracy:** 87% of RAG responses include valid citations
- **引文准确性：** 87% 的 RAG 响应包含有效引文
- **Hallucination rate:** 3.1% of responses contain ungrounded claims
- **幻觉率：** 3.1% 的响应包含无根据的声明

### Advanced Observability Patterns
### 高级可观察性模式

**User Behavior Analytics:**
**用户行为分析：**
- **Conversation length:** Average 3.4 turns, 15% of users have >10 turn conversations
- **对话长度：** 平均 3.4 轮，15% 的用户进行超过 10 轮的对话
- **Early termination:** 8% of users stop generation mid-response (indicates poor quality)
- **提前终止：** 8% 的用户在响应中途停止生成（表明质量差）
- **Retry rate:** 12% of users regenerate responses (quality indicator)
- **重试率：** 12% 的用户重新生成响应（质量指标）
- **Feature adoption:** AI features used by 67% of active users
- **功能采用率：** 67% 的活跃用户使用 AI 功能

**Performance Monitoring:**
**性能监控：**
- **End-to-end latency:** P50: 1.2s, P95: 4.7s, P99: 12.3s
- **端到端延迟：** P50：1.2 秒，P95：4.7 秒，P99：12.3 秒
- **Model latency:** Tracked separately from RAG retrieval and guardrail processing
- **模型延迟：** 与 RAG 检索和护栏处理分开跟踪
- **Error rates:** Model API errors (2.1%), timeout errors (0.8%), guardrail blocks (1.4%)
- **错误率：** 模型 API 错误 (2.1%)、超时错误 (0.8%)、护栏阻止 (1.4%)
- **Availability:** 99.2% uptime across all AI features
- **可用性：** 所有 AI 功能的正常运行时间为 99.2%

**Business Impact Metrics:**
**业务影响指标：**
- **User engagement:** AI users have 34% higher retention than non-AI users
- **用户参与度：** AI 用户的保留率比非 AI 用户高 34%
- **Support ticket reduction:** 23% fewer support tickets from AI-enabled users
- **支持工单减少：** AI 用户的支持工单减少 23%
- **Feature satisfaction:** NPS score of 67 for AI features (vs 45 overall product NPS)
- **功能满意度：** AI 功能的 NPS 得分为 67（而整体产品 NPS 为 45）
- **Revenue impact:** AI features drive 18% higher conversion to paid plans
- **收入影响：** AI 功能使付费计划的转化率提高了 18%

### RAG-Specific Observability
### RAG 特定的可观察性

**Real Example - Knowledge Management Platform:** An enterprise search tool with RAG capabilities:
**真实示例 - 知识管理平台：** 一个具有 RAG 功能的企业搜索工具：

**Retrieval Quality:**
**检索质量：**
- **Retrieval latency:** Vector search: 45ms, semantic reranking: 120ms
- **检索延迟：** 向量搜索：45 毫秒，语义重排：120 毫秒
- **Chunk relevance:** Human annotators rate top 3 chunks, 78% relevance score
- **块相关性：** 人工注释员对前 3 个块进行评分，相关性得分为 78%
- **Coverage analysis:** 23% of queries require information from multiple sources
- **覆盖率分析：** 23% 的查询需要来自多个来源的信息
- **Index freshness:** Track data age in search results (average 4.2 days)
- **索引新鲜度：** 跟踪搜索结果中的数据年龄（平均 4.2 天）

**Answer Quality:**
**答案质量：**
- **Citation rate:** 91% of responses include at least one citation
- **引文率：** 91% 的响应至少包含一个引文
- **Citation accuracy:** 83% of citations actually support the answer
- **引文准确性：** 83% 的引文实际支持答案
- **Completeness:** AI judge rates answer completeness, average 4.1/5
- **完整性：** AI 评委对答案完整性进行评分，平均 4.1/5
- **Conflicting information:** Flag when sources contradict each other (7% of queries)
- **信息冲突：** 当来源相互矛盾时标记（7% 的查询）

### Implementation Tools and Platforms
### 实施工具和平台

**Monitoring Stack:**
**监控堆栈：**
- **Application Metrics:** DataDog for latency, throughput, error rates
- **应用程序指标：** 用于延迟、吞吐量、错误率的 DataDog
- **AI-Specific Metrics:** Weights & Biases for model performance tracking
- **AI 特定指标：** 用于模型性能跟踪的 Weights & Bienses
- **User Analytics:** Mixpanel for user behavior and feature adoption
- **用户分析：** 用于用户行为和功能采用的 Mixpanel
- **Cost Tracking:** Custom dashboard aggregating usage across all model providers
- **成本跟踪：** 聚合所有模型提供商使用情况的自定义仪表板

**Alert Configuration:**
**警报配置：**
- **Immediate alerts:** Error rate >5%, latency >10 seconds, guardrail blocks >20%
- **即时警报：** 错误率 >5%，延迟 >10 秒，护栏阻止 >20%
- **Daily alerts:** Cost spike >30%, quality score drop >10%
- **每日警报：** 成本飙升 >30%，质量得分下降 >10%
- **Weekly reports:** Usage trends, cost optimization opportunities, quality improvements
- **每周报告：** 使用趋势、成本优化机会、质量改进

**Dashboard Design:**
**仪表板设计：**
- **Executive dashboard:** High-level metrics, cost trends, business impact
- **高管仪表板：** 高级指标、成本趋势、业务影响
- **Engineering dashboard:** Technical metrics, error analysis, performance optimization
- **工程仪表板：** 技术指标、错误分析、性能优化
- **Product dashboard:** User behavior, feature adoption, quality trends
- **产品仪表板：** 用户行为、功能采用、质量趋势

The key to successful AI observability is balancing technical metrics with business outcomes. Track what matters for your specific use case, and always connect AI performance to user satisfaction and business results.
成功实现 AI 可观察性的关键在于平衡技术指标与业务成果。跟踪对您的特定用例至关重要的内容，并始终将 AI 性能与用户满意度和业务成果联系起来。