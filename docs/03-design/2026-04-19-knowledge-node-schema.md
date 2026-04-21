# 知识节点 Schema 设计

日期：2026-04-19
状态：**已实现**

---

## 核心决策（已确认）

1. **数据与状态分离** — 静态知识描述在 YAML 中，用户状态（笔记、掌握度、遗忘曲线参数）在 localStorage 中独立管理
2. **显式依赖声明** — `prerequisites` 字段手动声明依赖，不做自动推断
3. **单字段类型区分** — 用 `type: concept|note|pitfall|practice` 而非多类继承
4. **本地路径优先** — 资源引用支持 `path`（本地文件）和 `url`（在线资源）
5. **YAML 数据与 HTML 分离** — `data/knowledge-base.yaml` 由 `scripts/yaml2js.js` 转换为 `src/data.js` 供 HTML 引用

---

## 知识节点 Schema

```yaml
nodes:
  - id: k001
    title: "着色器基础"
    description: "GLSL 语法、顶点/片段着色器编写"
    type: concept                    # concept | note | pitfall | practice
    difficulty: 2                    # 1-5，用户感知难度
    importance: 4                    # 1-5，对整体学习的影响程度
    tags: [shader, glsl]

    # 依赖关系：有向边从 prereq → 当前节点
    prerequisites: [k000]

    # 学习资源
    resources:
      - type: doc
        title: "LearnOpenGL - 着色器"
        url: "https://learnopengl.com/..."
      - type: source
        title: "示例源码"
        path: "./courses/01-intro/shaders/basic.cpp"
      - type: pitfall               # 踩坑记录嵌入资源中
        title: "VAO/VBO 绑定顺序"
        common_error: "..."
        cause: "..."
        solution: "..."

    estimated_minutes: 45
    related_nodes: [k015, k023]
```

### 用户状态字段（运行时，存储在 localStorage）

```json
{
  "k001": {
    "done": true,
    "completedAt": "2026-04-19",
    "note": "理解 Uniform 和 Attribute 的区别...",
    "completionOrder": 2
  }
}
```

---

## 字段说明

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `id` | string | 是 | 全局唯一标识 |
| `title` | string | 是 | 节点标题 |
| `description` | string | 否 | 简短描述 |
| `type` | enum | 是 | concept / note / pitfall / practice |
| `difficulty` | int 1-5 | 是 | 用户感知难度 |
| `importance` | int 1-5 | 是 | 对整体学习的影响程度 |
| `tags` | string[] | 否 | 便于检索和过滤 |
| `prerequisites` | string[] | 否 | 依赖节点 id 列表 |
| `resources` | Resource[] | 否 | 学习资源 |
| `pitfall` | object | 仅 pitfall 类型 | 踩坑详情 |
| `estimated_minutes` | int | 否 | 预计学习时长 |
| `related_nodes` | string[] | 否 | 相关节点 id |

---

## 工具链

```
data/knowledge-base.yaml
        │
        ▼ npm install js-yaml
scripts/yaml2js.js  ──生成──▶  src/data.js
                                          │
                                          ▼ 直接被 src/index.html <script src="data.js"> 引用
```

---

## 目录组织

```
data/
  knowledge-base.yaml       # 知识库源文件（只读）
src/
  index.html                # 主 UI（浏览器直接打开）
  data.js                   # 自动生成（不要手动编辑）
scripts/
  yaml2js.js                # npm run gen 或 node scripts/yaml2js.js
package.json
```

---

## Phase 1 验证集（5 节点依赖链）

```
k000 图形渲染管线概述
  ↓
k001 着色器基础
  ↓
k002 顶点缓冲区 (VBO/VAO)
  ↓
k003 纹理映射
  ↓
k004 帧缓冲 (FBO)
```

---

## 实现状态

| 功能 | 状态 |
|------|------|
| 知识节点 schema | ✅ |
| 初始 5 节点验证集 | ✅ |
| YAML → JS 自动同步 (yaml2js.js) | ✅ |
| 依赖解锁（前置知识未完成则锁定）| ✅ |
| 标记完成 / 重置 | ✅ |
| 笔记功能 | ✅ |
| 踩坑提醒展示 | ✅ |
| 进度统计（完成数、进度条、连续打卡）| ✅ |
| 资源链接（URL / 本地路径）| ✅ |
| 图谱可视化 | ⏳ 待做 |
| 推荐策略（朴素版）| ⏳ 待做 |