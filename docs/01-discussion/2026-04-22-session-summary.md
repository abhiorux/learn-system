# 今日讨论记录

日期：2026-04-22
状态：**已完成**

---

## 当前文件结构

```
E:\note\learn-system/
├── CLAUDE.md
├── package.json
├── scripts/
│   └── yaml2js.js
├── src/
│   ├── index.html   # 主 UI（图谱 + 推荐 + 暗色主题 + 移动端）
│   └── data.js      # 自动生成（18节点）
├── data/
│   └── knowledge-base.yaml
└── docs/
    └── 01-discussion/
        ├── 2026-04-19-session-summary.md
        ├── 2026-04-20-session-summary.md
        └── 2026-04-22-session-summary.md  # 本文件
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

---

## 今日完成

### Phase 3 ✅ 全部完成

- **移动端适配**：`viewport meta` + 480px/360px 两档媒体查询，图谱横向惯性滚动，节点ID小屏隐藏
- **知识库填充**：5 节点 → 18 节点（commit `a1fe7e2`）

### 知识库结构

| 节点段 | 数量 | 内容 |
|--------|------|------|
| k000-k004 | 5 | OpenGL 基础链 |
| k101-k107 | 7 | OpenGL 进阶：UBO、深度/模板、混合、实例化、GS/TS、阴影映射、PBR |
| k200-k205 | 6 | Vulkan 入门：概述、Instance/Device、Swapchain、Command/RP、Desc/Pipeline、同步 |

### Git 提交

| 提交 | 内容 |
|------|------|
| `7320aa7` | Add dark mode theme toggle with localStorage persistence |
| `a1fe7e2` | feat: add mobile responsive layout and expand knowledge base to 18 nodes |

---

## 下一步计划

### A. 记忆曲线复习提醒（推荐优先）

基于 CLAUDE.md 中定义的遗忘曲线模型 `mastery(t) = initial × e^(-t/τ)` 实现提醒：

**核心逻辑**：
1. 节点完成时记录 `completedAt`
2. 每次渲染时计算 `daysSince = now - completedAt`
3. 若 `e^(-daysSince/τ)` 低于阈值（如 50%），在 UI 中标记"建议复习"
4. 可在 stats 栏显示"待复习 N 个"

**τ 个性化**：可根据用户实际重学次数微调，初期用固定值（如 τ=7 天）

**UI 表现**：
- 节点卡片顶部加"复习提醒"标签（橙黄色）
- stats 栏新增"待复习"计数

### B. 知识库继续扩展（长期）

- OpenGL 进阶可补充：延迟渲染、GBuffer、PBR 细节
- Vulkan 进阶： VK_KHR_surface、Texture 采样、Depth Image、Compute Shader
- 添加更多 pitfall 记录（实操中踩到的坑价值最高）

### C. Phase 4 发布（可选）

GitHub Pages / npm / iframe widget 方案选型

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