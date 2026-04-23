# 今日讨论记录

日期：2026-04-23
状态：**未完成（暂停）**

---

## 当前文件结构

```
E:\note\learn-system/
├── CLAUDE.md
├── package.json
├── scripts/
│   └── yaml2js.js
├── src/
│   ├── index.html  # 主 UI（复习提醒功能）
│   └── data.js     # 自动生成（24节点）
├── data/
│   └── knowledge-base.yaml
└── docs/
    └── 01-discussion/
        ├── 2026-04-19-session-summary.md
        ├── 2026-04-20-session-summary.md
        ├── 2026-04-22-session-summary.md
        └── 2026-04-23-session-summary.md  # 本文件
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
| 记忆曲线复习提醒 | ✅ 今日完成 |

---

## 今日完成

### 记忆曲线复习提醒 ✅

基于遗忘曲线 `mastery(t) = e^(-t/τ)` 实现复习提醒功能：

**核心参数**：τ = 7 天（半衰期），阈值 50%

**新增代码（src/index.html）**：
- `calcMastery(nodeId)` — 计算当前掌握度
- `getDueReviewIds()` — 筛选低于阈值的已完成节点
- `masteryBar(nodeId)` — 卡片内掌握度进度条（绿/紫/橙三色）
- stats 栏新增"待复习 N 个"计数
- 节点卡头加"复习"橙色徽章 + "复习提醒"标签

**衰减曲线参考**：
| 距完成 | 掌握度 | 是否复习 |
|--------|--------|----------|
| 1天 | 83% | 否 |
| 3天 | 63% | 否 |
| 5天 | 47% | 是 |
| 7天 | 35% | 是 |
| 14天 | 13% | 是 |

### 知识库扩展 18 → 24 节点 ✅

**新增节点**：
| 节点 | 内容 | Pitfall 数 |
|------|------|-----------|
| k301 | Vulkan 纹理与采样器 | 2 |
| k302 | Vulkan 深度图像 | 2 |
| k303 | Vulkan Compute Shader | 2 |
| k304 | Vulkan 多遍/延迟渲染 | 2 |
| k108 | OpenGL 延迟渲染 G-Buffer | 1 |
| k109 | SSAO / Bloom / Tone Mapping | 2 |

**延伸路径**：
- Vulkan 路径：k205（同步）→ k301 → k302 → k303 → k304
- OpenGL 路径：k107（PBR）→ k108 → k109

---

## 明日待办

1. 继续知识库扩展（PBR 细节补充、Vulkan 拓展更多实际场景）
2. Phase 4 发布方案选型（GitHub Pages / npm / iframe widget）
3. 可选：τ 个性化（根据用户重学次数自动调整半衰期）

---

## 技术细节备忘

- **复习提醒**：τ = 7天，REVIEW_THRESHOLD = 0.5，`calcMastery` 基于 `completedAt` 时间戳
- **用户状态**：localStorage key = `ls_v2`
- **数据生成**：`npm run gen` → `src/data.js`，KNOWLEDGE_BASE 全局变量

---

## 未决问题

- τ 值是否需要动态个性化（基于重学次数调整）
- Phase 4 发布方案选型