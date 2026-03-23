# 项目重构计划：解耦 npm-samectx 和 skill-samectx

## 目标

- Git 仓库名：`npm-samectx`（保持不变）
- npm 包名：`samectx`（改回原名）
- 命令名：`samectx`（改回原名）
- 创建独立的 `skill-samectx` 仓库

## 阶段一：npm 应用名称回退

### 1.1 修改 package.json
- [ ] 将 `name` 字段从 `npm-samectx` 改为 `samectx`
- [ ] 将 `bin` 字段中的命令名从 `npm-samectx` 改为 `samectx`

### 1.2 修改 bin/samectx
- [ ] 将 `.name('npm-samectx')` 改为 `.name('samectx')`

### 1.3 修改 src/index.js
- [ ] 将文件头注释中的 `npm-samectx` 改为 `samectx`
- [ ] 将 metadata 中的 tool 字段从 `npm-samectx` 改为 `samectx`
- [ ] 将错误消息中的命令引用从 `npm-samectx` 改为 `samectx`

### 1.4 修改 src/cli.js
- [ ] 将初始化提示中的命令从 `npm-samectx` 改为 `samectx`

### 1.5 修改 README.md
- [ ] 将标题从 `npm-samectx` 改为 `samectx`
- [ ] 将所有安装命令从 `npm install -g npm-samectx` 改为 `npm install -g samectx`
- [ ] 将所有 npx 命令从 `npx npm-samectx` 改为 `npx samectx`
- [ ] 将所有命令示例从 `npm-samectx sync/list/config/init` 改为 `samectx sync/list/config/init`
- [ ] 将 GitHub Token 说明中的引用从 `npm-samectx` 改为 `samectx`

### 1.6 验证修改
- [ ] 运行 `npm install` 确保依赖正常
- [ ] 运行 `samectx --help` 验证命令可用
- [ ] 检查所有文件中的命令引用是否一致

## 阶段二：整理项目文件

### 2.1 识别文件归属

#### npm-samectx 仓库文件（保留）
- [ ] `bin/` - CLI 入口
- [ ] `src/` - 核心代码
- [ ] `package.json` - npm 配置
- [ ] `package-lock.json` - 依赖锁定
- [ ] `README.md` - npm 包文档
- [ ] `.env.example` - 环境变量示例
- [ ] `.gitignore` - Git 忽略配置

#### skill-samectx 仓库文件（待创建）
- [ ] `skills/` - SKILL 内容目录
  - [ ] `install.md` - 安装引导
  - [ ] `sync.md` - 同步提示
  - [ ] `config.md` - 配置提示
  - [ ] `init.md` - 初始化提示
- [ ] `skill.json` - SKILL 配置文件
- [ ] `README.md` - SKILL 说明文档
- [ ] `.gitignore` - Git 忽略配置

#### 测试/临时文件（可选保留）
- [ ] `trae-content-gist-notes/` - 测试数据（可加入 .gitignore）
- [ ] `test-plan.md` - 测试计划（可删除或归档）
- [ ] `index.js` - 根目录入口（检查是否需要）

### 2.2 清理当前项目
- [ ] 检查 `index.js` 是否被使用，如未使用则删除
- [ ] 将 `trae-content-gist-notes/` 加入 `.gitignore`（如未加入）
- [ ] 删除或归档 `test-plan.md`

## 阶段三：创建 skill-samectx 仓库

### 3.1 创建 SKILL 目录结构
- [ ] 创建 `skills/` 目录
- [ ] 创建 `skills/install.md`
- [ ] 创建 `skills/sync.md`
- [ ] 创建 `skills/config.md`
- [ ] 创建 `skills/init.md`

### 3.2 创建 SKILL 配置文件
- [ ] 创建 `skill.json`，定义 SKILL 元数据
- [ ] 定义触发条件（description）
- [ ] 定义 SKILL 名称和版本

### 3.3 创建 SKILL 文档
- [ ] 创建 `README.md`，说明 SKILL 用途
- [ ] 说明与 npm-samectx 的关系
- [ ] 提供使用示例

### 3.4 创建 Git 配置
- [ ] 创建 `.gitignore`
- [ ] 初始化 Git 仓库
- [ ] 创建 GitHub 远程仓库

## 阶段四：测试与验证

### 4.1 测试 npm 包
- [ ] 本地安装：`npm install -g .`
- [ ] 测试命令：`samectx --help`
- [ ] 测试同步：`samectx sync`
- [ ] 测试列表：`samectx list`
- [ ] 测试配置：`samectx config --show`

### 4.2 测试 SKILL
- [ ] 在 TRAE 中加载 SKILL
- [ ] 验证 SKILL 触发条件
- [ ] 验证 .md 内容显示正确
- [ ] 验证命令提示准确

### 4.3 测试双方案协作
- [ ] 触发 SKILL → 看到 npm 安装提示
- [ ] 执行 npm 安装命令
- [ ] 触发 SKILL → 看到 npm 使用提示
- [ ] 执行 npm 同步命令

## 阶段五：发布与部署

### 5.1 发布 npm 包
- [ ] 更新 package.json 版本号
- [ ] 运行 `npm publish`
- [ ] 验证 npm 包可安装：`npm install -g samectx`

### 5.2 发布 SKILL
- [ ] 提交代码到 GitHub
- [ ] 创建 GitHub Release
- [ ] 更新 SKILL 注册表（如需要）

### 5.3 更新文档
- [ ] 更新 npm-samectx README，说明独立使用方式
- [ ] 更新 skill-samectx README，说明与 npm 包的关系
- [ ] 创建使用指南文档

## 阶段六：清理与归档

### 6.1 清理临时文件
- [ ] 删除测试数据（如不需要）
- [ ] 删除临时文件
- [ ] 清理 Git 历史（如需要）

### 6.2 归档旧版本
- [ ] 创建 Git tag 标记重构前版本
- [ ] 归档旧版本代码（如需要）

## 执行顺序

1. **先执行阶段一**：修改 npm 包名，确保功能正常
2. **再执行阶段二**：整理文件，明确归属
3. **然后执行阶段三**：创建 SKILL 仓库
4. **接着执行阶段四**：全面测试
5. **最后执行阶段五和六**：发布和清理

## 注意事项

1. **版本兼容性**：确保 npm 包向后兼容
2. **文档一致性**：两个仓库的文档要保持一致
3. **测试充分**：在发布前充分测试双方案协作
4. **Git 历史**：考虑是否需要保留完整的 Git 历史
5. **用户通知**：如有现有用户，需要通知变更

## 预计时间

- 阶段一：30 分钟
- 阶段二：15 分钟
- 阶段三：45 分钟
- 阶段四：30 分钟
- 阶段五：15 分钟
- 阶段六：15 分钟

**总计：约 2.5 小时**
