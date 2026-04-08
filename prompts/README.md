# Sage-Wiki 自定义提示词使用方法

## 概述

Sage-wiki 支持用户自定义所有 LLM 提示词，用于控制文档摘要生成、概念提取、文章写作等行为。自定义提示词可以完全覆盖内置默认值。

---

## 提示词文件说明

| 文件名 | 用途 | 模板变量 |
|--------|------|----------|
| `summarize-article.md` | 文章摘要 | `{{.SourcePath}}`, `{{.SourceType}}`, `{{.MaxTokens}}` |
| `summarize-paper.md` | 论文摘要 | `{{.SourcePath}}`, `{{.SourceType}}`, `{{.MaxTokens}}` |
| `extract-concepts.md` | 概念提取 | `{{.ExistingConcepts}}`, `{{.Summaries}}` |
| `write-article.md` | 文章写作 | `{{.ConceptName}}`, `{{.ConceptID}}`, `{{.Sources}}`, `{{.RelatedConcepts}}`, `{{.ExistingArticle}}`, `{{.Learnings}}`, `{{.Aliases}}`, `{{.SourceList}}`, `{{.RelatedList}}`, `{{.Confidence}}`, `{{.MaxTokens}}` |
| `caption-image.md` | 图片描述 | `{{.SourcePath}}` |
| `capture-knowledge.md` | 知识捕获 | `{{.Context}}`, `{{.Tags}}` |

---

## 快速开始

### 1. 生成提示词模板

在项目目录下运行：

```bash
sage-wiki init --prompts
```

这会在 `prompts/` 目录下生成所有默认提示词模板文件。

### 2. 编辑提示词

直接编辑 `prompts/` 目录下的 `.md` 文件，修改为你需要的内容。

### 3. 激活自定义提示词

**自动加载**：sage-wiki 在编译时会**自动扫描**项目根目录下的 `prompts/` 文件夹，自动加载自定义提示词覆盖内置默认值。

无需额外配置，程序会自动发现并使用。

### 4. 验证生效

运行编译并观察输出：

```bash
sage-wiki compile --fresh
```

如果提示词被正确加载，程序会使用你编辑的内容。

---

## 文件命名规则

| 内置文件名 | 用户文件名 | 转换规则 |
|------------|------------|----------|
| `summarize_article.txt` | `summarize-article.md` | 下划线 → 中划线，`.txt` → `.md` |
| `summarize_paper.txt` | `summarize-paper.md` | 同上 |
| `extract_concepts.txt` | `extract-concepts.md` | 同上 |
| `write_article.txt` | `write-article.md` | 同上 |
| `caption_image.txt` | `caption-image.md` | 同上 |
| `capture_knowledge.txt` | `capture-knowledge.md` | 同上 |

---

## 模板语法

Sage-wiki 使用 Go `text/template` 语法。

### 变量输出

```
{{.VariableName}}
```

### 条件判断

```
{{if .ExistingArticle}}
这里的内容只在 .ExistingArticle 不为空时渲染
{{end}}
```

### 循环遍历

```
{{range .RelatedConcepts}}
- [[{{.}}]]
{{end}}
```

---

## 各提示词详解

### summarize-article.md（文章摘要）

**何时触发**：对非论文类文档（如网页、博客、技术文档）生成结构化摘要时使用。

**可用变量**：
- `{{.SourcePath}}` — 源文件路径
- `{{.SourceType}}` — 源文件类型（如 "article", "web", "doc"）
- `{{.MaxTokens}}` — 最大 token 数限制

**输出结构**（建议保留）：
```
## 关键主张
## 方法论
## 结果
## 概念
```

---

### summarize-paper.md（论文摘要）

**何时触发**：对学术论文（SourceType 为 "paper"）生成摘要时使用。

**可用变量**：
- `{{.SourcePath}}` — 源文件路径
- `{{.SourceType}}` — 固定为 "paper"
- `{{.MaxTokens}}` — 最大 token 数限制

**输出结构**（建议保留）：
```
## 关键主张
## 方法论
## 结果
## 局限性
## 概念
```

---

### extract-concepts.md（概念提取）

**何时触发**：从已摘要的文档中提取概念、建立概念关联时使用。

**可用变量**：
- `{{.ExistingConcepts}}` — 已有的概念列表（避免重复）
- `{{.Summaries}}` — 所有文档摘要内容

**输出要求**：必须输出 JSON 数组格式，每个概念包含：
```json
{
  "name": "概念标识符",
  "aliases": ["别名1", "别名2"],
  "sources": ["源文件路径"],
  "type": "concept | technique | claim"
}
```

---

### write-article.md（文章写作）

**何时触发**：根据提取的概念撰写完整维基文章时使用。

**可用变量**：
- `{{.ConceptName}}` — 概念名称
- `{{.ConceptID}}` — 概念唯一标识符（小写连字符）
- `{{.Sources}}` — 来源文件列表
- `{{.RelatedConcepts}}` — 相关概念列表
- `{{.ExistingArticle}}` — 已有的文章内容（如有，用于更新/扩展）
- `{{.Learnings}}` — 之前编译的学习笔记
- `{{.Aliases}}` — 概念别名
- `{{.SourceList}}` — 格式化后的源列表
- `{{.RelatedList}}` — 格式化后的相关概念列表
- `{{.Confidence}}` — 置信度
- `{{.MaxTokens}}` — 最大 token 数限制

**输出结构**（建议保留）：
```
---
概念 YAML frontmatter
---
## 定义
## 工作原理
## 变体
## 权衡
## 参见
```

---

### caption-image.md（图片描述）

**何时触发**：为文档中的图片生成描述性标题时使用。

**可用变量**：
- `{{.SourcePath}}` — 图片文件路径

---

### capture-knowledge.md（知识捕获）

**何时触发**：从对话文本中提取值得保留的知识时使用（MCP 工具 `wiki_capture` 调用）。

**可用变量**：
- `{{.Context}}` — 可选的上下文信息
- `{{.Tags}}` — 可选的标签列表

**输出要求**：必须输出 JSON 数组格式：
```json
[{"title": "short-slug-title", "content": "知识内容..."}, ...]
```
如果没有值得提取的内容，返回空数组 `[]`。

---

## 恢复默认提示词

删除 `prompts/` 目录下的对应文件，或删除整个 `prompts/` 目录，sage-wiki 会自动回退到内置默认提示词。

---

## 常见问题

### Q: 为什么编辑后没生效？

1. 确认文件**名称格式**正确（必须是 `summarize-article.md` 而不是 `summarize_article.md`）
2. 确认文件在项目**根目录**的 `prompts/` 下
3. 确认**编译时使用正确的项目目录**（`--project` 参数）

### Q: 可以只自定义部分提示词吗？

可以。只编辑你想要自定义的提示词文件，其他文件删除或留空，sage-wiki 会自动使用内置默认值补充。

### Q: 提示词有语法错误会怎样？

sage-wiki 会报错并停止编译。检查模板语法，特别是 `{{}}` 配对和 `{{if}}...{{end}}` 配对。

---

## 注意事项

1. **保留结构标记**（如 `## 关键主张`）有助于 sage-wiki 解析输出
2. **JSON 输出必须合法**（extract-concepts 提示词）
3. **YAML frontmatter 必须有效**（write-article 提示词）
4. **变量名大小写敏感**，`{{.SourcePath}}` 不能写成 `{{.sourcepath}}`
