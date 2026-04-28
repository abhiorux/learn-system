# 今日讨论记录

日期：2026-04-28
状态：**Phase 4 完成**

---

## 今日完成

### Phase 4 发布方案选型 ✅

| 方案 | 评价 |
|------|------|
| **GitHub Pages** | ✅ 选用 — 纯静态站点，零成本，天然匹配 |
| npm | ❌ 不适配（非可复用库） |
| iframe widget | ❌ 过度复杂，无实际需求 |

### GitHub Pages 发布 ✅

- 创建 `gh-pages` orphan 分支，仅包含 `index.html` + `data.js` + `.nojekyll`
- 站点上线：https://abhiorux.github.io/learn-system/
- GitHub repo：https://github.com/abhiorux/learn-system

### README 文档 ✅

- 新增 `README.md`，含 quick start、项目结构、工作原理、设计原则

---

## 更新流程（当前）

更新知识库后重新部署：

```bash
npm run gen
git checkout gh-pages && git checkout main -- src/data.js && mv src/data.js data.js && git add data.js && git commit -m "data: update" && git push
git checkout main
```

待优化：写一个 `npm run deploy` 脚本自动化此流程。

---

## 明日待办

1. 便捷部署脚本（`npm run deploy`）
2. 知识库扩展（PBR 细节 + Vulkan 实际场景）
3. τ 个性化（根据重学次数动态调整半衰期）

---

## Phase 进度总览

| Phase | 内容 | 状态 |
|-------|------|------|
| Phase 1 | MVP 单 HTML + YAML + localStorage | ✅ |
| Phase 2 | 通用化 config schema + 多项目 | ✅ |
| Phase 3 | 主题/图谱/笔记/移动端/复习提醒 | ✅ |
| Phase 4 | GitHub Pages 发布 | ✅ |
