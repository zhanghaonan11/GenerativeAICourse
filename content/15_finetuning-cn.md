# Fine-Tuning
# 微调

## The Fundamental Difference
## 根本区别

Prompt-based methods adapt the model by giving it instructions, but fine-tuning adapts the model by adjusting its weights or parameters.
基于提示的方法通过向模型提供指令来调整模型，而微调则通过调整其权重或参数来调整模型。

It is usually used for domain-specific capabilities like coding or medical applications.
它通常用于特定领域的功能，例如编码或医疗应用。

Although fine-tuning is powerful, it can be expensive and challenging.
尽管微调功能强大，但它可能既昂贵又具有挑战性。

![image](https://github.com/user-attachments/assets/a5c5a9e8-820c-4173-9bec-eedb36987f8f)
![图片](https://github.com/user-attachments/assets/a5c5a9e8-820c-4173-9bec-eedb36987f8f)


## Why Fine-Tuning Works
## 为什么微调有效

In practice, most people usually fine-tune smaller models because they require less memory. A small model, fine-tuned on a specific task, might outperform a much larger out-of-the-box model on that task.
在实践中，大多数人通常会微调较小的模型，因为它们需要较少的内存。在特定任务上微调的小型模型，在该任务上的性能可能会超过开箱即用的更大型模型。

The main challenge with fine-tuning is acquiring annotated data, which can be slow and expensive to acquire, especially for domain-specific areas. Some people rely on synthetic or AI-generated data, but their effectiveness is highly variable.
微调的主要挑战是获取带注释的数据，这可能既缓慢又昂贵，尤其是在特定领域。有些人依赖合成或 AI 生成的数据，但其有效性差异很大。

**You should always start with rigorous prompt experiments before even thinking about fine-tuning.**
**在考虑微调之前，您应该始终从严格的提示实验开始。**

## Fine-Tuning Tactics
## 微调策略

While many things around fine-tuning - deciding whether to fine-tune, acquiring data, and maintaining fine-tuned models - are hard, the actual process of fine-tuning is more straightforward. There are three things you need to choose: a base model, a fine-tuning method, and a framework for fine-tuning.
虽然围绕微调的许多事情——决定是否微调、获取数据和维护微调模型——都很困难，但微调的实际过程更为直接。您需要选择三件事：基础模型、微调方法和微调框架。

### Base Models: Starting Points Matter
### 基础模型：起点很重要

At the beginning of an AI project, when you're still exploring the feasibility of your task, it's useful to start with the most powerful model you can afford. If this model struggles to produce good results, weaker models are likely to perform even worse. If the strongest model meets your needs, you can then explore weaker models, using the initial model as a benchmark for comparison.
在 AI 项目的开始阶段，当您仍在探索任务的可行性时，从您能负担得起的最强大的模型开始是很有用的。如果这个模型难以产生好的结果，那么较弱的模型可能会表现得更差。如果最强的模型满足您的需求，那么您就可以探索较弱的模型，并使用初始模型作为比较的基准。

For fine-tuning, the starting models vary for different projects. There are two main development paths:
对于微调，不同的项目，起始模型也不同。主要有两条开发路径：

#### The Progression Path
#### 渐进路径
1. **Test your fine-tuning code** using the cheapest and fastest model to make sure the code works as expected
1. **测试您的微调代码**，使用最便宜、最快的模型来确保代码按预期工作
2. **Test your data** by fine-tuning a middling model. If the training loss doesn't go down with more data, something might be wrong
2. **测试您的数据**，通过微调一个中等模型。如果训练损失没有随着更多数据而下降，则可能出了问题
3. **Run experiments** with the best model to see how far you can push performance
3. **使用最佳模型进行实验**，看看您可以将性能推向多远
4. **Map the frontier:** Once you have good results, do a training run with all models to map out the price/performance frontier and select the model that makes the most sense for your use case
4. **绘制前沿图：** 一旦您获得了好的结果，就对所有模型进行一次训练，以绘制出价格/性能前沿图，并选择最适合您用例的模型

#### The Distillation Path
#### 蒸馏路径
1. **Start strong:** Begin with a small dataset and the strongest model you can afford. Train the best possible model with this small dataset. Because the base model is already strong, it requires less data to achieve good performance
1. **强势开局：** 从一个小数据集和您能负担得起的最强模型开始。用这个小数据集训练出最好的模型。因为基础模型已经很强大，所以它需要更少的数据就能达到很好的性能
2. **Generate more data:** Use this fine-tuned model to generate more training data
2. **生成更多数据：** 使用这个微调模型生成更多训练数据
3. **Train cheaper:** Use this new dataset to train a cheaper model
3. **更便宜地训练：** 使用这个新数据集训练一个更便宜的模型

Because fine-tuning usually comes after experiments with prompt engineering, by the time you start to fine-tune, ideally, you should have a pretty good understanding of different models' behaviors. You should plan your fine-tuning development path based on this understanding.
因为微调通常是在提示工程实验之后进行的，所以当您开始微调时，理想情况下，您应该对不同模型的行为有很好的了解。您应该根据这种理解来规划您的微调开发路径。

### Fine-Tuning Methods: Full vs. Adapter Techniques
### 微调方法：完整微调与适配器技术

Adapter techniques like LoRA are cost-effective but typically don't deliver the same level of performance as full fine-tuning. If you're just starting with fine-tuning, try something like LoRA, and attempt full fine-tuning later.
像 LoRA 这样的适配器技术具有成本效益，但通常无法提供与完整微调相同的性能水平。如果您刚开始进行微调，请尝试像 LoRA 这样的方法，稍后再尝试完整微调。

The fine-tuning methods to use also depend on your data volume. Depending on the base model and the task, full fine-tuning typically requires at least thousands of examples and often many more. PEFT (Parameter Efficient Fine-Tuning) methods, however, can show good performance with a much smaller dataset. If you have a small dataset, such as a few hundred examples, full fine-tuning might not outperform LoRA.
使用的微调方法也取决于您的数据量。根据基础模型和任务，完整微调通常需要至少数千个示例，而且通常更多。然而，PEFT（参数高效微调）方法可以在更小的数据集上表现出良好的性能。如果您有一个小数据集，例如几百个示例，完整微调可能不会优于 LoRA。

Take into account how many fine-tuned models you need and how you want to serve them when deciding on a fine-tuning method. Adapter-based methods like LoRA allow you to more efficiently serve multiple models that share the same base model. With LoRA, you only need to serve a single full model, whereas full fine-tuning requires serving multiple full models.
在决定微调方法时，请考虑您需要多少个微调模型以及您希望如何为它们提供服务。像 LoRA 这样的基于适配器的方法允许您更有效地为共享相同基础模型的多个模型提供服务。使用 LoRA，您只需要为一个完整的模型提供服务，而完整微调则需要为多个完整的模型提供服务。

### Fine-Tuning Frameworks: APIs vs. Custom Solutions
### 微调框架：API 与自定义解决方案

**Fine-tuning APIs** are the easiest way to fine-tune. You upload data, select a base model, and get back a fine-tuned model. Like model inference APIs, fine-tuning APIs can be provided by model providers, cloud service providers, and third-party providers. 
**微调 API** 是最简单的微调方法。您上传数据，选择一个基础模型，然后取回一个微调模型。与模型推理 API 一样，微调 API 可以由模型提供商、云服务提供商和第三方提供商提供。

Limitations of this approach:
此方法的局限性：
- You're limited to the base models that the API supports
- 您仅限于 API 支持的基础模型
- The API might not expose all the knobs you can use for optimal fine-tuning performance
- API 可能不会公开所有可用于优化微调性能的旋钮

Fine-tuning APIs are suitable for those who want something quick and easy, but they might be frustrating for those who want more customization.
微调 API 适合那些想要快速简便的人，但对于那些想要更多定制的人来说，它们可能会令人沮丧。

**Custom frameworks** give you more flexibility. You can fine-tune using frameworks like LLaMA-Factory, unsloth, PEFT, Axolotl, and LitGPT. They support a wide range of fine-tuning methods, especially adapter-based techniques. If you want to do full fine-tuning, many base models provide their open source training code on GitHub that you can clone and run with your own data.
**自定义框架**为您提供更大的灵活性。您可以使用 LLaMA-Factory、unsloth、PEFT、Axolotl 和 LitGPT 等框架进行微调。它们支持广泛的微调方法，尤其是基于适配器的技术。如果您想进行完整微调，许多基础模型都在 GitHub 上提供了它们的开源训练代码，您可以克隆并使用自己的数据运行。

Doing your own fine-tuning gives you more flexibility, but you'll have to provision the necessary compute. If you do only adapter-based techniques, a mid-tier GPU might suffice for most models. If you need more compute, you can choose a framework that integrates seamlessly with your cloud provider.
自己进行微调可以为您提供更大的灵活性，但您必须配置必要的计算资源。如果您只使用基于适配器的技术，那么中端 GPU 可能足以满足大多数模型的需求。如果您需要更多计算资源，您可以选择一个与您的云提供商无缝集成的框架。

To fine-tune a model using more than one machine, you'll need a framework that helps you do distributed training, such as DeepSpeed, PyTorch Distributed, and ColossalAI.
要使用多台机器微调模型，您需要一个可以帮助您进行分布式训练的框架，例如 DeepSpeed、PyTorch Distributed 和 ColossalAI。

## Fine-Tuning Hyperparameters
## 微调超参数

Depending on the base model and the fine-tuning method, there are many hyperparameters you can tune to improve fine-tuning efficiency. Here are the most important ones:
根据基础模型和微调方法，您可以调整许多超参数来提高微调效率。以下是最重要的几个：

### Learning Rate: The Step Size Problem
### 学习率：步长问题

The learning rate determines how fast the model's parameters should change with each learning step. If you think of learning as finding a path toward a goal, the learning rate is the step size. If the step size is too small, it might take too long to get to the goal. If the step size is too big, you might overstep the goal, and the model might never converge.
学习率决定了模型的参数在每个学习步骤中应该改变多快。如果您将学习视为寻找通往目标的路径，那么学习率就是步长。如果步长太小，可能需要很长时间才能达到目标。如果步长太大，您可能会越过目标，模型可能永远不会收敛。

A universal optimal learning rate doesn't exist. You'll have to experiment with different learning rates, typically between the range of 1e-7 to 1e-3, to see which one works best. A common practice is to take the learning rate at the end of the pre-training phase and multiply it with a constant between 0.1 and 1.
不存在通用的最佳学习率。您必须尝试不同的学习率，通常在 1e-7 到 1e-3 的范围内，才能找到最有效的学习率。一种常见的做法是在预训练阶段结束时取学习率，并将其乘以 0.1 到 1 之间的常数。

**Reading the loss curve:** If the loss curve fluctuates a lot, it's likely that the learning rate is too big. If the loss curve is stable but takes a long time to decrease, the learning rate is likely too small. Increase the learning rate as high as the loss curve remains stable.
**读取损失曲线：** 如果损失曲线波动很大，则很可能学习率过大。如果损失曲线稳定但下降缓慢，则学习率可能过小。将学习率提高到损失曲线保持稳定的最高水平。

You can vary learning rates during the training process using **learning rate schedules** — algorithms that determine how learning rates should change throughout the training process (larger learning rates in the beginning, smaller near the end).
您可以在训练过程中使用**学习率调度**来改变学习率——这些算法决定了学习率在整个训练过程中应该如何变化（开始时学习率较大，接近结束时学习率较小）。

### Batch Size: Memory vs. Stability Trade-off
### 批量大小：内存与稳定性的权衡

The batch size determines how many examples a model learns from in each step to update its weights. A batch size that is too small, such as fewer than eight, can lead to unstable training. A larger batch size helps aggregate the signals from different examples, resulting in more stable and reliable updates.
批量大小决定了模型在每个步骤中从多少个示例中学习以更新其权重。批量大小太小（例如少于 8 个）可能会导致训练不稳定。较大的批量大小有助于聚合来自不同示例的信号，从而产生更稳定、更可靠的更新。

In general, the larger the batch size, the faster the model can go through training examples. However, the larger the batch size, the more memory is needed to run your model. Thus, batch size is limited by the hardware you use.
一般来说，批量越大，模型遍历训练示例的速度就越快。但是，批量越大，运行模型所需的内存就越多。因此，批量大小受您使用的硬件的限制。

This is where you see the cost versus efficiency trade-off. More expensive compute allows faster fine-tuning.
这就是您看到成本与效率权衡的地方。更昂贵的计算可以实现更快的微调。

**Gradient accumulation:** As of this writing, compute is still a bottleneck for fine-tuning. Often, models are so large, and memory is so constrained, that only small batch sizes can be used. This can lead to unstable model weight updates. To address this, instead of updating the model weights after each batch, you can accumulate gradients across several batches and update the model weights once enough reliable gradients are accumulated.
**梯度累积：** 在撰写本文时，计算仍然是微调的瓶颈。通常，模型非常大，内存非常有限，因此只能使用小批量。这可能导致不稳定的模型权重更新。为了解决这个问题，您可以不在每个批次之后更新模型权重，而是在多个批次中累积梯度，并在累积了足够可靠的梯度后一次性更新模型权重。

### Number of Epochs: How Many Times to See Each Example
### 周期数：每个示例被看到的次数

An epoch is a pass over the training data. The number of epochs determines how many times each training example is trained on.
一个周期是对训练数据的一次遍历。周期数决定了每个训练示例被训练的次数。

Small datasets may need more epochs than large datasets. For a dataset with millions of examples, 1–2 epochs might be sufficient. A dataset with thousands of examples might still see performance improvement after 4–10 epochs.
小数据集可能比大数据集需要更多的周期。对于包含数百万个示例的数据集，1-2 个周期可能就足够了。包含数千个示例的数据集在 4-10 个周期后仍可能看到性能提升。

**Using loss curves to guide epochs:** If both the training loss and the validation loss still steadily decrease, the model can benefit from more epochs (and more data). If the training loss still decreases but the validation loss increases, the model is overfitting to the training data, and you might try lowering the number of epochs.
**使用损失曲线指导周期：** 如果训练损失和验证损失都仍在稳步下降，则模型可以从更多的周期（和更多的数据）中受益。如果训练损失仍在下降但验证损失在增加，则模型正在对训练数据过拟合，您可以尝试减少周期数。

### Prompt Loss Weight: Learning From Instructions vs. Responses
### 提示损失权重：从指令与响应中学习

For instruction fine-tuning, each example consists of a prompt and a response, both of which can contribute to the model's loss during training. During inference, however, prompts are usually provided by users, and the model only needs to generate responses. Therefore, response tokens should contribute more to the model's loss during training than prompt tokens.
对于指令微调，每个示例都包含一个提示和一个响应，这两者都会在训练期间对模型的损失产生影响。然而，在推理过程中，提示通常由用户提供，模型只需要生成响应。因此，在训练期间，响应令牌对模型损失的贡献应该大于提示令牌。

The prompt loss weight determines how much prompts should contribute to this loss compared to responses. If this weight is 100%, prompts contribute to the loss as much as responses, meaning that the model learns equally from both. If this weight is 0%, the model learns only from responses. Typically, this weight is set to 10% by default, meaning that the model should learn some from prompts but mostly from responses.
提示损失权重决定了与响应相比，提示应该对该损失贡献多少。如果此权重为 100%，则提示对损失的贡献与响应相同，这意味着模型从两者中学习的程度相同。如果此权重为 0%，则模型仅从响应中学习。通常，此权重默认设置为 10%，这意味着模型应该从提示中学习一些，但主要从响应中学习。