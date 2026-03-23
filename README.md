# samectx-skill

TRAE CN 上下文同步技能 - 智能提示用户使用 samectx npm 工具同步上下文

## 简介

这是一个 TRAE SKILL，用于智能提示用户使用 `samectx` npm 工具同步上下文到 GitHub Gist。

## 功能

- 🔄 **智能提示**：根据用户对话内容，智能提示需要同步上下文
- 📦 **安装引导**：首次使用时引导安装 samectx npm 包
- ⚙️ **配置帮助**：帮助用户配置 GitHub Token
- 🚀 **使用指导**：提供详细的使用说明和命令示例

## 与 npm-samectx 的关系

- **npm-samectx**：核心功能实现，提供实际的同步功能
- **skill-samectx**：智能助手，帮助用户记住和使用 npm 工具

## 安装

### 方式1：通过 TRAE 加载

1. 将此 SKILL 目录添加到 TRAE 的 skills 目录
2. 重启 TRAE 或重新加载 SKILL

### 方式2：手动安装

```bash
# 克隆仓库
git clone https://github.com/ethanhuangcst/skill-samectx.git

# 添加到 TRAE skills 目录
cp -r skill-samectx ~/.trae/skills/
```

## 使用

SKILL 会在以下场景自动触发：

- 用户提到"同步上下文"、"多设备"、"跨设备"等关键词
- 用户提到需要在其他设备继续工作
- 用户提到项目切换或上下文备份

触发后，SKILL 会显示相应的提示和命令示例。

## SKILL 内容

### install.md
首次使用时的安装引导，包含：
- npm 安装命令
- Token 配置说明
- 基本使用命令

### sync.md
同步提示，包含：
- 同步命令和选项
- 同步内容说明
- 故障排除

### config.md
配置提示，包含：
- Token 配置方法
- 配置查看命令
- 安全提示

### init.md
初始化提示，包含：
- 初始化命令
- 目录结构说明
- 后续步骤

## 依赖

- **npm 包**：`samectx`
- **Node.js**：>= 14.0.0

## 相关链接

- **npm 包**：https://github.com/ethanhuangcst/npm-samectx
- **npm 发布**：https://www.npmjs.com/package/samectx

## 许可证

MIT

## 作者

ethanhuangcst
