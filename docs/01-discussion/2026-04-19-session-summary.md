# 今日讨论记录

日期：2026-04-19
状态：**已归档至 2026-04-20-session-summary.md**

---

## 目录整理

已建立四区目录结构：

| 目录 | 用途 |
|------|------|
| `docs/01-discussion/` | 讨论区 — 聊天日志、会议记录 |
| `docs/02-predesign/` | 预设计区 — 未经评审的草案 |
| `docs/03-design/` | 确认设计区 — 经评审确认的设计文档 |
| `src/` | 开发区 — 源码 |
| `data/` | 知识库数据 |
| `resources/` | 静态资源 |
| `scripts/` | 工具脚本 |

---

## 今日成果

### Phase 1 ✅ 完成
- 知识节点 schema 确认（YAML 格式）
- 5 节点验证集（k000→k004 依赖链）
- 极简 HTML 闭环（展示 → 标记完成 → localStorage）

### Phase 2 进行中 ✅ 完成（2026-04-20 图谱可视化已完成）
- `scripts/yaml2js.js` — 从 `data/knowledge-base.yaml` 生成 `src/data.js`（依赖 js-yaml）
- `src/index.html` 完整 UI：
  - 依赖解锁（前置知识未完成则锁定）
  - 标记完成 / 重置
  - 笔记功能（失焦自动保存）
  - 踩坑提醒展示
  - 进度统计（完成数、进度条、连续打卡天数）
  - 可展开卡片
  - 前置依赖提示
  - 全局重置
- **图谱可视化**（2026-04-20）：SVG DAG 图谱，拓扑分层布局，节点颜色区分状态，点击展开卡片，"图谱/列表"视图切换
- **朴素推荐策略**（2026-04-20）：`getRecommendation()` 按 `importance - difficulty + chain×0.5` 排序，首位节点显示"推荐"标签

---

## 当前文件结构

```
E:\note\learn-system\
├── CLAUDE.md
├── package.json               # 含 js-yaml 依赖
├── scripts/
│   └── yaml2js.js             # npm run gen 或 node scripts/yaml2js.js
├── src/
│   ├── index.html             # 主 UI（浏览器直接打开）
│   └── data.js                # 自动生成（勿手动编辑）
├── data/
│   └── knowledge-base.yaml    # 知识库源文件
└── docs/
    ├── 01-discussion/
    │   ├── 2026-04-19-learning-system-design-chatlog.md
    │   └── 2026-04-19-session-summary.md    # 本文件
    ├── 02-predesign/
    │   └── 2026-04-19-learning-system-design-draft.md
    └── 03-design/
        └── 2026-04-19-knowledge-node-schema.md
├── .claude/skills/
│   └── 下一步计划/    # 自定义 skill
```

---

## 完整工作流

```
1. 编辑 data/knowledge-base.yaml（添加/修改节点）
2. npm run gen   或   node scripts/yaml2js.js
3. 浏览器刷新 src/index.html
```

---

## 待做事项（按优先级）

1. ~~图谱可视化~~ ✅ **已完成** — SVG 绘制知识节点依赖关系图，拓扑分层布局
2. ~~推荐策略（朴素版）~~ ✅ **已完成** — 评分算法 `importance - difficulty + chain_length×0.5`，推荐节点顶部显示"推荐"标签高亮
3. **样式完善** — 暗色主题、移动端适配（Phase 3）
4. **知识库填充** — 用真实的 OpenGL/Vulkan 资料完善更多节点（长期）
5. **记忆曲线** — 基于遗忘曲线的复习提醒（Phase 3+）

---

## 技术细节备忘

- **YAML 解析**：用 `js-yaml` npm 包，不自写解析器
- **数据流**：`knowledge-base.yaml` → `yaml2js.js` → `data.js`（`KNOWLEDGE_BASE` 全局变量）→ `index.html`
- **用户状态**：`localStorage` key = `ls_v2`，结构 `{"k001": {done, completedAt, note, completionOrder}}`
- **依赖解锁逻辑**：`node.prerequisites.every(pid => completedIds.includes(pid))`
- **连续打卡计算**：扫描 `completedAt` 日期，从今天往回数连续天数
- **图谱布局算法**：拓扑分层（最长路径法），每层内 id 排序居中分布，贝塞尔曲线连接
- **视图切换**：`view` 全局变量，"list" ↔ "graph"，`toggleView()` 切换
- **推荐策略**：`getRecommendation()` 返回评分最高的可学节点，评分 = `importance - difficulty + prereq_count×0.5`

---

## 未决问题

- ~~知识图谱可视化实现方式~~ ✅ 已选定 SVG
- 推荐策略的"探索/巩固"平衡具体算法参数
- 多项目支持的具体设计方案（Phase 4）