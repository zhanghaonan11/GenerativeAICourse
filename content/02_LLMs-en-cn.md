# How LLMs Work
# 大语言模型的工作原理

We're going to get more technical as we move on, but still within the spirit of sticking to first principles.

随着我们继续深入，内容将变得更加技术性，但仍然秉持着坚守第一性原理的精神。

## The Core Mechanism: Next Word Prediction
## 核心机制：下一个词预测

We all used tools like ChatGPT — how do these actually work behind the scenes? A large language model takes a series of text input like "The cat sat on" and literally predicts what the next word is. It predicts that "the" is 70% likely, "a" is 20% likely, "my" is 10% likely, so it picks "the". It becomes "the cat sat on the". Then predicts the next word, which is "mat" as most likely. Each new word is based on all previous words, like a sophisticated auto-complete.

我们都使用过像 ChatGPT 这样的工具——它们在幕后究竟是如何工作的？大语言模型接收一系列文本输入，如"The cat sat on"，然后字面上预测下一个词是什么。它预测"the"有 70% 的可能性，"a"有 20% 的可能性，"my"有 10% 的可能性，所以它选择"the"。它变成"the cat sat on the"。然后预测下一个词，最可能的是"mat"。每个新词都基于所有之前的词，就像一个复杂的自动补全功能。

![image](https://github.com/user-attachments/assets/9331a11c-f988-4099-bb76-ecc77fd835c4)


So if you ask ChatGPT, "What is the capital of France?", it looks at the series of words "the" "capital" "of" "France" then predicts "is" as logical next word, then "Paris". The model is literally just predicting likely next words based on patterns it learned. It does this well because it is trained on massive amounts of text.

所以如果你问 ChatGPT，"法国的首都是什么？"，它会查看一系列单词"the""capital""of""France"，然后预测"is"是合乎逻辑的下一个词，然后是"Paris"。这个模型实际上只是根据它学到的模式预测可能的下一个词。它做得很好，因为它是在海量文本上训练的。

You might say, "OK, I get that when I ask ChatGPT a question, it takes my prompt or question, which is a series of words, and predicts what the next words are. How does it generate text, like write an essay, when I ask it to?"

你可能会说，"好的，我理解当我问 ChatGPT 一个问题时，它接收我的提示或问题，这是一系列单词，然后预测下一个单词是什么。但当我要求它生成文本，比如写一篇文章时，它是怎么做到的？"

**Text generation is just repeated word prediction.** If you ask the model: "Write an essay about cats", it looks at the prompt "write an essay about cats" then it predicts the most likely word which is "cats" then "are" then "fascinating" and so on one word at a time. It is literally thinking one word at a time and picking the most appropriate word each time.

**文本生成只是重复的词预测。** 如果你要求模型："写一篇关于猫的文章"，它查看提示"write an essay about cats"，然后预测最可能的词是"cats"，然后是"are"，然后是"fascinating"，如此一个词一个词地进行。它实际上是一次只考虑一个词，每次都挑选最合适的词。

## The Mathematical Foundation
## 数学基础

With house prices, f(x) = wx + b, we find w and b values that work best (parameters). With language models, each word becomes a list of numbers like [0.2, -0.5, 0.8], and these numbers, for each word, are known as **embeddings**. Each word is represented by a series of numbers capturing meaning, sentiment, etc.

对于房价，f(x) = wx + b，我们找到最有效的 w 和 b 值（参数）。对于语言模型，每个词变成一个数字列表，如 [0.2, -0.5, 0.8]，这些数字对于每个词来说，被称为**嵌入（embeddings）**。每个词由一系列数字表示，这些数字捕获了含义、情感等。

So "cat" becomes [0.2, -0.5, 0.8], "sat" becomes [0.3, 0.6, -0.2]. Cool, each word is represented by a series of numbers capturing different features of the word. The LLM model takes these numbers (embeddings) as input, uses a complex mathematical function (based on neural network) with billions of parameters and the output is a next word prediction.

所以"cat"变成 [0.2, -0.5, 0.8]，"sat"变成 [0.3, 0.6, -0.2]。很酷，每个词由一系列数字表示，这些数字捕获了词的不同特征。大语言模型将这些数字（嵌入）作为输入，使用一个具有数十亿参数的复杂数学函数（基于神经网络），输出是下一个词的预测。

These parameters or variables in a mathematical function capture so many things like grammar rules, which sequence of words makes sense, how to combine words. This is why when you throw a sentence into the model, the model has billions of parameters that are able to predict what the next word is.

这些数学函数中的参数或变量捕获了很多东西，如语法规则、哪些词序是有意义的、如何组合词语。这就是为什么当你把一个句子输入模型时，模型有数十亿个参数能够预测下一个词是什么。

## Training Process: Self-Supervision
## 训练过程：自监督

The billions of parameters in this function are what gets adjusted during training to make better word predictions. So you show the model a lot of text, you start with random parameter values, model makes prediction, adjusts parameters to improve predictions.

函数中的数十亿个参数在训练期间被调整以做出更好的词预测。所以你向模型展示大量文本，从随机参数值开始，模型做出预测，调整参数以改进预测。

What happens in practice: you literally train the model on millions of sentences or text examples. Starting with: "The cat sat on the mat", we show the model "The cat sat on the", we know the correct answer is "The cat sat on the mat" and we adjust parameters until it predicts the word "mat". If it predicts the word "box", that is wrong, so we keep adjusting parameters. Then we move on to next sentence "Paris the capital", we change parameter values until it comes up with the word "of" as the next prediction.

实际发生的情况：你字面上在数百万个句子或文本示例上训练模型。从"The cat sat on the mat"开始，我们向模型展示"The cat sat on the"，我们知道正确答案是"The cat sat on the mat"，然后我们调整参数直到它预测出单词"mat"。如果它预测的是单词"box"，那是错误的，所以我们继续调整参数。然后我们继续下一个句子"Paris the capital"，我们改变参数值直到它预测出单词"of"作为下一个预测。

### Self-Supervision: Why This is Revolutionary
### 自监督：为什么这是革命性的

Supervised machine learning (ML algorithms trained using labeled data) is expensive — if it costs 5 cents for one person to label one image, it would cost 50k to label a million images — and often not scalable. Language models use **self-supervision**. Say you train the model with the sentence "The cat sat on the mat", the model sees "The cat sat on the" and predicts "mat", so the original sentence itself provides the correct answer. So no human labeling is required.

监督式机器学习（使用标记数据训练的机器学习算法）是昂贵的——如果一个人标记一张图片需要 5 美分，那么标记一百万张图片将需要 5 万美元——而且通常不可扩展。语言模型使用**自监督**。假设你用句子"The cat sat on the mat"训练模型，模型看到"The cat sat on the"并预测"mat"，所以原始句子本身就提供了正确答案。因此不需要人工标记。

So self-supervised learning means language models can learn from text sequences without requiring any labeling. Since text sequences are everywhere (books, blog posts, articles, etc), there is massive amount of training data allowing models to scale up to become LLMs.

因此，自监督学习意味着语言模型可以从文本序列中学习，而不需要任何标记。由于文本序列无处不在（书籍、博客文章、文章等），有大量的训练数据允许模型扩展成为大语言模型。

Think about how many sentences we have — this is why the training is rigorous. Billions of parameters to adjust, millions of examples to learn from, takes weeks or months to train. Once we find parameters that can capture patterns from all examples, we are confident that the model can predict likely next words.

想想我们有多少句子——这就是为什么训练是严格的。需要调整数十亿个参数，从数百万个示例中学习，训练需要数周或数月。一旦我们找到能够从所有示例中捕获模式的参数，我们就有信心模型可以预测可能的下一个词。

Once we find values for these parameters, we're good — we can input any word, and we can easily predict the next word.

一旦我们找到这些参数的值，我们就准备好了——我们可以输入任何词，就可以轻松预测下一个词。

## Tokens
## 词元（Tokens）

These words that are transformed to numbers are known as **tokens**. Whenever you use LLMs hosted by other companies, you are paying per tokens.

这些被转换为数字的词被称为**词元（tokens）**。每当你使用其他公司托管的大语言模型时，你是按词元付费的。

![image](https://github.com/user-attachments/assets/7a7bd7e9-4134-4480-a22c-a71cc3872277)


## Transformer Architecture
## Transformer 架构

In 2017, Google released a paper called "Attention is All You Need." Transformer architecture was invented in this paper. The inventors themselves did not realize the breakthrough they made. In 2018, GPT-1 was released, then GPT-2 in 2019, GPT-3 in 2020, and then in 2022 ChatGPT was released leveraging GPT-3 in combination with reinforcement learning from human feedback.

2017 年，Google 发布了一篇名为"Attention is All You Need"的论文。Transformer 架构就是在这篇论文中发明的。发明者自己都没有意识到他们取得的突破。2018 年，GPT-1 发布，然后 2019 年是 GPT-2，2020 年是 GPT-3，然后在 2022 年 ChatGPT 发布，它利用 GPT-3 结合人类反馈的强化学习。

OpenAI's first GPT model had 117M parameters, now it is trillions.

OpenAI 的第一个 GPT 模型有 1.17 亿个参数，现在是数万亿。

## Why Large Language Models Are General Purpose
## 为什么大语言模型是通用的

Language models, thanks to their scale, are capable of a wide range of tasks. Previous models were specific to specific tasks like sentiment analysis or translation (language models can do both). If you're a retailer and want to generate product descriptions, you can use a model that generates accurate descriptions but might fail to capture the brand's voice or messaging. With language models, you can craft detailed instructions with examples of desired product descriptions, you can ground it in specific data or even fine-tune it.

由于其规模，语言模型能够完成广泛的任务。以前的模型是针对特定任务的，如情感分析或翻译（语言模型可以同时做到两者）。如果你是零售商，想要生成产品描述，你可以使用一个生成准确描述的模型，但它可能无法捕获品牌的声音或信息。使用语言模型，你可以制作详细的指令，并提供所需产品描述的示例，你可以将其建立在特定数据上，甚至可以对其进行微调。
