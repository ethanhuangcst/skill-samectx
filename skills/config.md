# ⚙️ 配置 samectx

## 配置 GitHub Token

```bash
samectx config --token ghp_your_token_here
```

## 查看当前配置

```bash
samectx config --show
```

## 配置文件位置

配置文件存储在：`~/.samectx/config.json`

## 获取 GitHub Token

1. 访问 https://github.com/settings/tokens/new
2. Note: `samectx`
3. Expiration: 选择 `Custom` → 设置 1 年后
4. Select scopes: ✅ `gist`
5. 点击 Generate token，复制保存

## 安全提示

- Token 会保存在本地配置文件中
- 不会上传到 Git 仓库
- 请妥善保管，不要分享给他人

## 故障排除

如果配置失败：
1. 确保 Token 格式正确（以 `ghp_` 开头）
2. 确保 Token 权限包含 `gist`
3. 检查 Token 是否过期
