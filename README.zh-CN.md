# Vibe Coding 终极指南 V1.2
**作者：** [Nicolas Zullo, https://x.com/NicolasZu](https://x.com/NicolasZu)  
**创建日期：** 2025 年 3 月 12 日  
**最近更新日期：** 2025 年 10 月 06 日  

---

## 快速开始
开始 vibe coding，你只需要以下任一工具：  
- **Claude Sonnet 4.5**，在 Claude Code 中使用
- **gpt-5-codex (high)**，在 Codex CLI 中使用

本指南同时适用于 CLI 版本（在终端使用）和 VSCode 扩展版本（Codex 与 Claude Code 都有，并且界面更新）。  

*注：本指南早期使用过 **Grok 3**，后来转向 **Gemini 2.5 Pro**，现在使用 **Claude 4.5**（或 **gpt-5-codex (high)**）。*  

*注 2：如果想用 Cursor，请查看[1.1 版本](https://github.com/EnzeD/vibe-coding/tree/1.1.1)的指南，但我们认为它不如 Codex CLI 或 Claude Code 强大。*  

要认真做出完整、好看的游戏（或应用），先把基础打牢。  

**关键原则：** *规划为一切。* 不要让 AI 自主规划，否则代码库会变得难以管理。

---

## 环境配置

### 1. 游戏设计文档
- 将你的游戏想法交给 **GPT-5** 或 **Sonnet 4.5**，让它生成一个简洁的 **Game Design Document**（Markdown）：`game-design-document.md`。  
- 自行审阅和微调，使之符合你的愿景。保持简洁即可，目的是给 AI 传递结构和意图。无需过度设计，后续会迭代。

### 2. 技术栈与 `CLAUDE.md` / `Agents.md`
- 让 **GPT-5** 或 **Sonnet 4.5** 推荐最合适的技术栈（例如 ThreeJS + WebSocket 适合多人 3D 游戏），保存为 `tech-stack.md`。  
  - 要求它提出“最简单且最可靠”的方案。  
- 在终端打开 **Claude Code** 或 **Codex CLI**，运行 `/init`，它会使用目前已有的两份 .md 文件，为 LLM 生成规则。  
- **务必审阅这些规则。** 确保强调**模块化**（多文件）并避免**单体**（巨型单文件）。你可能需要手动调整或补充规则，也要检查它们的触发条件。  
  - **重要：** 部分规则应设为 **“Always”**，保证 AI 写代码前必读。例如：  
    > ```
    > # IMPORTANT:
    > # Always read memory-bank/@architecture.md before writing any code. Include entire database schema.
    > # Always read memory-bank/@game-design-document.md before writing any code.
    > # After adding a major feature or completing a milestone, update memory-bank/@architecture.md.
    > ```  
  - 其他（非“Always”）规则应引导 AI 遵循你的技术栈最佳实践（网络、状态管理等）。  
  - *如果想要最优化、最干净的代码库，这套规则是必须的。*

### 3. 实施计划
- 向 **GPT-5** 或 **Sonnet 4.5** 提供：  
  - 游戏设计文档 (`game-design-document.md`)  
  - 技术栈推荐 (`tech-stack.md`)  
- 让它生成详细的 **实施计划**（Markdown，`.md`）：为 AI 开发者准备的逐步指令。  
  - 步骤要小而具体。  
  - 每步都需包含验证是否正确的测试。  
  - 不要写代码，只要清晰的执行说明。  
  - 只聚焦基础游戏（核心循环），完整功能后面迭代。

### 4. Memory Bank
- 为项目创建新文件夹并在 VSCode 中打开。  
- 在项目中创建 `memory-bank` 子目录。  
- 在 `memory-bank` 内添加：  
  - `game-design-document.md`  
  - `tech-stack.md`  
  - `implementation-plan.md`  
  - `progress.md`（空文件，用于记录已完成步骤）  
  - `architecture.md`（空文件，用于记录各文件用途）

---

## 开发基础版本

### 确保需求清晰
- 在 VSCode 扩展或终端中打开 **Codex** 或 **Claude Code**。  
- 提示词：阅读 `/memory-bank` 中所有文档，`implementation-plan.md` 是否清晰？要让它 100% 清晰，你需要哪些问题？  
- 它通常会问 9-10 个问题。回答后提示它据此改写 `implementation-plan.md`，让计划更完善。

### 第一个实现提示
- 在 VSCode 扩展或终端中打开 **Codex** 或 **Claude Code**。  
- 提示词：阅读 `/memory-bank` 中所有文档，执行实施计划的第 1 步。我来运行测试。在我验证测试前不要开始第 2 步。验证通过后，打开 `progress.md` 记录你做了什么，再将架构相关的洞察写入 `architecture.md`，解释各文件用途。  
- **务必**先用“Ask”模式或“Plan Mode”（Claude Code 中 `shift+tab`），确认后再让 AI 继续。

- **极致 vibe：** 安装 [Superwhisper](https://superwhisper.com) 用语音和 Claude 或 GPT-5 交流。

### 工作流程
- 完成第 1 步后：  
- 提交 Git（不会的话让 AI 帮忙）。  
- 开启新对话（`/new` 或 `/clear`）。  
- 提示词：现在阅读 memory-bank 中所有文件，查看 progress.md 理解已完成的工作，然后执行第 2 步。验证测试前不要开始第 3 步。  
- 重复此流程直到完成整个 `implementation-plan.md`。

---

## 添加细节
恭喜你完成了基础游戏！它可能还很粗糙，但现在可以尝试和打磨：  
- 想要雾效、后处理、特效或音效？更好的飞机/汽车/城堡？漂亮的天空？  
- 每个大特性创建新的 `feature-implementation.md`，写下简短步骤和测试。  
- 按步骤实现并测试，逐步迭代。

---

## 修复 Bug 与卡壳
- 如果提示失败或破坏了游戏：  
- 在 Claude Code 中使用 `/rewind`，改进提示再试。如果用 GPT-5，可以多次提交并在需要时 reset。  
- 遇到错误：  
    - **JavaScript：** 打开控制台（F12），复制错误，粘贴到 VSCode，如果有视觉问题提供截图。  
    - **懒人选项：** 安装 [BrowserTools](https://browsertools.agentdesk.ai/installation) 省去手动复制/截图。  
- 如果卡住：  
    - 回退到上一次 Git 提交（`git reset`），然后用新提示重试。  
- 如果仍然卡住：  
    - 使用 [RepoPrompt](https://repoprompt.com/) 或 [uithub](https://uithub.com/) 把整个代码库收集到一个文件，再让 **GPT-5** 或 **Claude** 帮忙。

---

## Claude Code & Codex 提示
- **终端中的 Codex CLI 或 Claude Code：** 在 VSCode 终端运行，可在不离开 IDE 的情况下查看 diff、提供额外上下文。  
- **Claude Code `/rewind`：** 如果本次迭代偏离预期，用它回滚到之前状态。  
- **自定义 Claude Code 命令：** 例如 `/explain $arguments`，触发类似提示：“深度解析 $arguments 的代码并理解其工作原理，理解后告诉我，我再给你任务。” 这样模型会先获取足够上下文再修改。  
- **清理上下文：** 经常用 `/clear` 或 `/compact` 清理上下文，如果仍需之前的内容再重新加载。  
- **省时间（自行承担风险）：** 使用 `claude --dangerously-skip-permissions` 或 `codex --yolo`，让 Claude Code 或 Codex CLI 不再反复确认。

---

## 其他提示
- **小改动：** 用 GPT-5 (medium)  
- **营销文案：** 用 Opus 4.1  
- **生成高质量 2D 精灵图：** 用 ChatGPT 和 Nano Banana  
- **生成音乐：** 用 Suno  
- **生成音效：** 用 ElevenLabs  
- **生成视频：** 用 Sora 2  
- **更好的提示输出：**  
    - 添加 “think as long as needed to get this right, I am not in a hurry. What matters is that you follow precisely what I ask you and execute it perfectly. Ask me questions if I am not precise enough."  
    - 对 Claude Code，用特定短语触发更深入的推理：`think` < `think hard` < `think harder` < `ultrathink`.

---

## 常见问题
**问：我在做应用不是游戏，流程一样吗？**  
**答：** 基本一致！把 GDD（Game Design Document）换成 PRD（Product Requirements Document）。你还可以先用 v0、Lovable 或 Bolt.new 快速原型，然后把代码迁到 GitHub，再克隆到 VSCode 或终端继续用本指南。  

**问：你狗斗游戏的飞机很棒，但我一条提示做不出来！**  
**答：** 不是一条提示，而是 ~30 条提示，由特定的 `plane-implementation.md` 引导。用精确的提示，比如“在机翼上预留副翼的空间”，不要模糊地说“做一架飞机”。  

**问：为什么 Claude Code 或 Codex CLI 比 Cursor 好？**  
**答：** 看个人喜好。我们强调 Claude Code 适配 Claude Sonnet 4.5，Codex CLI 适配 GPT-5，都比 Cursor 对这两者的支持更好。跑在终端还能解锁更多开发工作流：任意 IDE、SSH 远程等；还能做自定义命令、子代理、hooks，随着时间推移提升质量和效率。如果你用低阶 Claude 或 ChatGPT 套餐，这也足够起步。  

**问：我不会搭建多人游戏的服务器怎么办？**  
**答：** 问你的 AI。

--- 
