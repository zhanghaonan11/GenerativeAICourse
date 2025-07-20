# Introduction to AI
# AI 简介

## What is AI?
## 什么是 AI？

Sometimes, when we hear the word too many times, we don't question what it actually means. What is AI? To explain that, let's go back in history.
有时，当我们听一个词太多次时，我们就不再质疑它的实际含义。什么是 AI？为了解释这一点，让我们回顾一下历史。

In the past, in order for computers to do something — show an Instagram post, allow us to buy a product on Amazon, calculate our taxes — we had to program the computer by giving it explicit instructions. So we as humans programmed the application step by step to solve these problems. What that means is, we solved the problem first as humans, and then gave these instructions to the computer.
过去，为了让计算机做某事——显示 Instagram 帖子、允许我们在亚马逊上购买产品、计算我们的税款——我们必须通过给计算机明确的指令来对它进行编程。所以我们作为人类一步一步地对应用程序进行编程来解决这些问题。这意味着，我们首先作为人类解决了问题，然后将这些指令提供给计算机。

With AI, instead of us solving the problem first, we teach the machine to solve the problem by giving it a bunch of different examples or data.
有了 AI，我们不再是先解决问题，而是通过给机器一堆不同的例子或数据来教它解决问题。

A very basic example I always give: say we have an app that predicts house prices based on size. We can write that logic ourselves, say the house price is always the size times 3. But what if instead of us writing the formula, we showed the computer millions of examples or data points of house prices and sizes? The computer figures out from all these data points, "Oh, actually it's not just the size times 3, maybe it's size times 1.5." Even if you showed me as a human millions of examples of house prices and size, I will come up with a better formula than just guessing that the house price is equal to size times 3, but of course humans can't look at millions of examples.
我总是举一个非常基本的例子：假设我们有一个根据房屋大小预测房价的应用程序。我们可以自己编写该逻辑，比如房价总是大小乘以 3。但是，如果我们不编写公式，而是向计算机展示数百万个房价和大小的示例或数据点呢？计算机会从所有这些数据点中得出结论，“哦，实际上不仅仅是大小乘以 3，也许是大小乘以 1.5。”即使你给我看数百万个房价和大小的例子，我也会想出一个比仅仅猜测房价等于大小乘以 3 更好的公式，但当然人类无法查看数百万个例子。

So AI is teaching the computer to learn from large datasets or examples. AI is excellent in cases where we want to look at data and solve problems. Let's take Instagram feed recommendation: without AI, we can let the human solve the problem (i.e., decide what to show you), or we can let AI see your browsing history, and based on that data, decide what to show you.
所以 AI 就是教计算机从大型数据集或示例中学习。在我们需要查看数据和解决问题的情况下，AI 非常出色。让我们以 Instagram 信息流推荐为例：没有 AI，我们可以让人类解决问题（即决定向您展示什么），或者我们可以让 AI 查看您的浏览历史，并根据该数据决定向您展示什么。

## AI Model
## AI 模型

I want to define what an AI model is. Because so far, we kept it at a high level — we said that AI is teaching computers to solve problems from large datasets.
我想定义一下什么是 AI 模型。因为到目前为止，我们一直把它保持在一个很高的水平上——我们说 AI 是在教计算机从大型数据集中解决问题。

What happens behind the scenes? Purely math. And don't be scared of math, because a mathematical function is nothing more than something that takes an input and produces an output. There are functions that double the input, so if you input 2, you get 4 (y = 2x). Some functions reverse the input, so if you put in 2, you get -2 (y = -x). These are all mathematical functions.
幕后发生了什么？纯粹是数学。不要害怕数学，因为数学函数只不过是接收一个输入并产生一个输出的东西。有些函数会将输入加倍，所以如果你输入 2，你会得到 4 (y = 2x)。有些函数会反转输入，所以如果你输入 2，你会得到 -2 (y = -x)。这些都是数学函数。

If we go back to our example of a function that predicts house price based on its size: instead of us writing this mathematical function, we show the computer millions of examples of house prices based on their size and we let it come up with the mathematical formula itself.
如果我们回到根据房屋大小预测房价的函数示例：我们不是自己编写这个数学函数，而是向计算机展示数百万个基于房屋大小的房价示例，然后让它自己得出数学公式。

**That mathematical formula is the AI model.** So there is an AI model to predict house prices — that is a mathematical function to predict house prices. The input is house size, and output is house price. There is an AI model that puts the relevant content on your Instagram feed based on your browsing activities — that is another mathematical function trained specifically on Instagram browsing data. The input is browsing activities, the output is the Instagram post. But again, these are functions that computers came up with based on millions of data examples.
**那个数学公式就是 AI 模型。** 所以有一个 AI 模型来预测房价——那是一个预测房价的数学函数。输入是房屋大小，输出是房价。有一个 AI 模型根据您的浏览活动将相关内容放在您的 Instagram 信息流中——那是另一个专门针对 Instagram 浏览数据训练的数学函数。输入是浏览活动，输出是 Instagram 帖子。但同样，这些是计算机根据数百万个数据示例得出的函数。

More precisely in practice, what happens is that humans choose the base mathematical function. So for example, we know that the house price is proportional to the size (the bigger the house, the more expensive it is), so we can model it using a linear function like (y = ax + b). Then the computer looks at all of the examples of input and output pairs (house prices and their size) then determines what the most appropriate values for a and b are (these are called **parameters**). Previously, we just guessed that the house price is 3 times its size (y = 3x + 0), but with AI, we let it look at multiple examples and come up with accurate values for a and b.
更准确地说，在实践中，发生的事情是人类选择基础数学函数。例如，我们知道房价与大小成正比（房子越大，越贵），所以我们可以用线性函数（y = ax + b）来建模。然后计算机查看所有输入和输出对的示例（房价及其大小），然后确定 a 和 b 的最合适值是什么（这些被称为**参数**）。以前，我们只是猜测房价是其大小的 3 倍（y = 3x + 0），但有了 AI，我们让它查看多个示例并得出 a 和 b 的准确值。

## Types of AI
## AI 的类型

There are 3 primary methods of teaching the computer from a large subset of data:
从大量数据子集中教计算机有 3 种主要方法：

**Supervised Learning, Unsupervised Learning, and Reinforcement Learning.**
**监督学习、无监督学习和强化学习。**

### 1. Supervised Learning
### 1. 监督学习

Supervised learning is the first and most common approach to train AI models. We teach the machine to solve problems by giving it a set of labeled data — not just any data, **labeled data**. Let's use the house size predictor as an example. To teach the computer, we give it a series of input/output pairs of house size and house price, all of that data is labeled. It says, "For this house size, this is the price," and we have millions of such data. The computer keeps trying different formulas, compares it with the set of labeled data until it arrives at the perfect formula. This is what we call training the AI or ML model — this is why it is expensive to train AI models because the computer keeps trying different formulas for millions of datasets. But now we have an AI model that predicts house prices based on size.
监督学习是训练 AI 模型的第一个也是最常见的方法。我们通过给机器一组标记数据来教它解决问题——不仅仅是任何数据，而是**标记数据**。让我们以房屋大小预测器为例。为了教计算机，我们给它一系列房屋大小和房价的输入/输出对，所有这些数据都是标记的。它说，“对于这个房屋大小，这是价格”，我们有数百万这样的数据。计算机不断尝试不同的公式，将其与标记数据集进行比较，直到得出完美的公式。这就是我们所说的训练 AI 或 ML 模型——这就是为什么训练 AI 模型很昂贵，因为计算机不断为数百万个数据集尝试不同的公式。但现在我们有了一个可以根据大小预测房价的 AI 模型。

If we want to build an AI model, a mathematical function that can predict what picture is a dog or cat, we give it millions of labeled pictures of dogs and cats. It's all labeled, hence why it is called **supervised**. We as humans did the hard work of labeling the data, and hence are supervising the machine to learn from that data.
如果我们想构建一个 AI 模型，一个可以预测图片是狗还是猫的数学函数，我们会给它数百万张标记好的狗和猫的图片。所有这些都是标记好的，因此被称为**监督式**。我们作为人类完成了标记数据的艰苦工作，因此正在监督机器从这些数据中学习。

![image](https://github.com/user-attachments/assets/4c1b4c17-b1f6-4070-b528-689c6b681668)




### 2. Unsupervised Learning
### 2. 无监督学习

Unsupervised learning is when the data only has inputs with no outputs. We are not trying to predict something like whether an email is spam; instead, we are simply discovering patterns in the data. Shopping data is the easiest example to think of. The computer can discover patterns from this massive data and conclude that customers who buy bread often buy milk. This data is not labeled or doesn't have outputs — it is simply buying behavior consisting of information like "this customer bought eggs, this bought oats, this bought milk," and from all that data, it discovers patterns.
无监督学习是指数据只有输入而没有输出。我们不是在尝试预测像电子邮件是否是垃圾邮件这样的事情；相反，我们只是在数据中发现模式。购物数据是最容易想到的例子。计算机可以从这些海量数据中发现模式，并得出结论，购买面包的顾客通常也会购买牛奶。这些数据没有标记或没有输出——它只是购买行为，包含诸如“这位顾客买了鸡蛋，这位买了燕麦，这位买了牛奶”之类的信息，并从所有这些数据中发现模式。

So supervised learning is: "Here's house size and price, learn to predict prices based on size." Unsupervised is: "Here's shopping data, find interesting patterns."
所以监督学习是：“这是房屋大小和价格，学习根据大小预测价格。”无监督学习是：“这是购物数据，寻找有趣的模式。”

On Amazon, product recommendations ("customers who bought x also bought y") is an example of an unsupervised ML model. The next time you're on Netflix, you are bombarded with unsupervised ML models telling you "people who watched this series also watched this."
在亚马逊上，产品推荐（“购买了 x 的顾客也购买了 y”）是无监督机器学习模型的一个例子。下次您在 Netflix 上时，您会看到大量无监督机器学习模型告诉您“看过这个系列的人也看过这个”。

![image](https://github.com/user-attachments/assets/c5538fab-6499-4179-be97-b74a1761fd0e)



### 3. Reinforcement Learning
### 3. 强化学习

Instead of learning from labeled data or finding patterns in unlabeled data, reinforcement learning is about learning through trial and error. It's literally like training a dog. When a dog does something good, you give it a treat. Over time, the dog learns which actions lead to treats and which don't.
强化学习不是从标记数据中学习或在未标记数据中寻找模式，而是通过试错来学习。这就像训练一只狗。当狗做了好事，你给它一个奖励。随着时间的推移，狗会学会哪些行为会带来奖励，哪些不会。

In reinforcement learning, the ML model takes actions, and based on these actions, it receives rewards or penalties. As an example, an AI chess player uses reinforcement learning. It played millions of matches and received positive rewards for winning moves and negative rewards for losing moves. Over time, the machine discovers strategies that human players never considered. Self-driving vehicles is another example. A self-driving car gets rewards for staying in its lane, maintaining safe distances, and reaching its destination.
在强化学习中，机器学习模型采取行动，并根据这些行动获得奖励或惩罚。例如，人工智能国际象棋棋手使用强化学习。它下了数百万场比赛，并因获胜的棋步而获得正奖励，因失败的棋步而获得负奖励。随着时间的推移，机器发现了人类棋手从未考虑过的策略。自动驾驶汽车是另一个例子。自动驾驶汽车因保持在车道内、保持安全距离和到达目的地而获得奖励。

When we talk about rewards, of course we're not giving the computer a treat. The reward is simply a numerical value that tells the algorithm whether an action was good or bad. So positive numbers indicate good actions, negative numbers indicate bad actions.
当我们谈论奖励时，我们当然不是给电脑奖励。奖励只是一个数值，它告诉算法一个动作是好是坏。所以正数表示好的动作，负数表示坏的动作。

With reinforcement learning, we're not giving the AI a dataset to study upfront. Instead, we're placing it in an environment (real or simulated) where it takes actions, results are observed, and it either gets a reward or penalty.
对于强化学习，我们不是预先给 AI 一个数据集来研究。相反，我们把它放在一个环境（真实的或模拟的）中，让它采取行动，观察结果，然后它会得到奖励或惩罚。

With AI models like ChatGPT, the model is trained using a supervised model where it is trained on millions of labeled data like books or websites. The model then uses reinforcement learning where it generates multiple possible responses to a given prompt, and these responses are rated based on how helpful or accurate they are either by humans or by another AI system. The model receives these ratings as rewards.
对于像 ChatGPT 这样的 AI 模型，该模型是使用监督模型进行训练的，该模型在数百万个标记数据（如书籍或网站）上进行训练。然后，该模型使用强化学习，其中它针对给定的提示生成多个可能的响应，并且这些响应根据其有用性或准确性由人类或另一个 AI 系统进行评级。该模型将这些评级作为奖励。

![image](https://github.com/user-attachments/assets/d72c893d-1390-4583-8007-50aa0c6bf6d1)



## Why AI Became Mainstream
## 为什么 AI 变得主流

Did you know that AI has been around since the 1950s? The question is, why did it suddenly become mainstream? We've had Google Translate for a long time, which is AI. We had fraud detection in credit cards, which is AI. We had Instagram feed algorithms for a long time. Why did it suddenly kick off?
您知道人工智能自 20 世纪 50 年代就已存在吗？问题是，为什么它突然变得主流？我们很早就有了谷歌翻译，这就是人工智能。我们有信用卡欺诈检测，这也是人工智能。我们很早就有了 Instagram 信息流算法。为什么它突然火起来了？

Sure, products like ChatGPT became viral, but here's the actual reason: In order for AI to work, you need 3 things:
当然，像 ChatGPT 这样的产品变得病毒式传播，但真正的原因是：为了让 AI 发挥作用，你需要 3 样东西：

1. **Algorithms**
1. **算法**
2. **Computers** 
2. **计算机**
3. **Data**
3. **数据**

What companies like OpenAI did is they created a general-purpose model, exposed them to the public that anyone can use. And these models understood natural language really well, and it turns out that models that can understand natural language are general-purpose models, meaning they can be used to solve many everyday use cases like writing code, customer support, etc.
像 OpenAI 这样的公司所做的是，他们创建了一个通用的模型，并向公众开放，任何人都可以使用。这些模型对自然语言的理解非常好，事实证明，能够理解自然语言的模型是通用的模型，这意味着它们可以用来解决许多日常用例，比如编写代码、客户支持等。

In the past, you had to train an army of machine learning engineers and invest in massive amounts of infrastructure to train your own AI model.
过去，您必须培训一支机器学习工程师大军，并投入大量基础设施来训练自己的 AI 模型。

So what happened now is, every company can now become an AI company.
所以现在发生的是，每家公司现在都可以成为一家 AI 公司。