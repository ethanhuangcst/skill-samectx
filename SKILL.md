# samectx

Context management tool - 智能整理对话上下文并保存到本地目录

[English](#english-version)

## 技能描述

该技能用于在使用 TRAE、Cursor、Claude Code 等 AI 工具时，自动分析对话内容，提取关键信息，并生成 `samectx sync` 命令，帮助用户保存对话上下文到本地项目目录，支持多用户协作和跨设备同步。

This skill automatically analyzes conversation content, extracts key information, and generates `samectx sync` commands to help users save conversation context to local project directories, supporting multi-user collaboration and cross-device synchronization when using AI tools like TRAE, Cursor, and Claude Code.

## 触发条件

- 用户提到「保存上下文」、「同步笔记」、「记录对话」等相关词汇
- 对话中包含明确的任务、决策或技术讨论
- 用户直接请求保存当前对话

## Trigger Conditions

- User mentions keywords like "save context", "sync notes", "record conversation", etc.
- Conversation contains clear tasks, decisions, or technical discussions
- User directly requests to save the current conversation

## 技能逻辑

1. **分析对话内容**：识别对话中的关键任务、关键点和关键决策
2. **提取关键信息**：从对话中提取重要的任务、技术要点和决策
3. **生成命令**：构建带参数的 `samectx sync` 命令
4. **提示用户**：显示分析结果并提供执行命令

## Skill Logic

1. **Analyze conversation content**：Identify key tasks, key points, and key decisions in the conversation
2. **Extract key information**：Extract important tasks, technical points, and decisions from the conversation
3. **Generate command**：Build a parameterized `samectx sync` command
4. **Prompt user**：Display analysis results and provide the execution command

## 核心功能

### 1. 对话分析

**识别关键任务**：
- 包含「需要」、「必须」、「应该」、「要」等关键词的句子
- 明确的待办事项和开发任务
- 项目计划和里程碑

**识别关键点**：
- 技术要点和重要发现
- 注意事项和风险提示
- 解决方案和实现细节

**识别关键决策**：
- 技术选型和架构决策
- 方案选择和比较
- 重要约定和标准

## Core Features

### 1. Conversation Analysis

**Identifying key tasks**：
- Sentences containing keywords like "need", "must", "should", "have to", etc.
- Clear to-do items and development tasks
- Project plans and milestones

**Identifying key points**：
- Technical points and important discoveries
- Notes and risk warnings
- Solutions and implementation details

**Identifying key decisions**：
- Technical selection and architecture decisions
- Solution selection and comparison
- Important agreements and standards

### 2. 命令生成

**基本命令**：
```bash
samectx sync
```

**带参数命令**：
```bash
samectx sync --tasks "任务1;任务2" --keypoints "关键点1;关键点2" --decisions "决策1;决策2"
```

### 2. Command Generation

**Basic command**：
```bash
samectx sync
```

**Command with parameters**：
```bash
samectx sync --tasks "task1;task2" --keypoints "keypoint1;keypoint2" --decisions "decision1;decision2"
```

### 3. 用户提示

**提示模板**：

```markdown
# 同步上下文

## 当前对话摘要

{对话摘要}

## 提取的关键信息

### 关键任务
1. {任务1}
2. {任务2}

### 关键点
- {关键点1}
- {关键点2}

### 关键决策
- {决策1}

## 执行命令

请复制并执行以下命令保存上下文：

```bash
samectx sync --tasks "{任务1};{任务2}" --keypoints "{关键点1};{关键点2}" --decisions "{决策1}"
```
```

### 3. User Prompt

**Prompt template**：

```markdown
# Sync Context

## Current Conversation Summary

{conversation summary}

## Extracted Key Information

### Key Tasks
1. {task1}
2. {task2}

### Key Points
- {keypoint1}
- {keypoint2}

### Key Decisions
- {decision1}

## Execute Command

Please copy and execute the following command to save the context：

```bash
samectx sync --tasks "{task1};{task2}" --keypoints "{keypoint1};{keypoint2}" --decisions "{decision1}"
```
```

## 使用示例

**示例 1：基本同步**

用户：「我需要保存这次对话的内容」

技能：
```markdown
# 同步上下文

## 当前对话摘要

用户请求保存对话内容。

## 执行命令

请复制并执行以下命令保存上下文：

```bash
samectx sync
```
```

**示例 2：带参数同步**

用户：「我们需要实现用户登录功能，使用 JWT 进行认证，选择 bcrypt 加密密码」

技能：
```markdown
# 同步上下文

## 当前对话摘要

用户讨论了登录功能的实现，包括认证方式和密码加密方案。

## 提取的关键信息

### 关键任务
1. 实现用户登录功能

### 关键点
- 使用 JWT 进行认证

### 关键决策
- 选择 bcrypt 加密密码

## 执行命令

请复制并执行以下命令保存上下文：

```bash
samectx sync --tasks "实现用户登录功能" --keypoints "使用JWT进行认证" --decisions "选择bcrypt加密密码"
```
```

## Usage Examples

**Example 1: Basic Sync**

User: "I need to save the content of this conversation"

Skill:
```markdown
# Sync Context

## Current Conversation Summary

User requests to save conversation content.

## Execute Command

Please copy and execute the following command to save the context：

```bash
samectx sync
```
```

**Example 2: Sync with Parameters**

User: "We need to implement user login functionality, use JWT for authentication, and choose bcrypt for password encryption"

Skill:
```markdown
# Sync Context

## Current Conversation Summary

User discussed the implementation of login functionality, including authentication method and password encryption scheme.

## Extracted Key Information

### Key Tasks
1. Implement user login functionality

### Key Points
- Use JWT for authentication

### Key Decisions
- Choose bcrypt for password encryption

## Execute Command

Please copy and execute the following command to save the context：

```bash
samectx sync --tasks "Implement user login functionality" --keypoints "Use JWT for authentication" --decisions "Choose bcrypt for password encryption"
```
```

## 技术依赖

- **npm 包**：samectx (全局安装)
- **Node.js**：>= 14.0.0
- **Git**：用于跨设备同步

## Technical Dependencies

- **npm package**：samectx (globally installed)
- **Node.js**：>= 14.0.0
- **Git**：for cross-device synchronization

## 注意事项

- 笔记将保存在项目目录的 `samectx-notes/{username}/` 文件夹中
- 建议将笔记目录提交到 Git 仓库以支持跨设备同步
- 请勿在笔记中包含敏感信息（密码、密钥等）

## Notes

- Notes will be saved in the `samectx-notes/{username}/` folder in the project directory
- It is recommended to commit the notes directory to the Git repository to support cross-device synchronization
- Do not include sensitive information (passwords, keys, etc.) in notes

## 故障排除

- **用户名识别错误**：运行 `samectx config --username <name>` 设置用户名
- **笔记位置错误**：检查当前工作目录或使用 `-d` 参数指定目录
- **Git 冲突**：手动解决冲突，保留各自的笔记文件

## Troubleshooting

- **Username recognition error**：Run `samectx config --username <name>` to set the username
- **Note location error**：Check the current working directory or use the `-d` parameter to specify a directory
- **Git conflict**：Manually resolve conflicts and keep each user's notes files

## 版本信息

- **技能版本**：1.0.0
- **依赖版本**：samectx >= 1.0.0
- **最后更新**：2026-03-23

## Version Information

- **Skill version**：1.0.0
- **Dependency version**：samectx >= 1.0.0
- **Last updated**：2026-03-23

---

## <a name="english-version"></a>English Version

[中文](#samectx)

# samectx

Context management tool - Intelligent conversation analysis and samectx command generation

## Introduction

This is a TRAE/Cursor SKILL that intelligently analyzes conversation content, extracts key information, and generates `samectx` npm tool commands to help users save conversation context to local project directories, supporting multi-user collaboration and cross-device synchronization.

## Core Features

- 🤖 **Intelligent Analysis**：Automatically identifies key tasks, key points, and key decisions in conversations
- 📝 **Command Generation**：Generates parameterized `samectx sync` commands based on analysis results
- 🌍 **Bilingual Support**：Supports both Chinese and English conversations
- 🚀 **User-Friendly**：Provides clear prompts and command examples
- 🔄 **Cross-Device Sync**：Supports cross-device synchronization via Git

## Relationship with npm-samectx

- **npm-samectx**：Core functionality implementation, providing actual context saving and management features
- **skill-samectx**：Intelligent assistant that analyzes conversations and generates corresponding npm commands

## Working Principle

1. **Trigger**：The skill automatically activates when conversations contain relevant keywords or content
2. **Analysis**：Analyzes conversation content to extract key tasks, key points, and key decisions
3. **Generation**：Generates parameterized `samectx sync` commands
4. **Prompt**：Displays analysis results and execution commands to the user
5. **Execution**：User copies and executes the command to save context locally

## Installation

### 1. Install Dependencies

```bash
# Install samectx npm package globally
npm install -g samectx
```

### 2. Install the Skill

Add this SKILL directory to TRAE or Cursor's skills directory：

```bash
# Clone the repository
git clone https://github.com/ethanhuangcst/skill-samectx.git

# Add to TRAE skills directory
cp -r skill-samectx ~/.trae/skills/

# Or add to Cursor skills directory
cp -r skill-samectx ~/.cursor/skills/
```

## Usage

### Trigger Scenarios

The SKILL automatically triggers in the following scenarios：

- User mentions keywords like "save context", "sync notes", "record conversation", etc.
- Conversation contains clear tasks, decisions, or technical discussions
- User directly requests to save the current conversation

### Examples

**Scenario 1: Basic Sync**

User: "I need to save the content of this conversation"

The skill will generate：
```bash
samectx sync
```

**Scenario 2: Sync with Parameters**

User: "We need to implement user login functionality, use JWT for authentication, and choose bcrypt for password encryption"

The skill will generate：
```bash
samectx sync --tasks "Implement user login functionality" --keypoints "Use JWT for authentication" --decisions "Choose bcrypt for password encryption"
```

## Skill File Structure

- **SKILL.md**：Main skill file containing complete skill logic and functionality
- **docs/**：Contains design and requirement documents

## Technical Dependencies

- **npm package**：`samectx` (globally installed)
- **Node.js**：>= 14.0.0
- **Git**：for cross-device synchronization

## Notes

- Notes will be saved in the `samectx-notes/{username}/` folder in the project directory
- It is recommended to commit the notes directory to the Git repository to support cross-device synchronization
- Do not include sensitive information (passwords, keys, etc.) in notes

## Troubleshooting

- **Username recognition error**：Run `samectx config --username <name>` to set the username
- **Note location error**：Check the current working directory or use the `-d` parameter to specify a directory
- **Git conflict**：Manually resolve conflicts and keep each user's notes files

## Related Links

- **npm package**：https://github.com/ethanhuangcst/npm-samectx
- **npm publication**：https://www.npmjs.com/package/samectx

## License

MIT

## Author

ethanhuangcst

## Version

- **Skill version**：1.0.0
- **Last updated**：2026-03-23