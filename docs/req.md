# skill-samectx 需求文档

## 项目概述

**skill-samectx** 是一个全局技能，用于调用 npm-samectx 命令，在多用户、多项目的场景下，把用户在 TRAE、Cursor 等 AI 辅助编程工具中与 LLM 对话的上下文保存在本地文件中。

## 核心功能

1. **自动初始化**：技能加载时自动检查并初始化 samectx 笔记目录
2. **定时保存**：默认每 30 分钟自动保存对话上下文
3. **手动保存**：用户可以随时手动保存当前对话上下文
4. **上下文加载**：新打开工具时自动加载之前的对话上下文

## 用户故事与验收标准

### US-1: 技能加载与初始化

```gherkin
Scenario: 加载全局 SKILL samectx
  Given 用户已经加载全局 SKILL samectx
  When 用户启动 TRAE 或 Cursor 工具
  Then SKILL 加载成功
  And SKILL 提供安装 npm-samectx 的脚本
  And SKILL 提供配置文件设置指南

Scenario: 打开工程时自动检测并初始化 samectx 笔记
  Given 全局 SKILL samectx 已加载
  When 用户打开 TRAE 或 Cursor 的工程 "project-a"
  And 工程 "project-a" 中尚未初始化 samectx 笔记
  Then SKILL 调用 "npm-samectx init" 命令
  And 在项目目录中创建 "samectx-notes/{username}/" 文件夹
  And 显示初始化成功的消息

Scenario: 打开工程时检测到已初始化的笔记
  Given 全局 SKILL samectx 已加载
  When 用户打开 TRAE 或 Cursor 的工程 "project-a"
  And 工程 "project-a" 中已存在 "samectx-notes/{username}/" 文件夹
  Then SKILL 跳过初始化步骤
  And 显示已初始化的提示信息
```

### US-2: 定时保存对话上下文

```gherkin
Scenario: 定时自动保存对话上下文
  Given 用户 "ethan" 在项目 "project-a" 中与 LLM 进行对话
  And samectx 技能已加载，定时保存功能已启用
  And 配置文件中设置的定时保存间隔为 30 分钟（默认值）
  And 距离上次保存已超过配置的时间间隔
  When 定时保存触发
  Then LLM 总结对话中的关键信息（关键任务、关键点、关键决策、关键问题）
  And 技能调用 "npm-samectx sync" 命令，传入关键信息
  And 对话上下文被保存到 "samectx-notes/ethan/" 目录
  And 显示保存成功的消息

Scenario: 修改定时保存间隔
  Given 用户 "ethan" 想要修改定时保存间隔
  And 技能配置文件中当前定时保存间隔为 30 分钟
  When 用户在配置文件中设置定时保存间隔为 15 分钟
  Then 技能使用新的 15 分钟间隔进行定时保存
  And 显示配置更新成功的消息
```

### US-3: 手动保存对话上下文

```gherkin
Scenario: 用户手动保存对话上下文
  Given 用户 "ethan" 在项目 "project-a" 中与 LLM 进行对话
  And samectx 技能已加载
  When 用户调用 "samectx" 命令
  Then LLM 总结对话中的关键信息（关键任务、关键点、关键决策、关键问题）
  And 技能调用 "npm-samectx sync" 命令，传入关键信息，例如：
  And "samectx sync --tasks \"实现用户登录功能;修复登录页面样式\" --keypoints \"使用JWT进行认证\" --decisions \"选择bcrypt加密密码\""
  And 对话上下文被保存到 "samectx-notes/ethan/" 目录
  And 显示保存成功的消息

Scenario: 手动保存时无对话内容
  Given 用户 "ethan" 在项目 "project-a" 中未与 LLM 进行对话
  And samectx 技能已加载
  When 用户调用 "samectx" 命令
  Then 技能提示 "无对话内容可保存"
  And 不执行保存操作
```

### US-4: 加载之前的对话上下文

```gherkin
Scenario: 新打开工具时加载之前的对话上下文
  Given 用户 "ethan" 之前在项目 "project-a" 中已保存对话上下文
  And 项目 "project-a" 中存在 "samectx-notes/ethan/" 目录，包含之前的笔记文件
  When 用户新打开 TRAE 或 Cursor 工具
  And samectx 技能加载
  Then 技能读取 "samectx-notes/ethan/" 目录中的最新笔记文件
  And 提取笔记中的关键信息
  And 将关键信息作为上下文提供给 LLM
  And 显示上下文加载成功的消息

Scenario: 无历史上下文时的处理
  Given 用户 "ethan" 首次在项目 "project-a" 中打开工具
  And 项目 "project-a" 中不存在 "samectx-notes/ethan/" 目录
  When samectx 技能加载
  Then 技能执行初始化操作
  And 提示用户开始新的对话
```

### US-5: 多用户支持

```gherkin
Scenario: 不同用户在同一项目中使用技能
  Given 用户 "ethan" 已在项目 "project-a" 中初始化 samectx 笔记
  And 项目 "project-a" 中存在 "samectx-notes/ethan/" 目录
  When 用户 "alice" 在同一项目中加载 samectx 技能
  Then 技能为 "alice" 初始化 "samectx-notes/alice/" 目录
  And alice 的笔记与 ethan 的笔记相互隔离
  And 显示初始化成功的消息
```

### US-6: 多项目支持

```gherkin
Scenario: 同一用户在不同项目中使用技能
  Given 用户 "ethan" 已在项目 "project-a" 中初始化 samectx 笔记
  When 用户 "ethan" 在项目 "project-b" 中加载 samectx 技能
  Then 技能为 "ethan" 在 "project-b" 中初始化 "samectx-notes/ethan/" 目录
  And 不同项目的笔记相互隔离
  And 显示初始化成功的消息
```

### US-7: 自动总结新增上下文的重点

```gherkin
Scenario: 自动总结新增上下文的重点
  Given 用户 "ethan" 在项目 "project-a" 中与 LLM 进行对话
  And samectx 技能已加载
  And 上次保存上下文笔记的时间为 T1
  And 当前时间为 T2，且 T2 > T1
  When 用户继续与 LLM 对话，产生新的上下文内容
  Then 技能自动识别 T1 之后的新增上下文
  And LLM 总结新增上下文的重点
  And 技能在对话中向用户展示总结的重点
  And 总结内容包括新增的关键任务、关键点、关键决策、关键问题

Scenario: 总结上下文重点的逻辑
  Given 用户 "ethan" 在项目 "project-a" 中与 LLM 进行对话
  And samectx 技能已加载
  And 上次保存的笔记文件包含时间戳 T1
  When 技能需要总结新增上下文
  Then 技能获取 T1 之后的对话内容
  And LLM 分析新增内容，识别关键信息
  And 技能生成结构化的总结，包括：
  And - 新增的关键任务
  And - 新增的关键点
  And - 新增的关键决策
  And - 新增的关键问题
  And 技能在对话中展示总结结果
```

### US-8: 性能优化要求

```gherkin
Scenario: 节约 token 消耗
  Given 用户 "ethan" 在项目 "project-a" 中与 LLM 进行对话
  And samectx 技能已加载
  When 技能整理上下文、生成笔记、保存文件
  Then 技能不在对话中展示思考过程
  And 技能只展示关键信息和最后结果
  And 技能优化提示词，减少不必要的 token 消耗

Scenario: 操作在短时间内完成
  Given 用户 "ethan" 在项目 "project-a" 中与 LLM 进行对话
  And samectx 技能已加载
  When 技能执行整理上下文、生成笔记、保存文件操作
  Then 整个操作在 5 秒内完成
  And 用户可以正常继续对话，无明显卡顿
```

## 非功能性需求

### 性能要求

```gherkin
Scenario: 保存操作在合理时间内完成
  Given 用户与 LLM 的对话内容不超过 1MB
  When 执行保存操作
  Then 保存操作在 3 秒内完成
  And 不影响用户正常使用工具

Scenario: 加载操作在合理时间内完成
  Given 项目中存在多个 samectx 笔记文件
  When 技能加载并读取最新笔记
  Then 加载操作在 2 秒内完成
  And 工具正常启动
```

### 可靠性要求

```gherkin
Scenario: 网络或磁盘错误时的处理
  Given 用户执行保存操作
  And 网络连接中断或磁盘空间不足
  When 保存操作失败
  Then 技能显示友好的错误消息
  And 提示用户检查网络或磁盘状态
  And 不影响工具的正常使用

Scenario: npm-samectx 未安装时的处理
  Given samectx 技能加载
  And 系统中未安装 npm-samectx
  When 技能检测到 npm-samectx 未安装
  Then 技能提示用户安装 npm-samectx
  And 提供安装命令："npm install -g samectx"
  And 不影响工具的正常使用
```

### 安全性要求

```gherkin
Scenario: 敏感信息处理
  Given 用户对话中包含敏感信息（如密码、API 密钥）
  When 执行保存操作
  Then LLM 在总结时识别并排除敏感信息
  And 保存的笔记中不包含敏感信息
  And 提示用户注意保护敏感信息
```

### 兼容性要求

```gherkin
Scenario: 跨平台支持
  Given 用户在 Windows、macOS 或 Linux 系统上
  When 安装并使用 samectx 技能
  Then 所有功能正常工作
  And 笔记目录结构一致

Scenario: 跨工具支持
  Given 用户在 TRAE、Cursor 或其他 AI 辅助编程工具中
  When 加载 samectx 技能
  Then 技能正常运行
  And 功能表现一致
```

## 技术约束

### 依赖约束

- **npm-samectx**：必须安装 1.1.0 或更高版本
- **Node.js**：>= 14.0.0
- **npm**：>= 6.0.0

### 存储约束

- 本地笔记存储在项目目录下的 `samectx-notes/{username}/` 目录
- 全局配置存储在 `~/.samectx/` 目录
- 笔记文件格式：JSON (.json)

### 配置约束

- 定时保存间隔：默认 30 分钟，可在配置文件中修改
- 配置文件格式：JSON
- 配置文件路径：`~/.samectx/config.json`

## 术语表

| 术语 | 定义 |
|------|------|
| samectx | 上下文管理工具，包括 skill-samectx（技能）和 npm-samectx（命令行工具） |
| 上下文 | AI 工具中的对话历史，包括任务、关键点、决策等信息 |
| 笔记 | 保存后存储的结构化上下文信息 |
| LLM | 大语言模型，如 GPT、Claude 等 |
| TRAE | AI 辅助编程工具 |
| Cursor | AI 辅助编程工具 |

## 变更历史

| 版本 | 日期 | 变更内容 | 作者 |
|------|------|---------|------|
| 1.1.0 | 2026-03-23 | 增加自动总结新增上下文的重点功能，增加性能优化要求，删除笔记文件安全存储场景 | ethanhuangcst |
| 1.0.0 | 2026-03-23 | 初始版本，基于 ATDD 方法编写 | ethanhuangcst |