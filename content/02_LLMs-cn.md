# How LLMs Work
# LLM 的工作原理

We're going to get more technical as we move on, but still within the spirit of sticking to first principles.
随着我们的深入，我们将变得更加技术化，但仍将本着坚持第一性原则的精神。

## The Core Mechanism: Next Word Prediction
## 核心机制：下一个单词预测

We all used tools like ChatGPT — how do these actually work behind the scenes? A large language model takes a series of text input like "The cat sat on" and literally predicts what the next word is. It predicts that "the" is 70% likely, "a" is 20% likely, "my" is 10% likely, so it picks "the". It becomes "the cat sat on the". Then predicts the next word, which is "mat" as most likely. Each new word is based on all previous words, like a sophisticated auto-complete.
我们都使用过像 ChatGPT 这样的工具——它们在幕后是如何工作的？一个大型语言模型会接收一系列文本输入，比如“猫坐在”，然后逐字预测下一个单词是什么。它预测“the”的可能性为 70%，“a”的可能性为 20%，“my”的可能性为 10%，所以它选择了“the”。它变成了“猫坐在了”。然后预测下一个单词，最有可能的是“垫子”。每个新单词都基于所有前面的单词，就像一个复杂的自动完成功能。

![image](https://github.com/user-attachments/assets/9331a11c-f988-4099-bb76-ecc77fd835c4)

So if you ask ChatGPT, "What is the capital of France?", it looks at the series of words "the" "capital" "of" "France" then predicts "is" as logical next word, then "Paris". The model is literally just predicting likely next words based on patterns it learned. It does this well because it is trained on massive amounts of text.
所以，如果你问 ChatGPT：“法国的首都是什么？”，它会查看“the”、“capital”、“of”、“France”这一系列单词，然后预测“is”是合乎逻辑的下一个单词，然后是“Paris”。该模型实际上只是根据它学到的模式预测可能的下一个单词。它之所以能做到这一点，是因为它是在海量文本上进行训练的。

You might say, "OK, I get that when I ask ChatGPT a question, it takes my prompt or question, which is a series of words, and predicts what the next words are. How does it generate text, like write an essay, when I ask it to?"
你可能会说：“好吧，我明白了，当我问 ChatGPT 一个问题时，它会接收我的提示或问题，也就是一系列单词，然后预测下一个单词是什么。当我要求它写一篇文章时，它是如何生成文本的呢？”

**Text generation is just repeated word prediction.** If you ask the model: "Write an essay about cats", it looks at the prompt "write an essay about cats" then it predicts the most likely word which is "cats" then "are" then "fascinating" and so on one word at a time. It is literally thinking one word at a time and picking the most appropriate word each time.
**文本生成只是重复的单词预测。** 如果你问模型：“写一篇关于猫的文章”，它会查看提示“写一篇关于猫的文章”，然后预测最有可能的单词是“猫”，然后是“是”，然后是“迷人的”，依此类推，一次一个单词。它实际上是一次思考一个单词，并每次选择最合适的单词。

## The Mathematical Foundation
## 数学基础

With house prices, f(x) = wx + b, we find w and b values that work best (parameters). With language models, each word becomes a list of numbers like [0.2, -0.5, 0.8], and these numbers, for each word, are known as **embeddings**. Each word is represented by a series of numbers capturing meaning, sentiment, etc.
对于房价，f(x) = wx + b，我们找到最合适的 w 和 b 值（参数）。对于语言模型，每个单词都变成一个数字列表，例如 [0.2, -0.5, 0.8]，而这些数字，对于每个单词，都称为**嵌入**。每个单词都由一系列数字表示，这些数字捕获了意义、情感等。

So "cat" becomes [0.2, -0.5, 0.8], "sat" becomes [0.3, 0.6, -0.2]. Cool, each word is represented by a series of numbers capturing different features of the word. The LLM model takes these numbers (embeddings) as input, uses a complex mathematical function (based on neural network) with billions of parameters and the output is a next word prediction.
所以“猫”变成了 [0.2, -0.5, 0.8]，“坐”变成了 [0.3, 0.6, -0.2]。很酷，每个单词都由一系列数字表示，这些数字捕获了单词的不同特征。 LLM 模型将这些数字（嵌入）作为输入，使用具有数十亿个参数的复杂数学函数（基于神经网络），输出是下一个单词的预测。

These parameters or variables in a mathematical function capture so many things like grammar rules, which sequence of words makes sense, how to combine words. This is why when you throw a sentence into the model, the model has billions of parameters that are able to predict what the next word is.
数学函数中的这些参数或变量捕获了许多东西，例如语法规则、哪些单词序列有意义、如何组合单词。这就是为什么当您向模型输入一个句子时，该模型拥有数十亿个能够预测下一个单词是什么的参数。

## Training Process: Self-Supervision
## 训练过程：自监督

The billions of parameters in this function are what gets adjusted during training to make better word predictions. So you show the model a lot of text, you start with random parameter values, model makes prediction, adjusts parameters to improve predictions.
这个函数中的数十亿个参数是在训练期间进行调整的，以做出更好的单词预测。所以你向模型展示大量文本，你从随机参数值开始，模型进行预测，调整参数以改进预测。

What happens in practice: you literally train the model on millions of sentences or text examples. Starting with: "The cat sat on the mat", we show the model "The cat sat on the", we know the correct answer is "The cat sat on the mat" and we adjust parameters until it predicts the word "mat". If it predicts the word "box", that is wrong, so we keep adjusting parameters. Then we move on to next sentence "Paris the capital", we change parameter values until it comes up with the word "of" as the next prediction.
实践中会发生什么：您实际上是在数百万个句子或文本示例上训练模型。从“猫坐在垫子上”开始，我们向模型展示“猫坐在”，我们知道正确答案是“猫坐在垫子上”，我们调整参数直到它预测出单词“垫子”。如果它预测出单词“盒子”，那是错误的，所以我们继续调整参数。然后我们转到下一个句子“巴黎首都”，我们更改参数值，直到它得出单词“的”作为下一个预测。

### Self-Supervision: Why This is Revolutionary
### 自我监督：为什么这是革命性的

Supervised machine learning (ML algorithms trained using labeled data) is expensive — if it costs 5 cents for one person to label one image, it would cost 50k to label a million images — and often not scalable. Language models use **self-supervision**. Say you train the model with the sentence "The cat sat on the mat", the model sees "The cat sat on the" and predicts "mat", so the original sentence itself provides the correct answer. So no human labeling is required.
监督式机器学习（使用标记数据训练的机器学习算法）成本高昂——如果一个人标记一张图片需要 5 美分，那么标记一百万张图片就需要 5 万美元——而且通常不可扩展。语言模型使用**自监督**。假设您用句子“猫坐在垫子上”来训练模型，模型看到“猫坐在”，并预测出“垫子”，因此原始句子本身就提供了正确答案。因此不需要人工标记。

So self-supervised learning means language models can learn from text sequences without requiring any labeling. Since text sequences are everywhere (books, blog posts, articles, etc), there is massive amount of training data allowing models to scale up to become LLMs.
因此，自监督学习意味着语言模型可以从文本序列中学习，而无需任何标记。由于文本序列无处不在（书籍、博客文章、文章等），因此有大量的训练数据，允许模型扩展成为 LLM。

Think about how many sentences we have — this is why the training is rigorous. Billions of parameters to adjust, millions of examples to learn from, takes weeks or months to train. Once we find parameters that can capture patterns from all examples, we are confident that the model can predict likely next words.
想想我们有多少个句子——这就是为什么训练如此严格。数十亿个参数需要调整，数百万个例子需要学习，需要数周或数月的时间来训练。一旦我们找到能够从所有例子中捕捉模式的参数，我们就有信心该模型能够预测可能的下一个单词。

Once we find values for these parameters, we're good — we can input any word, and we can easily predict the next word.
一旦我们找到这些参数的值，我们就大功告成了——我们可以输入任何单词，并且可以轻松地预测下一个单词。

## Tokens
## 令牌

These words that are transformed to numbers are known as **tokens**. Whenever you use LLMs hosted by other companies, you are paying per tokens.
这些被转换成数字的单词被称为**令牌**。每当您使用其他公司托管的 LLM 时，您都需要按令牌付费。

![image](https://github.com/user-attachments/assets/7a7bd7e9-4134-4480-a22c-a71cc3872277)



## Transformer Architecture
## Transformer 架构

In 2017, Google released a paper called "Attention is All You Need." Transformer architecture was invented in this paper. The inventors themselves did not realize the breakthrough they made. In 2018, GPT-1 was released, then GPT-2 in 2019, GPT-3 in 2020, and then in 2022 ChatGPT was released leveraging GPT-3 in combination with reinforcement learning from human feedback.
2017 年，谷歌发表了一篇名为《注意力就是你所需要的一切》的论文。 Transformer 架构就是在这篇论文中发明的。发明者自己并没有意识到他们所取得的突破。 2018 年发布了 GPT-1，2019 年发布了 GPT-2，2020 年发布了 GPT-3，然后在 2022 年发布了 ChatGPT，它利用 GPT-3 并结合了人类反馈的强化学习。

OpenAI's first GPT model had 117M parameters, now it is trillions.
OpenAI 的第一个 GPT 模型有 1.17 亿个参数，现在有数万亿个。

## Why Large Language Models Are General Purpose
## 为什么大型语言模型是通用的

Language models, thanks to their scale, are capable of a wide range of tasks. Previous models were specific to specific tasks like sentiment analysis or translation (language models can do both). If you're a retailer and want to generate product descriptions, you can use a model that generates accurate descriptions but might fail to capture the brand's voice or messaging. With language models, you can craft detailed instructions with examples of desired product descriptions, you can ground it in specific data or even fine-tune it.
语言模型由于其规模，能够胜任广泛的任务。以前的模型是针对特定任务的，例如情感分析或翻译（语言模型可以同时做到这两点）。如果您是零售商并希望生成产品描述，您可以使用一个可以生成准确描述但可能无法捕捉品牌声音或信息的模型。使用语言模型，您可以制作带有所需产品描述示例的详细说明，您可以将其建立在特定数据的基础上，甚至可以对其进行微调。