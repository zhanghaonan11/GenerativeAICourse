# 生成式 AI 课程

这是您能找到的最全面的生成式 AI 课程。

我们将从最基本的第一性原理开始——什么是 AI，它是如何发明的，以及大型语言模型 (LLM) 的兴起。从那里，我们将进入动手教程，包括构建基本的聊天机器人、实现检索增强生成 (RAG)、创建 AI 代理、探索 MCP，以及了解构建生产级 AI 应用程序需要什么。

我曾与数百名客户合作，了解构建可扩展的真实世界 AI 解决方案的真正需要。本课程没有行话，没有废话——一切都从头开始解释。市面上大多数 AI 课程要么令人困惑，要么过于学术化，要么缺乏实践相关性。这个不一样。

我们正处在一个 AI 应用需求爆炸式增长的时代——而入门门槛比以往任何时候都低。这就是为什么 **AI 工程**——在强大、现成的模型之上构建应用程序的过程——是技术领域发展最快的学科之一。

在机器学习模型之上构建应用程序并不是什么新鲜事。早在 LLM 出现之前，我们就已经为欺诈检测、产品推荐等构建了 AI 应用程序。许多原则仍然适用，但 LLM 带来了新的挑战和机遇。

**传统机器学习和 AI 工程之间的主要区别：**
1. AI 工程更少关注训练模型，而更多地关注调整模型（例如提示工程、微调）。
2. AI 工程处理需要更多计算能力的更大型模型——这意味着更高的延迟和不同的基础设施需求。
3. AI 模型通常产生开放式输出，使得评估比传统机器学习更复杂。

**以下是我们将涵盖的内容快照：**
* 部署本地 LLM
* 构建端到端 AI 聊天机器人并管理上下文
* 提示工程
* 防御性提示和防止常见的 AI 漏洞利用
* 检索增强生成 (RAG)
* AI 代理和高级用例
* 模型上下文协议 (MCP)
* LLMOps
* 好的 AI 数据是什么样的

---

## 设置说明

请按照以下步骤设置您的开发环境并运行实验练习。

### 第 1 步：安装 Visual Studio Code

1. 前往 [https://code.visualstudio.com](https://code.visualstudio.com)
2. 单击适用于您操作系统的蓝色“下载”按钮：
   - **Windows**：单击“下载 Windows 版”
   - **Mac**：单击“下载 Mac 版”
   - **Linux**：单击“下载 Linux 版”
3. 运行下载的安装程序并按照安装提示进行操作
4. 安装完成后打开 VS Code

### 第 2 步：安装 Git

**Windows：**
1. 前往 [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. 下载并运行安装程序
3. 在整个安装过程中使用默认设置
4. 安装后重新启动 VS Code

**Mac：**
1. 打开终端（按 `Cmd + 空格`，输入“终端”，按 Enter）
2. 输入：`git --version`
3. 如果未安装 Git，它将提示您安装 Xcode 命令行工具 - 单击“安装”
4. 等待安装完成

**Linux：**
```bash
sudo apt update
sudo apt install git
```

### 第 3 步：克隆此存储库

**首先，获取存储库 URL：**
1. 在此 GitHub 页面的顶部（您正在阅读这些说明的地方），查找绿色的“**< > Code**”按钮
2. 单击绿色的“**< > Code**”按钮
3. 确保已选择“HTTPS”
4. 单击 URL 旁边的复制图标 (📋) 以复制它

**现在克隆存储库：**
1. 打开 VS Code
2. 按 `Ctrl+Shift+P` (Windows/Linux) 或 `Cmd+Shift+P` (Mac)
3. 输入“Git: Clone”并选择它
4. **粘贴您刚刚复制的 URL** (`Ctrl+V` 或 `Cmd+V`)
5. 在您的计算机上选择一个文件夹来保存项目（例如，桌面或文档）
6. 单击“克隆”
7. 当提示“您想打开克隆的存储库吗？”时，单击“打开”

**使用终端的替代方法：**
1. 从上面的绿色“Code”按钮复制存储库 URL
2. 打开终端/命令提示符
3. 导航到您想要项目的位置：`cd Desktop`（或您首选的位置）
4. 运行：`git clone [在此处粘贴 URL]`
5. 运行：`cd [repository-name]`（替换为创建的实际文件夹名称）
6. 运行：`code .` 以打开 VS Code

### 第 4 步：安装 Python 扩展

1. 在 VS Code 中，单击左侧边栏中的扩展图标（看起来像积木）
2. 搜索“Python”并安装 Microsoft 的官方 Python 扩展
3. 搜索“Jupyter”并安装 Microsoft 的 Jupyter 扩展

### 第 5 步：创建您的环境文件

1. 在 VS Code 中，您应该在左侧边栏中看到您的项目文件
2. **导航到 `content` 文件夹** - 单击 `content` 文件夹以打开它
3. 在 `content` 文件夹内右键单击并选择“新建文件”
4. 将文件命名为：`.env`（包括开头的点）
5. 打开 `.env` 文件并添加此行：
   ```
   OPENAI_API_KEY=your_openai_key_will_go_here
   ```
6. 保存文件（在 Windows/Linux 上为 `Ctrl+S`，在 Mac 上为 `Cmd+S`）

### 第 6 步：获取您的 OpenAI API 密钥

1. 前往 [https://platform.openai.com](https://platform.openai.com)
2. 如果您没有帐户，请单击“注册”，如果您有帐户，请单击“登录”
3. 完成注册过程（您需要验证您的电子邮件和电话号码）
4. 登录后，单击右上角的个人资料图标
5. 从下拉菜单中选择“查看 API 密钥”
6. 单击“创建新的密钥”
7. 给它一个像“AI Labs Project”这样的名字
8. **重要提示**：立即复制密钥 - 您将无法再次看到它
9. 返回 VS Code 并将 `.env` 文件中的 `your_openai_key_will_go_here` 替换为您的实际密钥
10. 保存 `.env` 文件

**您的 .env 文件现在应如下所示：**
```
OPENAI_API_KEY=sk-proj-abc123...your_actual_key_here
```

### 第 7 步：设置 Python 虚拟环境

1. 打开 VS Code 的集成终端：
   - 在 Windows/Linux 上按 `Ctrl+\` (反引号)
   - 在 Mac 上按 `Cmd+\` (反引号)
2. 创建虚拟环境：
   ```bash
   python -m venv venv
   ```
3. 激活虚拟环境：
   - **Windows**：`venv\Scripts\activate`
   - **Mac/Linux**：`source venv/bin/activate`
4. 您应该在终端提示符的开头看到 `(venv)`
5. 安装所需的包：
   ```
   pip install openai python-dotenv jupyter
   ```

### 第 8 步：测试您的设置

1. 在 VS Code 中，打开其中一个 `.ipynb` (Jupyter notebook) 文件
2. 如果提示选择内核，请从您的 `venv` 文件夹中选择 Python 解释器
3. 运行加载 API 密钥的第一个单元格 - 您应该会看到“✅ API key configured”
4. 如果您看到此消息，则说明您已准备好开始实验！

## 故障排除

**“找不到命令‘python’”：**
- 尝试使用 `python3` 代替 `python`
- 在 Windows 上，您可能需要从 [python.org](https://python.org) 安装 Python

**“没有名为‘openai’的模块”：**
- 确保您的虚拟环境已激活（您应该在终端中看到 `(venv)`）
- 再次运行 `pip install openai python-dotenv`

**API 密钥不起作用：**
- 仔细检查您的 `.env` 文件中没有多余的空格
- 确保文件名为 `.env`（而不是 `.env.txt`）
- 验证您的 OpenAI 帐户已设置计费

**需要帮助？**
- 检查您的 `.env` 文件是否在 `content` 文件夹中（与 notebook 文件在同一文件夹中）
- 确保在添加 API 密钥后已保存 `.env` 文件
- 如果问题仍然存在，请重新启动 VS Code

---

**安全说明**：切勿与任何人共享您的 `.env` 文件或 API 密钥。`.env` 文件应仅保留在您的计算机上。