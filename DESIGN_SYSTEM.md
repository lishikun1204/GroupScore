# 主界面视觉设计规范（Tkinter）

本项目为 Tkinter 单机桌面客户端。此文档用于统一主界面配色、间距、排版与响应式规则，确保可读性与一致性。

## 1. 配色体系

### 1.1 语义颜色（Theme 字段）

颜色定义位置：`Theme / LIGHT_THEME / DARK_THEME`（见 [main.py](file:///c:/Users/Administrator/PycharmProjects/PythonProject/main.py)）。

#### Light
- 背景（bg_primary）：#f6f8fb
- 卡片/面板（bg_secondary）：#ffffff
- 浅层背景（bg_surface）：#eef2f7
- 主文本（text_primary）：#0f172a
- 次文本（text_secondary）：#475569
- 边框（border）：#cbd5e1
- 主色（accent_blue）：#2563eb
- 成功（accent_green）：#15803d
- 危险（accent_red）：#b91c1c
- 辅助强调（accent_purple）：#7c3aed
- 中性强调（accent_neutral）：#334155

#### Dark
- 背景（bg_primary）：#0b1220
- 卡片/面板（bg_secondary）：#0f172a
- 浅层背景（bg_surface）：#111c33
- 主文本（text_primary）：#e2e8f0
- 次文本（text_secondary）：#94a3b8
- 边框（border）：#23304a
- 主色（accent_blue）：#3b82f6
- 成功（accent_green）：#22c55e
- 危险（accent_red）：#ef4444
- 辅助强调（accent_purple）：#a78bfa
- 中性强调（accent_neutral）：#94a3b8

### 1.2 可访问性与对比度（WCAG 2.1 AA）

- 所有“彩色背景上的文字”统一通过 `_best_text_color(bg)`/`_fg_on(bg)` 自动选择前景色，优先保证对比度。
- 目标：普通文本对比度 ≥ 4.5:1（AA），大号文本 ≥ 3:1（AA）。

## 2. 排版（Typography）

字体定义入口：`_configure_fonts`（见 [main.py](file:///c:/Users/Administrator/PycharmProjects/PythonProject/main.py)）。

- 正文：10px × `font_scale`
- 辅助：9px × `font_scale`
- 标题：11px × `font_scale`（bold）
- 头部：12px × `font_scale`（bold）
- 分数：34px × `font_scale`（bold）

## 3. 间距体系（Spacing）

间距由断点动态控制（`_apply_responsive_layout`），核心变量：
- 外边距：`_outer_pad`
- 卡片间距：`_card_gap`
- 卡片内边距：`_card_inner_pad`

推荐理解为一套 10/12/14/16 的尺度系统，按设备宽度自动选择。

## 4. 响应式断点（Breakpoints）

断点规则定义入口：`_apply_responsive_layout(width)`（见 [main.py](file:///c:/Users/Administrator/PycharmProjects/PythonProject/main.py)）。

- Desktop（1920）：`width >= 1920`
  - 布局：左右分栏（side）
  - 卡片列数：3 列（6 组呈 2 行）
- Desktop（1440）：`1440 <= width < 1920`
  - 布局：左右分栏（side）
  - 卡片列数：2 列（6 组呈 3 行）
- Tablet（768）：`768 <= width < 1440`
  - 布局：上下堆叠（stack）
  - 卡片列数：2 列
- Mobile（375）：`width < 768`
  - 布局：上下堆叠（stack）
  - 卡片列数：1 列
  - 卡片区启用滚动容器以保证可用性

## 5. 组件状态样式（States）

当前实现（可用且已统一）：
- 卡片 hover：边框高亮（`highlightthickness` 变化）
- Treeview 选中态：背景使用主色，文字使用自动前景色

建议后续增强（如果需要更强的“品牌动效”）：
- 按钮 hover/pressed：通过 `<Enter>/<Leave>/<ButtonPress>/<ButtonRelease>` 切换背景色的轻微明度
- 输入框 focus：通过 `highlightthickness` 与 `highlightbackground` 强化聚焦状态

## 6. 测试与验收（建议流程）

### 6.1 尺寸预览
应用菜单「视图」提供 1920/1440/768/375 的一键窗口尺寸预览，确保断点切换正确。

### 6.2 可用性测试与反馈收集
- 菜单「帮助 → 可用性反馈」打开内置反馈表单
- 反馈将追加写入 `perf_artifacts/feedback.jsonl`（每行一条 JSON）

### 6.3 性能指标（桌面端等价指标）
Web 的 LCP/FID/CLS 不适用于 Tkinter。桌面端建议以以下指标替代：
- 启动到可交互时间（TTI）：程序启动到首屏渲染完成
- 交互延迟：按钮点击到 UI 更新完成的耗时
- 调整窗口尺寸抖动：多次 resize 时是否出现明显卡顿/重排闪烁

