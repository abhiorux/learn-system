# 今日讨论记录

日期：2026-04-22
状态：**进行中（待续）**

---

## 今日成果

### Phase 2 ✅ 完成

- **暗色主题确认**：`toggleTheme()` + localStorage (`ls_theme` key) + 按钮 UI，完整实现
- **笔记功能确认**：展开节点卡片内 textarea，`blur` 时 `saveNote()` 自动保存到 `ls_v2`
- **Git 提交**：`7320aa7` — Add dark mode theme toggle with localStorage persistence

---

## 当前文件结构

```
E:\note\learn-system/
├── CLAUDE.md
├── package.json
├── scripts/
│   └── yaml2js.js
├── src/
│   ├── index.html  # 主 UI（图谱 + 推荐 + 暗色主题）
│   └── data.js     # 自动生成
├── data/
│   └── knowledge-base.yaml
├── .claude/skills/
│   └── 下一步计划/
└── docs/
    ├── 01-discussion/
    │   ├── 2026-04-19-session-summary.md
    │   ├── 2026-04-20-session-summary.md
    │   └── 2026-04-22-session-summary.md  # 本文件
    ├── 02-predesign/
    │   └── 2026-04-19-learning-system-design-draft.md
    └── 03-design/
        └── 2026-04-19-knowledge-node-schema.md
```

---

## 完整工作流

```bash
1. 编辑 data/knowledge-base.yaml
2. npm run gen 或 node scripts/yaml2js.js
3. 浏览器刷新 src/index.html
```

---

## Phase 3 进度

| 功能 | 状态 |
|------|------|
| 暗色主题 | ✅ 已完成 |
| 图谱视图 | ✅ 已完成 |
| 笔记功能 | ✅ 已完成 |
| 移动端适配 | ✅ 已完成 |

### 移动端适配细节

- `viewport meta` + 两档 media query（480px / 360px）
- 节点 ID 小屏隐藏、图谱横向惯性滚动（`-webkit-overflow-scrolling: touch`）
- 按钮最小高度 40px（触屏友好）

## 今日完成

### Phase 3 ✅ 全部完成

- **移动端适配**：`viewport meta` + 480px/360px 两档媒体查询，图谱横向滑动，节点ID小屏隐藏
- **知识库填充**：18 节点（k000-k205），覆盖 OpenGL 进阶（k101-k107）+ Vulkan 入门（k200-k205）

### 知识库结构

| 节点段 | 数量 | 内容 |
|--------|------|------|
| k000-k004 | 5 | OpenGL 基础链（原有） |
| k101-k107 | 7 | OpenGL 进阶：UBO、深度/模板、混合、实例化、GS/TS、Shadow、PBR |
| k200-k205 | 6 | Vulkan 入门：概述、Instance/Device、Swapchain、Command/RP、Desc/Pipeline、同步 |

## 下一步计划

### Phase 4 发布准备（可选）

1. **知识库填充** — 用真实的 OpenGL/Vulkan 资料完善更多节点（长期、YAML 编辑即可）
2. **记忆曲线提醒** — 基于 `mastery(t) = initial × e^(-t/τ)` 的复习触发逻辑（Phase 3+）
3. **Phase 4 发布** — GitHub Pages / npm / iframe widget 方案选型

---

## 技术细节备忘

- **YAML 解析**：js-yaml npm 包
- **数据流**：knowledge-base.yaml → yaml2js.js → data.js（KNOWLEDGE_BASE 全局变量）→ index.html
- **用户状态**：localStorage key = `ls_v2`，结构 `{"k001": {done, completedAt, note, completionOrder}}`
- **图谱布局**：拓扑分层（最长路径法），贝塞尔曲线连接
- **视图切换**：`view` 全局变量，`toggleView()`，"list" ↔ "graph"
- **推荐策略**：`getRecommendation()`，评分 = `importance - difficulty + prereq_count×0.5`
- **暗色主题**：`toggleTheme()` + localStorage key `ls_theme`

---

## 未决问题

- 推荐策略的"探索/巩固"平衡具体算法参数
- 多项目支持的具体设计方案（Phase 4）