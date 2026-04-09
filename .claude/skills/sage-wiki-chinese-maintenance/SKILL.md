---
name: sage-wiki-chinese-maintenance
description: Use when maintaining sage-wiki 中文提示词仓库，需要同步上游变化、更新提示词或添加版本说明
---

# Sage-Wiki 中文提示词维护

## Overview

维护 sage-wiki-chinese 仓库与上游 sage-wiki 同步的中文提示词版本。

## 仓库对应关系

| 本地目录 | 说明 |
|---------|------|
| `E:\MyProjects\sage-wiki` | 上游 sage-wiki 本地 clone |
| `E:\MyProjects\sage-wiki-chinese` | 本仓库（中文提示词）|

## 维护流程

### 1. 检查上游变化

```bash
# 查看上游最近提交
cd /e/MyProjects/sage-wiki && git log --oneline -20

# 查看涉及提示词/文档的改动
cd /e/MyProjects/sage-wiki && git diff HEAD~10 --name-only | grep -E "(prompts|CHANGELOG|README|template)"
```

### 2. 对比提示词模板

上游模板位置：`/e/MyProjects/sage-wiki/internal/prompts/templates/`

本仓库模板位置：`E:\MyProjects\sage-wiki-chinese\prompts\`

需要同步的文件：
- `summarize_article.txt` → `summarize-article.md`
- `summarize_paper.txt` → `summarize-paper.md`
- `extract_concepts.txt` → `extract-concepts.md`
- `write_article.txt` → `write-article.md`
- `caption_image.txt` → `caption-image.md`
- `capture_knowledge.txt` → `capture-knowledge.md`

### 3. 判断是否需要更新

| 情况 | 操作 |
|------|------|
| 上游提示词有变化 | 翻译更新中文版本 |
| 上游新增提示词 | 创建对应的中文版本 |
| 上游只有功能变化 | 在 README 添加版本特性说明 |

### 4. 更新版本历史

在仓库根目录 `README.md` 顶部添加版本更新说明：

```markdown
## 版本历史

### 0.1.x — YYYY-MM-DD
[上游版本特性简要中文说明]
```

参考 `CHANGELOG.md` 编写中文摘要。

## 快速检查清单

- [ ] `git log --oneline -20` 查看上游最新变化
- [ ] 对比 `internal/prompts/templates/` 与 `prompts/` 目录
- [ ] 提示词有变化 → 翻译/更新中文版本
- [ ] 有新功能 → 在根目录 README 添加版本说明
- [ ] 提交到 git
