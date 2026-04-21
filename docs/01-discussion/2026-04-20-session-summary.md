# 今日讨论记录

日期：2026-04-20
状态：**进行中（待续）**

---

## 今日成果

### Phase 2 ✅ 完成（收尾）

- **图谱可视化**：SVG DAG 图谱，拓扑分层布局，节点颜色区分状态（绿=已完成、蓝=可学、灰=锁定），贝塞尔曲线箭头，"图谱/列表"视图切换，点击图谱节点展开卡片
- **朴素推荐策略**：`getRecommendation()` 评分算法 `importance - difficulty + prereq_count×0.5`，首位可学节点显示"推荐"紫色标签

### 自定义 Skill ✅ 完成

- `.claude/skills/下一步计划/SKILL.md` — 输入 `/下一步计划` 查看讨论区并给出下一步建议

---

## 当前文件结构

```
E:\note\learn-system\
├── CLAUDE.md
├── package.json
├── scripts/
│   └── yaml2js.js
├── src/
│   ├── index.html          # 主 UI（含图谱 + 推荐功能）
│   └── data.js             # 自动生成
├── data/
│   └── knowledge-base.yaml # 知识库源文件
├── .claude/skills/
│   └── 下一步计划/          # 自定义 skill
└── docs/
    ├── 01-discussion/
    │   ├── 2026-04-19-session-summary.md  # 已归档
    │   └── 2026-04-20-session-summary.md  # 本文件
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

## 待做事项（按优先级）

1. **样式完善** — 暗色主题、移动端适配（Phase 3）
2. **知识库填充** — 用真实的 OpenGL/Vulkan 资料完善更多节点（长期）
3. **记忆曲线** — 基于遗忘曲线的复习提醒（Phase 3+）

---

## 技术细节备忘

- **YAML 解析**：js-yaml npm 包
- **数据流**：knowledge-base.yaml → yaml2js.js → data.js（KNOWLEDGE_BASE 全局变量）→ index.html
- **用户状态**：localStorage key = `ls_v2`，结构 `{"k001": {done, completedAt, note, completionOrder}}`
- **图谱布局**：拓扑分层（最长路径法），贝塞尔曲线连接
- **视图切换**：`view` 全局变量，`toggleView()`，"list" ↔ "graph"
- **推荐策略**：`getRecommendation()`，评分 = `importance - difficulty + prereq_count×0.5`

---

## 未决问题

- 推荐策略的"探索/巩固"平衡具体算法参数
- 多项目支持的具体设计方案（Phase 4）