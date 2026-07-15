---
title: 测试体系
covers_file: [test/, demo/, test-btn-light.html]
depends_on: [css-architecture]
api_signature: 无（测试页面，纯 HTML 手工构造）
last_updated: 2026-07-15
why_exists: CSS 库没有单元测试，测试靠手动创建的 HTML 页面验证布局行为，需要文档说明测试覆盖情况
---

## 设计意图

CSS 布局实验室的测试体系没有自动化测试框架，依靠**手工构造的测试页**验证浏览器渲染结果。测试页分两类：

- **Demo**（`demo/`）— 直观展示某个功能的视觉效果，用户可打开看"好不好看"
- **Test**（`test/`）— 系统验证某组 Utility 类在不同组合下的表现，开发者可打开看"对不对"

此外 `test-btn-light.html` 在根目录，是按钮颜色主题的视觉对比页面。

## Demo 目录

| 文件 | 测试内容 | 说明 |
|------|----------|------|
| `太极.html` | `.taiji` 类 | 纯 CSS 太极图绘制效果 |
| `grid-writing-mode-demo.html` | `.grid` + writing-mode | Grid 原生属性与书写模式的关系 |
| `native-property-demo.html` | justify/align-content/items | 验证不同属性的实际行为 |
| `stretch.html` | stretch 行为 | Flex/Grid 中 stretch 的规律探索 |
| `layout-demo.html` | 布局预设 | `.工字形` `.亚字形` 等 Grid 预设布局演示 |
| `hourglass.html` | `.icon.hourglass` | 沙漏图标动画 |

## Test 目录

| 文件 | 测试内容 |
|------|----------|
| `basics.html` | 基础类（`.hidden` `.invisible` `.shape.*` `.icon.*` `.btn.*`） |
| `layout.html` | 完整的 layout.css utility 类测试 |
| `animation.html` | 动画效果（`.spin` `.pulse` `.shadow-breathing`） |
| `modal.html` | 模态框布局 |
| `shape.html` | 特殊图形（`.shape.triangle` `.taiji`） |
| `writing-mode.html` | 书写模式类（`.content-Z/S/N/И`） |

### Test 页面命名约定

test 目录中有些页面使用双连字符命名表示测试的特定模块组合：

| 文件 | 测试组合 |
|------|----------|
| `flex--center.html` | Flex 居中与弹性填充相关类 |
| `content--mode.html` | 书写模式类 |
| `flex--monopoly.html` | Flex 独占类（`.monopoly-x/y`） |
| `go--from.html` | 方向组合（`.items-go-x` + `.items-from-*`） |
| `items--near.html` | 轨道位置类（`.items-near-left/right/top/bottom`） |
| `items--space-around.html` | space-around 分布类 |
| `items--space-between.html` | space-between 分布类 |
| `items--space-evenly.html` | space-evenly 分布类 |
| `items--stretch.html` | stretch 行为 |
| `row-col-near.html` | row-near-* / col-near-*（Grid + Flex） |

## 如何新增测试

1. 在 `test/` 下创建 HTML 文件，引用 `../css/common.css` 和 `../css/layout.css`
2. 页面标题与文件功能对应
3. 将入口链接添加到 `index.html` 的 Test 列表中
4. 如果测试的是某个特定模块的复杂行为，在对应模块文档的决策日志中添加条目

## 测试原则

- 一个页面测试一组相关的类，不要混入无关测试
- 使用文字标签标注每个被测元素的预期行为
- 视觉对比：对于颜色/尺寸等视觉变量，可创建并排对比页面（如 `test-btn-light.html`）
- 验证在主流浏览器（Chrome/Firefox/Safari/Edge）上的表现一致

## 函数索引

| 文件 | 路径 | 测试范围 |
|------|------|----------|
| 基础功能测试 | test/basics.html | common.css 全部基础类 |
| 布局测试 | test/layout.html | layout.css 全部 utility 类 |
| Demo 演示 | demo/ 目录 | 视觉展示 |

## 决策日志

- 2026-07-15: 初始文档创建
