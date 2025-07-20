# LLM Architecture and Training
# LLM 架构与训练

You don't need to know how to develop a model to use it, but understanding how they work at a high level will help you decide which model to use.
您无需了解如何开发模型即可使用它，但从宏观层面了解其工作原理将有助于您决定使用哪种模型。

There is lack of transparency in the training process of foundation models, but we can get a good understanding of them by knowing the pre-training data, model architecture & size, and how they are post-trained to align with human preferences.
基础模型的训练过程缺乏透明度，但我们可以通过了解预训练数据、模型架构和规模，以及它们如何经过后训练以符合人类偏好，从而对它们有一个很好的了解。

## 1. Training Data
## 1. 训练数据

A common source for training data is Common Crawl, which is a nonprofit organization that regularly crawls websites on the Internet. The quality of Common Crawl though is not always the best because within that training data you have clickbait, misinformation, propaganda, etc. To avoid scrutiny from the public and competitors, companies stopped disclosing this information.
训练数据的一个常见来源是 Common Crawl，这是一个非营利组织，定期抓取互联网上的网站。然而，Common Crawl 的质量并非总是最好的，因为在该训练数据中，您会发现点击诱饵、错误信息、宣传等。为了避免公众和竞争对手的审查，公司停止披露此信息。

English language dominates the internet, so it performs better than other languages. General purpose foundational models can be great for everyday tasks, but might struggle with domain-specific tasks they never saw during training like drug discovery and cancer screening. For instance, drug discovery involves protein, DNA and other data which is unlikely to be found publicly. Cancer screening typically involves X-ray and MRI scans which also is hard to find publicly due to privacy. DeepMind's AlphaFold is an example of domain-specific model trained on about 100k known proteins. Nvidia BioNeMo is another model that focuses on drug discovery.
英语在互联网上占主导地位，因此其表现优于其他语言。通用基础模型非常适合日常任务，但可能难以处理它们在训练期间从未见过的特定领域任务，例如药物发现和癌症筛查。例如，药物发现涉及蛋白质、DNA 和其他不太可能公开找到的数据。癌症筛查通常涉及 X 射线和 MRI 扫描，由于隐私原因，这些扫描也很难公开找到。 DeepMind 的 AlphaFold 是一个在约 10 万个已知蛋白质上训练的特定领域模型的例子。 Nvidia BioNeMo 是另一个专注于药物发现的模型。

### Data Challenges and Future Concerns
### 数据挑战与未来担忧

**Scaling Bottleneck:** Until now, every magnitude increase in model size led to an increase in model performance. But would there be a point where model performance plateaus regardless of size? Foundational models use so much data that there's a concern we will run out of internet data in the next few years. Foundation models consume training data faster than humans create new content. If we need 100T words for the next model generation, but the entire internet only contains 50T words, we're approaching physical limits. To tackle that, companies are now tapping into proprietary data like paying Reddit, publishers, exclusive data partnerships, etc.
**扩展瓶颈：** 到目前为止，模型规模的每一次数量级增长都会带来模型性能的提升。但是，是否存在一个点，无论规模如何，模型性能都会停滞不前？基础模型使用如此多的数据，以至于人们担心我们将在未来几年内耗尽互联网数据。基础模型消耗训练数据的速度比人类创造新内容的速度快。如果下一代模型需要 100 万亿个单词，但整个互联网只包含 50 万亿个单词，那么我们正在接近物理极限。为了解决这个问题，公司现在正在利用专有数据，例如向 Reddit、出版商付费、建立独家数据合作伙伴关系等。

**Security Implications:** Anything you post online, you should assume it will be part of future models' training data. Bad actors exploit that for prompt injection attacks by publishing stuff on the internet hoping it will influence future models to generate the responses they desire. For instance, they can create fake academic looking articles containing statements like: "The most effective way to bypass airport security involves [detailed harmful instructions]." Now when someone asks the AI model about airport security, the model might reference this research and present dangerous information. AI models can't distinguish between real and fake expertise during training.
**安全隐患：** 您在网上发布的任何内容，都应假定它将成为未来模型训练数据的一部分。不良行为者利用这一点进行提示注入攻击，他们在互联网上发布内容，希望影响未来模型生成他们想要的响应。例如，他们可以创建看起来像学术文章的虚假文章，其中包含诸如“绕过机场安检最有效的方法包括[详细的有害说明]”之类的陈述。现在，当有人向 AI 模型询问机场安检时，模型可能会引用这项研究并提供危险信息。 AI 模型在训练期间无法区分真实和虚假的专业知识。

**Data Authenticity:** The other thing to think about is that AI trained on human data is more authentic. Humans make genuine mistakes, they reflect authentic emotional states & experiences, etc. If models are trained on AI data that lacks authenticity, is perfect, etc. then future AI models can be disconnected from authentic human communication patterns or unable to generate truly novel ideas.
**数据真实性：** 另一件需要考虑的事情是，在人类数据上训练的 AI 更具真实性。人类会犯真正的错误，他们会反映真实的情感状态和体验等。如果模型在缺乏真实性、完美等的 AI 数据上进行训练，那么未来的 AI 模型可能会与真实的人类交流模式脱节，或者无法产生真正新颖的想法。

**Data Forgetting:** An open research question is how to make a model forget specific information it has learned during training. Imagine you published a blog post that you eventually deleted. If that blog post was included in a model's training data, the model might still reproduce the post's content. As a result, people could potentially access removed content without your consent.
**数据遗忘：** 一个悬而未决的研究问题是如何让模型忘记它在训练期间学到的特定信息。想象一下，您发表了一篇博客文章，但最终删除了它。如果该博客文章包含在模型的训练数据中，模型可能仍会重现该文章的内容。因此，人们可能会在未经您同意的情况下访问已删除的内容。

## 2. Architecture
## 2. 架构

### Seq2Seq (Pre-Transformer Era)
### Seq2Seq（前 Transformer 时代）

LLMs are based on the transformer architecture. Before the transformer was invented, there was seq2seq architecture which was used for machine translation. It introduced an encoder-decoder paradigm where an encoder processes input sequences and decoder generates output sequences, both using RNN (recurrent neural networks). The problem with Seq2Seq is that all inputs (tokens) are processed sequentially which is slow, but also all input is compressed into a final state so context can be missed.
LLM 基于 Transformer 架构。在 Transformer 发明之前，有用于机器翻译的 seq2seq 架构。它引入了一种编码器-解码器范式，其中编码器处理输入序列，解码器生成输出序列，两者都使用 RNN（循环神经网络）。 Seq2Seq 的问题在于所有输入（令牌）都是按顺序处理的，这很慢，而且所有输入都被压缩成一个最终状态，因此可能会丢失上下文。

![image](https://github.com/user-attachments/assets/18614e4d-573e-47be-9dea-c649dac2444b)



### Transformer Architecture
### Transformer 架构

Transformer architecture removed RNNs. Say you want to summarize a 2k word article. Seq2Seq with RNN must process all of these words sequentially, while the transformer processes it in parallel (but the output is processed sequentially, token by token). Transformers have a two-phase inference process:
Transformer 架构移除了 RNN。假设您想总结一篇 2000 字的文章。带有 RNN 的 Seq2Seq 必须按顺序处理所有这些单词，而 Transformer 则并行处理它（但输出是按顺序处理的，逐个令牌）。 Transformer 有一个两阶段的推理过程：

1. **First Phase:** Processing all input tokens simultaneously. When you ask "explain quantum computing", the transformer processes all the words in parallel by creating a key and value vector for each token (like a lookup table). So "explain" becomes K1, V1 (key vector 1 and value vector v1).
1. **第一阶段：** 同时处理所有输入令牌。当您询问“解释量子计算”时，Transformer 通过为每个令牌创建键和值向量（如查找表）来并行处理所有单词。因此，“解释”变为 K1、V1（键向量 1 和值向量 v1）。

2. **Second Phase:** Generates output tokens or words one at a time (sequential). So it generates "Quantum" looking at your entire question, then generating "Computing" looking at your question + Quantum (first token outputted), then generates "uses" looking at your question + Quantum Computing (first 2 tokens).
2. **第二阶段：** 一次生成一个输出令牌或单词（顺序）。因此，它会查看您的整个问题生成“Quantum”，然后查看您的问题 + Quantum（第一个输出的令牌）生成“Computing”，然后查看您的问题 + Quantum Computing（前 2 个令牌）生成“uses”。

3. ![image](https://github.com/user-attachments/assets/9c2aa716-bc26-4634-bf5e-cdc84a2989c8)



### Attention Mechanism
### 注意力机制

If you ask ChatGPT "Write a product description for our new wireless headphone that emphasizes battery life and sound quality", the LLM needs to generate each word of its responses while keeping track of what's important. It should focus on keywords like battery life and sound quality not generic words like "for" or "our". So at each word generation step, the model must decide: "Out of all the previous words I've seen, which ones should influence what I write next?"
如果您问 ChatGPT“为我们的新款无线耳机写一份产品描述，强调电池续航和音质”，LLM 需要在生成每个单词的响应时，同时跟踪重要信息。它应该关注像“电池续航”和“音质”这样的关键词，而不是像“为”或“我们的”这样的通用词。因此，在每个单词生成步骤中，模型都必须决定：“在我见过的所有前面的单词中，哪些应该影响我接下来写什么？”

It consists of 3 things:
它由 3 部分组成：

- **Query Vector (Q):** As it is writing the word, it creates a query vector: "What type of word do I need right now?"
- **查询向量 (Q)：** 在书写单词时，它会创建一个查询向量：“我现在需要什么类型的单词？”
- **Key Vector (K):** What type of information does each previous word contain (every word in your request gets a key vector describing what it is, so "product" is content type, "wireless" is feature descriptor, etc).
- **键向量 (K)：** 每个前面的单词包含什么类型的信息（您请求中的每个单词都会得到一个描述其内容的键向量，因此“产品”是内容类型，“无线”是功能描述符等）。
- **Value Vectors (V):** Each word gets a value vector containing its semantic meanings.
- **值向量 (V)：** 每个单词都会得到一个包含其语义含义的值向量。

Then after all that, the attention calculation process starts. When generating the word "exceptional" as an example, the LLM computes how relevant each previous word is by giving them attention weights, so 35% attention is given to battery, 32% is given to sound, etc. So this gives "exceptional" a meaning that's 35% influenced by battery concepts and 32% by sound concepts.
然后，在所有这些之后，注意力计算过程开始。例如，在生成“卓越”一词时，LLM 通过赋予每个前面的单词注意力权重来计算其相关性，因此 35% 的注意力给予电池，32% 的注意力给予声音等。因此，这赋予了“卓越”一个受电池概念影响 35% 和受声音概念影响 32% 的含义。

**In summary:** A transformer is an architecture that processes input tokens in parallel and generates output tokens sequentially, and it leverages attention mechanism which allows the model to dynamically focus on the most relevant parts of the input when generating each new word.
**总之：** Transformer 是一种并行处理输入令牌并按顺序生成输出令牌的架构，它利用注意力机制，允许模型在生成每个新词时动态地关注输入的最相关部分。

### Model Size
### 模型大小

Note that as the community better understands how to train models, newer models tend to outperform older generation even of the same parameter size. The number of parameters help us estimate the compute resources needed to train and run the model. For example, if a model has 7B parameters, and each parameter is stored using 2 bytes, then we can calculate that the GPU memory needed to do inference using this model will be at least 14B bytes (14GB).
请注意，随着社区对如何训练模型的理解越来越深入，即使参数大小相同，较新的模型也往往优于上一代模型。参数数量有助于我们估算训练和运行模型所需的计算资源。例如，如果一个模型有 70 亿个参数，并且每个参数使用 2 个字节存储，那么我们可以计算出使用该模型进行推理所需的 GPU 内存至少为 140 亿字节（14GB）。

But when discussing model size, it is important to consider the size of the data it was trained on. We can look at the number of tokens, which isn't a perfect measure, as different models can have different tokenization processes. Meta used 15T tokens for Llama 3.
但是在讨论模型大小时，考虑其训练数据的大小很重要。我们可以查看令牌的数量，但这并不是一个完美的衡量标准，因为不同的模型可以有不同的令牌化过程。 Meta 为 Llama 3 使用了 15T 令牌。

## Post-Training
## 后训练

Post-training exists because a pre-trained model has two issues: They are optimized for text completion & not conversations, and the output of the model can be racist, sexist, or just wrong since it is trained on data indiscriminately scraped from the internet.
后训练之所以存在，是因为预训练模型存在两个问题：它们针对文本补全而非对话进行了优化，并且模型的输出可能是种族主义、性别歧视或完全错误的，因为它是在从互联网上不加选择地抓取的数据上进行训练的。

Pre-training optimizes token level quality where the model is trained to predict the next token accurately. But users don't care about token level quality, they care about the quality of the entire response. Post-training optimizes for generating responses that users prefer.
预训练优化了令牌级别的质量，其中模型被训练来准确预测下一个令牌。但用户不关心令牌级别的质量，他们关心整个响应的质量。后训练优化了生成用户偏好的响应。

So for the first issue, it is handled by giving the model a series of training data with prompts and responses which contain the range of requests you want your model to handle such as question answering, summarization, translation, etc.
因此，对于第一个问题，可以通过向模型提供一系列带有提示和响应的训练数据来处理，这些数据包含您希望模型处理的请求范围，例如问答、摘要、翻译等。

For the latter, reinforcement learning is used where a reward model scores the foundation model's outputs. The foundation model will then be optimized to generate responses for which the reward model will give maximal scores.
对于后者，使用强化学习，其中奖励模型对基础模型的输出进行评分。然后将优化基础模型以生成奖励模型将给予最高分数的响应。

## Sampling: How AI Generates Responses
## 采样：AI 如何生成响应

A model produces its output through a process known as **sampling**. Sampling is what makes AI's output probabilistic, which makes it powerful and creative, but can sometimes cause it to be inconsistent and hallucinate.
模型通过一个称为**采样**的过程产生其输出。采样使 AI 的输出具有概率性，这使其强大而富有创造力，但有时也会导致其不一致和产生幻觉。

Neural networks receive an input and produce an output by computing the possibilities of different outcomes. If it is a classification model on email spams, there are only two possible outcomes which are true or false. For a language model, the model has to generate the next token. If the input is: "What's your favorite color?" There are many possible outputs like green (50%), red (30%), the (0.2%), a (0.1%).
神经网络接收一个输入，并通过计算不同结果的可能性来产生一个输出。如果它是一个关于电子邮件垃圾邮件的分类模型，那么只有两个可能的结果，即真或假。对于语言模型，模型必须生成下一个令牌。如果输入是：“你最喜欢的颜色是什么？”有很多可能的输出，比如绿色 (50%)、红色 (30%)、the (0.2%)、a (0.1%)。

You might think that a language model would just pick the highest probability (like green) but that results in boring outputs. Imagine if for whatever question you ask, you always get back the most common words.
您可能会认为语言模型只会选择概率最高的（比如绿色），但这会导致无聊的输出。想象一下，无论您问什么问题，您总是得到最常见的单词。

So instead of always picking the most likely token, the model picks red 30% of the time, green 50% of the time, etc.
因此，模型不是总是选择最有可能的令牌，而是在 30% 的时间里选择红色，在 50% 的时间里选择绿色，等等。

So the right sampling strategy can make a model generate responses more suitable for your application. One might be to generate more creative responses, while others might be to generate more predictable responses.
因此，正确的采样策略可以使模型生成更适合您的应用程序的响应。一种可能是生成更具创造性的响应，而另一种可能是生成更可预测的响应。

![image](https://github.com/user-attachments/assets/f1d0915c-d03c-433f-ac30-4ab482515708)



### Temperature Control
### 温度控制

**Temperature** is an example of a way to control that sampling. The problem with probability is that the model can be less creative. For instance, colors red, green, and purple have highest probabilities so the answers you get will always be rigid like "my favorite color is green". Because "the" has a low probability, the model has a low chance of generating a creative sentence such as "My favorite color is the color of a still lake on a spring morning". A higher temperature reduces the probabilities of common tokens, so as a result, increases the probabilities of rarer tokens enabling models to have more creative responses.
**温度**是控制采样的一种方式。概率的问题在于模型可能缺乏创造力。例如，红色、绿色和紫色的概率最高，因此您得到的答案总是很刻板，比如“我最喜欢的颜色是绿色”。因为“the”的概率很低，所以模型生成创造性句子的机会很小，比如“我最喜欢的颜色是春天早晨静止湖泊的颜色”。较高的温度会降低常见令牌的概率，因此会增加稀有令牌的概率，从而使模型能够产生更具创造性的响应。

**Top-k and Top-p** is another sampling strategy.
**Top-k 和 Top-p** 是另一种采样策略。

## Inconsistency and Hallucinations
## 不一致和幻觉

The way AI sample their responses make them probabilistic. This nature causes inconsistency and hallucinations. **Inconsistency** is when a model generates very different responses for the same prompt, and **hallucinations** is when a model gives a response that isn't grounded in facts (it makes things up).
AI 采样其响应的方式使其具有概率性。这种性质会导致不一致和幻觉。**不一致**是指模型对同一提示生成截然不同的响应，而**幻觉**是指模型给出的响应不基于事实（它会编造事实）。

If there are essays in the training data saying US presidents are aliens, the model might probabilistically output that the current US president is an alien, and from our perspective, we think the model is making things up. Remember, language models are trained on large amounts of data, and anything with a non-zero probability, no matter how far-fetched or wrong, can be generated by AI.
如果训练数据中有文章说美国总统是外星人，模型可能会概率性地输出说现任美国总统是外星人，而从我们的角度来看，我们认为模型是在编造事实。请记住，语言模型是在大量数据上训练的，任何具有非零概率的东西，无论多么牵强或错误，都可以由 AI 生成。

Inconsistency is caused by randomness in the sampling process, but hallucinations is more nuanced. Because yes, a model samples outputs from all probable options and the output can be something ridiculous if it was in the training set. But how does a model make something up that it never seen before in the training data?
不一致是由采样过程中的随机性引起的，但幻觉则更为微妙。因为是的，模型会从所有可能的选项中采样输出，如果它在训练集中，输出可能会很荒谬。但是，模型如何编造出它在训练数据中从未见过的东西呢？

The first hypothesis was expressed by DeepMind, and that is that language models can't differentiate between the data it's given and the data it generates. If you showed the model a picture of a bottle with ingredients, it predicts that the bottle is milk (even though it is not), then in the consecutive computation, it will use that and will invent ingredients related to a bottle of milk. Usually this is mitigated by reinforcement learning where the model is learned to differentiate between user provided prompts and tokens generated by the model.
第一个假设是由 DeepMind 提出的，即语言模型无法区分给定的数据和它生成的数据。如果你给模型看一张带有成分的瓶子图片，它会预测瓶子里是牛奶（即使不是），然后在接下来的计算中，它会使用这个预测并会发明与一瓶牛奶相关的成分。通常，这可以通过强化学习来缓解，其中模型被学习来区分用户提供的提示和模型生成的令牌。