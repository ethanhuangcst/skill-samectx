# 🔄 上下文同步工具安装

欢迎使用 samectx 上下文同步工具！

## 首次使用，请先安装

```bash
npm install -g samectx
```

## 安装完成后，配置 GitHub Token

```bash
samectx config --token ghp_your_token_here
```

## 然后可以开始同步

```bash
samectx sync
```

## 其他常用命令

- 查看笔记列表：`samectx list`
- 查看配置：`samectx config --show`
- 初始化项目：`samectx init`
- 查看帮助：`samectx --help`

## 获取 GitHub Token

1. 访问 https://github.com/settings/tokens/new
2. Note: `samectx`
3. Expiration: 选择 `Custom` → 设置 1 年后
4. Select scopes: ✅ `gist`
5. 点击 Generate token，复制保存

## 更多信息

详细文档：https://github.com/ethanhuangcst/npm-samectx
