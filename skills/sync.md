# 🔄 同步上下文

检测到您可能需要同步上下文到其他设备。

## 执行同步命令

```bash
samectx sync
```

## 同步选项

```bash
# 指定项目名称
samectx sync --project my-project

# 指定笔记存储目录
samectx sync --dir ./notes

# 简写形式
samectx s
```

## 同步内容

同步会将当前项目的对话上下文：
- 保存到本地 `samectx-notes/` 目录
- 上传到 GitHub Gist（如果配置了 Token）

## 查看同步结果

```bash
samectx list
```

## 故障排除

如果同步失败：
1. 检查 Token 是否配置：`samectx config --show`
2. 检查 Token 权限是否包含 `gist`
3. 本地笔记仍可使用，不受影响
