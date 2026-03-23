# 🚀 初始化项目

## 初始化命令

```bash
samectx init
```

## 初始化内容

初始化会创建：
- `samectx-notes/` 目录（如果不存在）
- `.env.example` 文件（如果不存在）

## 初始化后步骤

1. 获取 GitHub Token
   ```bash
   # 访问
   https://github.com/settings/tokens/new
   ```

2. 配置 Token
   ```bash
   samectx config --token ghp_your_token_here
   ```

3. 开始同步
   ```bash
   samectx sync
   ```

## 目录结构

初始化后的项目结构：

```
your-project/
├── samectx-notes/        # 本地笔记目录
│   └── context_*.json    # 同步的上下文文件
└── .env.example          # 环境变量示例
```

## 简写形式

```bash
samectx i
```
