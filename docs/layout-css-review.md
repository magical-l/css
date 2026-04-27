# Layout CSS 语义与逻辑审查

> 审查目标：验证命名语义是否准确、逻辑是否合理、风格是否自洽

---

## 问题列表

### 问题1：方向相关类名的语义不准确

**涉及类名**：`.stack`、`.vertical`、`.items-near-x-start` / `.items-near-x-end` / `.items-near-x-center`、`.items-near-y-start` 等

**问题描述**：
- `flex-direction: column-reverse` 的实际方向依赖 `writing-mode` 和 `direction`，并非固定"从下往上"
- `flex-direction: column` 同样依赖书写模式，在 RTL 或竖排文字中不一定是"垂直"
- 用 `x` 表示主轴、`y` 表示次轴，但 flex 的主轴方向取决于 `flex-direction`。当 `flex-direction: column` 时，主轴是 Y 方向，命名与实际相反

**讨论结论**：

采用"物理方向类 + 书写模式适配"方案。

**书写模式类**（设置 writing-mode + direction）：

| 类名 | CSS 组合 | 使用场景 |
|------|----------|----------|
| `.content-Z` | `horizontal-tb` + `ltr` | 现代横排（中文、英文） |
| `.content-S` | `horizontal-tb` + `rtl` | 阿拉伯语、希伯来语 |
| `.content-N` | `vertical-rl` + `ltr` | 传统中文竖排（列从右往左） |
| `.content-И` | `vertical-lr` + `ltr` | 蒙古文竖排（列从左往右） |

**方向类**（物理语义固定，CSS 值随书写模式自动适配）：

| 类名 | 物理语义 | `.content-Z` | `.content-S` | `.content-N` | `.content-И` |
|------|----------|--------------|--------------|--------------|--------------|
| `.items-from-top` | 项目从顶部开始排列 | `column` | `column` | `row` | `row` |
| `.items-from-bottom` | 项目从底部开始排列 | `column-reverse` | `column-reverse` | `row-reverse` | `row-reverse` |
| `.items-from-left` | 项目从左侧开始排列 | `row` | `row-reverse` | `column-reverse` | `column` |
| `.items-from-right` | 项目从右侧开始排列 | `row-reverse` | `row` | `column` | `column-reverse` |

命名逻辑：
- `from-方向` 表达排列起点，语义清晰，不暗示空间分配逻辑
- 书写模式和方向类组合在同一元素上（`.content-N.items-from-top`），使用组合选择器而非后代选择器

**已删除的旧类名**：`.vertical`、`.stack`、`.y-queue`、`.y-stack`、`.x-queue`、`.x-stack`

**忽略的组合**：
- `vertical-rl + rtl` 和 `vertical-lr + rtl` 这两种组合没有自然语言使用，忽略不影响实际场景。

---

### 问题2：`.stretch` 语义混淆

**涉及类名**：`.stretch`（flex部分，第67行）

**问题描述**：
- 当前定义：`flex: 1 1 auto`
- 但 `stretch` 在 CSS 中通常指 `align-items: stretch`（拉伸填充交叉轴），语义混淆
- 这个类的实际行为是"允许弹性伸缩"，而非"拉伸填充"

**讨论结论**：

删除 `.stretch`。功能合并到精简后的 `.flexible`：

```css
.flexible {
    flex: 1 1 auto;
    align-self: stretch;
    min-width: 0;
    min-height: 0;
}
```

命名 `flexible` 表示"弹性填充"，包含 flex 伸缩和交叉轴拉伸。`min-0` 是确保 flex 行为正常的技术细节。删除大模型添加的 `box-sizing`、`will-change`、`transition`。

---

### 问题3：`.monopoly` 是否需要汉字别名

**涉及类名**：`.monopoly`

**问题描述**：
- 当前定义：`flex-basis: 100%`
- 只保证"占满主轴"，不保证"独占"（nowrap 模式下其他项目仍被压缩）
- 需要 wrap 模式才真正实现"独占一行"

**讨论结论**：

删除原有 `.monopoly`，改为条件依赖的复合选择器：

```css
/* X轴独占一行：row 方向 + wrap */
.multi-line:not(.items-from-top):not(.items-from-bottom) .monopoly-x,
.multi-line.items-from-left .monopoly-x,
.multi-line.items-from-right .monopoly-x {
    flex-basis: 100%;
}

/* Y轴独占一列：column 方向 + wrap */
.multi-line.items-from-top .monopoly-y,
.multi-line.items-from-bottom .monopoly-y {
    flex-basis: 100%;
}
```

命名 `-x/-y` 对应物理轴方向。方向类 `.items-from-left/.items-from-top` 等来自问题1的设计。

---

### 问题4：`.lines-mutex` / `.lines-social-phobia` 等命名风格

**涉及类名**：`.lines-mutex`、`.lines-social-phobia`、`.lines-unfamiliar`、`.lines-friendly` 以及 `.items-*` 对应版本

**问题描述**：
- 这些命名是英文，但语义来自中文概念（互斥、社恐、不熟、友好）
- 是"形象化表达"，而非标准 CSS 术语
- 是否需要汉字主命名？或当前英文命名已足够表达？

**讨论结论**：保持现状。

这些命名都是英文原生词汇（mutex=计算机术语，social-phobia=心理学术语），形象化表达与 CSS 原生术语别名并存，用户可选择偏好风格。

---

### 问题5：`.flexible` 职责过重

**涉及类名**：`.flexible`（第204-212行）

**问题描述**：
- 包含 flex、align-self、box-sizing、min-width/height、will-change、transition 共6个属性
- 还带动画效果
- 名字 `flexible` 无法表达这么多行为
- 这更像组件样式而非原子工具类

**讨论结论**：已在问题2中一并处理。精简为4个属性，删除 `box-sizing`、`will-change`、`transition`。

---

### 问题6：`.near-center` 语义模糊且混用属性

**涉及类名**：`.near-center`（第202行）

**问题描述**：
- 同时设置 justify-content、align-items、text-align 三个属性
- 语义"靠近中心"过于模糊
- 混用了布局属性和文本属性

**讨论结论**：

拆成两个类，语义更清晰：

```css
/* 容器属性：让子项目居中 */
.item-near-center {
    justify-content: center;
    align-items: center;
    align-content: center;
    justify-items: center;
    text-align: center;
}

/* 子项目属性：让自己居中 */
.near-center {
    justify-self: center;
    align-self: center;
    margin: auto;
}
```

命名 `.item-near-center` 与现有 `.items-near-x-center` 等风格一致。无效属性会被浏览器忽略。

---

### 问题7：响应式部分直接修改元素而非通过类名

**涉及代码**：第218-230行

**问题描述**：
- 直接修改 `body` 和 `aside` 元素
- 与整份文件的"工具类"设计不一致

**讨论结论**：（暂缓，之后再处理）

---

## 已确认无误

### 亚字形、叵字形布局命名

- `.亚字形`：上横、下横、中间左右侧栏+主体，字形与布局匹配 ✓
- `.叵字形`：上横、下横、左侧边栏+主体，字形与布局匹配 ✓
- `.朋字形`：左右并列，字形与布局匹配 ✓
- `.目字形`：上下三段，字形与布局匹配 ✓