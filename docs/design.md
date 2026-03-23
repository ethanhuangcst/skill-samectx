# skill-samectx 技术设计文档

## 1. 核心架构

### 1.1 SKILL + npm 协作架构

```
┌─────────────────┐
│   TRAE SKILL    │  智能提示器
│  (skill-samectx)│  - 检测用户需求
│                 │  - 提供命令提示
└────────┬────────┘
         │ 提示用户
         ↓
┌─────────────────┐
│   npm Package   │  功能执行器
│  (npm-samectx)  │  - 执行保存命令
│                 │  - 管理本地文件
│                 │  - 支持多用户协作
└─────────────────┘
```

### 1.2 工作流程

1. **SKILL 分析对话**：提取关键任务、关键点、关键决策
2. **生成命令**：构建带参数的 `samectx sync` 命令
3. **用户执行**：用户复制并执行命令
4. **npm 保存**：npm-samectx 保存结构化笔记到本地

## 2. 核心命令设计

### 2.1 初始化命令

**功能**：初始化项目配置，创建用户专属笔记目录

**命令**：
```bash
samectx init
samectx i  # 别名
```

**执行效果**：
- 自动识别用户名
- 创建用户专属笔记目录：`samectx-notes/{username}/`
- 配置默认存储路径

### 2.2 同步上下文命令

**功能**：保存对话上下文到本地目录

**命令**：
```bash
samectx sync [options]
samectx s [options]  # 别名
```

**参数**：

| 参数 | 简写 | 类型 | 说明 |
|------|------|------|------|
| `--project <name>` | `-p` | string | 指定项目名称（默认从当前目录提取） |
| `--dir <path>` | `-d` | string | 指定笔记存储目录 |
| `--tasks <list>` | `-t` | string | 关键任务列表（分号 `;` 分隔） |
| `--keypoints <list>` | `-k` | string | 关键点列表（分号 `;` 分隔） |
| `--decisions <list>` | `-D` | string | 关键决策列表（分号 `;` 分隔） |
| `--content <text>` | `-c` | string | 完整对话内容（自动分析提取） |

**使用场景**：

1. **根据用户名生成笔记**：
   ```bash
   # 自动使用系统用户名或配置的用户名
   samectx sync
   
   # 临时指定用户名
   SAMECTX_USER=alice samectx sync
   ```

2. **总结上下文重点后生成笔记**：
   ```bash
   # 提取关键信息后生成命令
   samectx sync --tasks "实现用户登录;修复页面样式" --keypoints "使用JWT认证" --decisions "选择bcrypt加密"
   ```

3. **传入完整对话内容**：
   ```bash
   # 自动分析对话内容
   samectx sync --content "用户要求实现登录功能，我们讨论了JWT和Session两种方案..."
   ```

### 2.3 列出笔记命令

**功能**：列出所有保存的笔记

**命令**：
```bash
samectx list [options]
samectx l [options]  # 别名
```

**参数**：
| 参数 | 简写 | 类型 | 说明 |
|------|------|------|------|
| `--project <name>` | `-p` | string | 筛选指定项目的笔记 |

### 2.4 配置管理命令

**功能**：管理全局配置

**命令**：
```bash
samectx config [options]
samectx c [options]  # 别名
```

**参数**：
| 参数 | 简写 | 类型 | 说明 |
|------|------|------|------|
| `--username <name>` | `-u` | string | 设置用户名 |
| `--dir <path>` | `-d` | string | 设置默认笔记目录 |
| `--show` | `-s` | - | 显示当前配置 |

## 3. 技术实现细节

### 3.1 目录结构

**用户本地目录**：
```
project-a/
└── samectx-notes/           # 笔记目录（提交到 Git）
    ├── ethan/               # 用户 ethan 的笔记
    │   ├── context_2026-03-23T10-30.md
    │   └── ...
    ├── alice/               # 用户 alice 的笔记
    │   └── ...
    └── bob/                 # 用户 bob 的笔记
        └── ...
```

### 3.2 用户名识别优先级

1. **配置文件用户名**：`~/.samectx/config.json` 中的 `username` 字段
2. **Git 配置用户名**：`git config user.name`
3. **环境变量**：`SAMECTX_USER`
4. **系统用户名**：`os.userInfo().username`

### 3.3 数据结构

**笔记文件结构**：
```json
{
  "projectName": "...",
  "username": "...",
  "timestamp": "...",
  "summary": "...",
  "tasks": ["任务1", "任务2"],
  "keyPoints": ["关键点1"],
  "decisions": ["决策1"],
  "metadata": {
    "version": "1.0.0",
    "tool": "samectx"
  }
}
```

## 4. SKILL 集成方案

### 4.1 智能分析逻辑

SKILL 应自动识别对话中的：
- **关键任务**：待办事项、开发任务、计划
- **关键点**：重要发现、注意事项、技术要点
- **关键决策**：技术选型、架构决策、方案选择

### 4.2 命令生成模板

```markdown
# 同步上下文

## 当前对话摘要

{简要描述对话内容}

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

请复制并执行以下命令：

```bash
samectx sync --tasks "{任务1};{任务2}" --keypoints "{关键点1};{关键点2}" --decisions "{决策1}"
```
```

### 4.3 最佳实践

1. **使用分号分隔多个项目**：`--tasks "任务1;任务2"`
2. **保持命令简洁**：只包含核心信息
3. **明确提示用户执行**：清晰的命令执行说明

## 5. 跨设备同步

### 5.1 Git 同步（推荐）

```bash
# 设备 A：保存并提交
samectx sync
git add samectx-notes/
git commit -m "Update context notes"
git push

# 设备 B：拉取并继续工作
git pull
samectx list
```

### 5.2 手动同步

```bash
# 使用 -d 参数指定同步目录
samectx sync --dir /path/to/sync/folder
```

## 6. 技术栈

- **SKILL**：静态 .md 文件
- **npm 包**：Node.js >= 14.0.0
- **CLI 框架**：Commander.js
- **终端美化**：chalk
- **存储**：本地文件系统
- **版本控制**：Git

## 7. 安全与隐私

- **笔记可见性**：所有协作者可查看
- **历史记录**：保留在 Git 历史中
- **敏感信息**：请勿包含密码、密钥等
- **个人内容**：个人临时想法建议使用其他工具

## 8. 故障排除

| 问题 | 解决方案 |
|------|---------|
| 用户名识别错误 | `samectx config --username <name>` |
| 笔记位置错误 | 检查当前工作目录或使用 `-d` 参数 |
| Git 冲突 | 手动解决冲突，保留各自的笔记文件 |
| 权限问题 | 检查目录权限或使用 `-d` 参数指定其他目录 |

---

**文档版本**: 1.0.0  
**最后更新**: 2024-01-01  
**作者**: ethanhuangcst