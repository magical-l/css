# CSS 布局实验室 项目概要

纯 CSS Utility 库与布局实验室——提供通用 CSS 类（Flex/Grid 布局、形状、图标、按钮、表单、动画），附带演示与测试页。

- **技术栈**：纯 CSS，零框架，无构建工具依赖
- **版本号**：`css/common.css` 文件头 `@charset "utf-8";/* vX.Y.Z */`
- **不可修改**：无第三方依赖

## 知识索引

从入口文档开始，遇到不理解的引用时点链接深入。

| 想了解 | 去读 |
|--------|------|
| 项目结构、文件职责、阅读路线 | [`docs/index.md`](docs/index.md) |
| 技术选型、变量体系、类名命名约定 | [`docs/design/css-architecture.md`](docs/design/css-architecture.md) |
| common.css（reset/Tokens/图标/按钮/弹出层） | [`docs/modules/common.md`](docs/modules/common.md) |
| layout.css（Flex/Grid utility/书写模式/预设布局） | [`docs/modules/layout.md`](docs/modules/layout.md) |
| 花活.css（装饰图案/动画特效） | [`docs/modules/hua-huo.md`](docs/modules/hua-huo.md) |
| 测试体系（Demo/Test 目录） | [`docs/design/test-system.md`](docs/design/test-system.md) |

## 文档层（自愈文档）

`docs/` 目录包含从源代码反向推导的自愈文档层。改代码前后的读/写规则：

### 改代码前
- 在 `docs/index.md` 找到受影响模块对应的文档
- 读对应文档的「设计意图」和「函数索引」，理解模块边界和设计约束
- dispatch 子代理做代码调研时，在 prompt 中注明「先去 docs/modules/xxx.md 读设计意图，再 grep 定位代码」

### 改代码后（文档同步）
每次改完代码后，必须更新本次改动涉及的所有文档：
1. **定位**：通过 `covers_file` 元数据找到被改文件对应的文档
2. **更新功能描述**：如果改动改变了类的行为或新增了类
3. **追加决策日志**：记录变更原因，按 `YYYY-MM-DD: 原因` 格式
4. **更新 `last_updated`** 字段

**没有格式验证脚本**——当前只需确保 YAML frontmatter 格式正确即可。

### 不追踪行号
函数/类索引不写行号，由 AI 实时 grep 定位。

### 自愈纠错
读文档时，如果发现内容与代码不符（类描述过时、模块结构变化、`covers_file` 不完整等），**立即修正**。
