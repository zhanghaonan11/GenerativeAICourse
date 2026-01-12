# RAG (Retrieval Augmented Generation)
# RAG（检索增强生成）

## The Problem RAG Solves
## RAG 解决的问题

AI models are trained on public data up to a specific cutoff date. They cannot access information beyond what they were trained on — this includes your private company data, recent information beyond their training cutoff, or any specialized knowledge not in their training dataset.

AI 模型是在特定截止日期之前的公开数据上训练的。它们无法访问训练数据之外的信息——这包括你的私有公司数据、超出训练截止日期的最新信息，或任何不在其训练数据集中的专业知识。

The question is: how can we leverage the AI model's reasoning capabilities on data it wasn't trained on? For example, helping customers find products in your catalog, or reasoning about your company's internal documents.

问题是：我们如何在模型未经训练的数据上利用 AI 模型的推理能力？例如，帮助客户在你的产品目录中找到产品，或对公司内部文档进行推理。

## How RAG Works
## RAG 的工作原理

What we do is turn these private documents into smaller chunks (more on that later), use an embedding AI model to turn these chunks into vectors (a series of numbers that capture sentiment and meaning of these chunks), we store these vector chunks in a vector database. Then when the user asks a question about these private documents (such as: "Recommend me products for hiking Mount Everest"), we turn the user question into vectors (again capturing the question's sentiment and meaning), compare that vector with the vectors stored in our vector database, and then finally feed the model both the user question and the returned chunked vector (from the vector comparison).

我们所做的是将这些私有文档转换成更小的块（稍后详述），使用嵌入 AI 模型将这些块转换为向量（一系列捕获这些块的情感和含义的数字），我们将这些向量块存储在向量数据库中。然后当用户询问关于这些私有文档的问题时（例如："推荐我攀登珠穆朗玛峰的产品"），我们将用户问题转换为向量（同样捕获问题的情感和含义），将该向量与存储在向量数据库中的向量进行比较，然后最终将用户问题和返回的块向量（来自向量比较）一起提供给模型。

![image](https://github.com/user-attachments/assets/567589c9-4282-4a86-86d5-d9ea098e909f)


**So from first principles: RAG is literally building a function, where when user asks a question, we turn the question into vectors, compare it with vectors in our vector database, then once we find the relevant vectors, we add it to the question with the prompt. It is that simple.**

**所以从第一性原理来看：RAG 实际上就是构建一个函数，当用户提问时，我们将问题转换为向量，与向量数据库中的向量进行比较，然后一旦找到相关向量，我们就将其与提示一起添加到问题中。就是这么简单。**

## Why Chunking is Necessary
## 为什么需要分块

The reason why data is chunked is bec of context window. There is a limitation on the context window, and if the data exceeds that, which in most cases it will, then we divide it into chunks. Vector searches also work better on focused information. Note that when we say vector search, we typically mean semantic word search, rather than traditional keyword search.

数据被分块的原因是上下文窗口。上下文窗口有限制，如果数据超过这个限制（在大多数情况下会超过），那么我们就将其分成块。向量搜索在聚焦的信息上也能更好地工作。请注意，当我们说向量搜索时，我们通常指的是语义词搜索，而不是传统的关键词搜索。

## Search Types: Vector vs. Keyword vs. Hybrid
## 搜索类型：向量搜索 vs. 关键词搜索 vs. 混合搜索

**Vector search** is great, but might miss exact keyword matches (might be too smart and return related but not precisely matching content).

**向量搜索**很棒，但可能会错过精确的关键词匹配（可能太聪明了，返回相关但不精确匹配的内容）。

**Keyword search** is great for exact keyword matching. Of course, it can't understand semantic relationships.

**关键词搜索**非常适合精确的关键词匹配。当然，它无法理解语义关系。

This is why **Hybrid search** is common — it combines both techniques to get better overall retrieval quality. Documents are reranked by combined scores, giving you the best of both worlds.

这就是为什么**混合搜索**很常见——它结合了两种技术以获得更好的整体检索质量。文档按组合分数重新排名，让你两全其美。

There are actually chunking strategies to determine the most appropriate size of the chunk. There is a tradeoff: if it is too small it will lack context, but if it is too large it will dilute relevant information.

实际上有分块策略来确定最合适的块大小。这是一个权衡：如果太小会缺乏上下文，但如果太大会稀释相关信息。

Some vector databases, like Azure AI Search as an example, handle the chunking for us. We'll do it with code in this example.

一些向量数据库，例如 Azure AI Search，会为我们处理分块。在这个例子中，我们将用代码来实现。

## RAG Indexing: The Foundation of Quality
## RAG 索引：质量的基础

The quality of your RAG system depends on how you index your data.

你的 RAG 系统的质量取决于你如何索引数据。

### What Indexing Is
### 什么是索引

When you have a large collection of data, searching for it can be a challenge. The most straightforward way is to use linear search, but at scale, this becomes too slow.

当你有大量数据集合时，搜索可能是一个挑战。最直接的方法是使用线性搜索，但在大规模情况下，这会变得太慢。

Indexing creates a separate data structure that maps a searchable criteria to the location of the data. So instead of scanning all data, we scan the index first to identify where the relevant data is stored and retrieve only these pieces.

索引创建一个单独的数据结构，将可搜索的条件映射到数据的位置。因此，我们不是扫描所有数据，而是首先扫描索引以识别相关数据存储在哪里，然后只检索这些部分。

### Real-World Example: Pharmaceutical Company
### 真实世界示例：制药公司

For example, a pharmaceutical company creates an index mapping:

例如，一家制药公司创建索引映射：

```
"drug_interactions" → [doc_3, doc_127, doc_2891, doc_5643, doc_8901]
"clinical_trials" → [doc_45, doc_298, doc_3372, doc_7123]
"adverse_reactions" → [doc_12, doc_567, doc_4321, doc_9876]
"pharmacokinetics" → [doc_78, doc_1456, doc_6789]
```

Then when a scientist searches for drug interactions, it checks the index, and only retrieves the relevant documents as opposed to searching for all of them.

然后当科学家搜索药物相互作用时，它检查索引，只检索相关文档，而不是搜索所有文档。

### Database Indexing Example
### 数据库索引示例

In relational databases as an example, you can create an index which maps column values (like salary, name, address) to row locations (actual values).

以关系数据库为例，你可以创建一个索引，将列值（如工资、姓名、地址）映射到行位置（实际值）。

Let's say you have a table with 50,000 employee staff records:

假设你有一个包含 50,000 条员工记录的表：

```sql
CREATE TABLE employees (
    emp_id INT,
    last_name VARCHAR(100),
    department VARCHAR(50),
    hire_date DATE,
    salary DECIMAL(10,2)
);
```

You can create an index on the last name:

你可以在姓氏上创建索引：

```sql
CREATE INDEX idx_last_name ON employees(last_name);
```

This then creates a separate data structure that looks like this:

这会创建一个如下所示的单独数据结构：

**Index Structure (idx_last_name):**
**索引结构（idx_last_name）：**
```
"Adams" → Row 15,847
"Brown" → Row 2,156
"Chen" → Row 41,923
"Davis" → Row 8,901
"Evans" → Row 33,412
... (sorted alphabetically / 按字母顺序排序)
"Wilson" → Row 7,234
"Young" → Row 28,567
```

Now, when you search for Adams last name as an example, you can quickly retrieve it.

现在，当你搜索 Adams 姓氏作为示例时，你可以快速检索到它。

### RAG Indexing Context
### RAG 索引上下文

In the context of RAG, like in a relational database where we use indexes to avoid scanning every row, RAG systems use vector database indexes to avoid comparing your query against every single document chunk.

在 RAG 的上下文中，就像在关系数据库中我们使用索引来避免扫描每一行一样，RAG 系统使用向量数据库索引来避免将你的查询与每个文档块进行比较。

In RAG systems, indexing quality affects whether your AI finds the right information to answer questions. Poor indexing means the AI gets irrelevant or incomplete context which leads to wrong or useless responses.

在 RAG 系统中，索引质量影响你的 AI 是否能找到正确的信息来回答问题。糟糕的索引意味着 AI 获得不相关或不完整的上下文，这会导致错误或无用的响应。

## RAG Indexing: 3 Critical Layers
## RAG 索引：3 个关键层

### 1. Document Chunking
### 1. 文档分块

How do you actually organize the data. You can do it by character limit, but that means you will most likely miss some context or critical information. Hence why semantic chunking is used as illustrated before. These semantic splitters use language models.

你实际上如何组织数据。你可以按字符限制来做，但这意味着你很可能会错过一些上下文或关键信息。因此，如前所述，使用语义分块。这些语义分割器使用语言模型。

### 2. Vector Embedding (Converting Text to Numbers)
### 2. 向量嵌入（将文本转换为数字）

Generic embedding can retrieve bad results (for instance, "IP" in legal might not be understood as intellectual property). Domain-specific embeddings lead to better results, so like fine-tuned embedding model on company data.

通用嵌入可能会检索到不好的结果（例如，法律中的"IP"可能不会被理解为知识产权）。特定领域的嵌入会带来更好的结果，比如在公司数据上微调的嵌入模型。

### 3. Vector Database Indexing
### 3. 向量数据库索引

HNSW is the industry standard.

HNSW 是行业标准。

## Relevance and Quality Control
## 相关性和质量控制

One thing to note is that not all search results are actually relevant. You need to determine what constitutes relevant content.

需要注意的一点是，并非所有搜索结果都是真正相关的。你需要确定什么构成相关内容。

There is a **relevance threshold** that you put for vector searches. If you put it too high (0.0) then it might miss some content but it is valuable in high precision scenarios where wrong info could be costly. If it is too low, it might include irrelevant context that confuses the model. You need a way to measure this as well.

你为向量搜索设置一个**相关性阈值**。如果设置得太高（0.0），那么它可能会错过一些内容，但在错误信息可能代价高昂的高精度场景中很有价值。如果太低，它可能会包含混淆模型的不相关上下文。你也需要一种方法来衡量这一点。

**User satisfaction:** Are users getting useful information? How often are retrieved chunks actually relevant?

**用户满意度：** 用户是否获得了有用的信息？检索到的块实际上有多少是相关的？

## Production Considerations
## 生产环境考虑因素

### Data Pipeline Challenges
### 数据管道挑战
- How often does your knowledge base change?
- Ensuring clean and accurate data enters the system
- Version management (handling document updates and deletions)

- 你的知识库多久更新一次？
- 确保干净准确的数据进入系统
- 版本管理（处理文档更新和删除）

### Governance
### 治理
- How long to keep search logs & user queries
- Track who searched what and when

- 搜索日志和用户查询保留多长时间
- 跟踪谁在什么时候搜索了什么
