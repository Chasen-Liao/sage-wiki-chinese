# CLAUDE.md

此仓库是 sage-wiki 的中文提示词模板集合，用于自定义文档摘要、概念提取和文章写作行为。

## 仓库结构

```
sage-wiki-chinese/
├── prompts/              # 提示词模板目录
│   ├── summarize-article.md   # 文章摘要
│   ├── summarize-paper.md     # 论文摘要
│   ├── extract-concepts.md    # 概念提取
│   ├── write-article.md       # 文章写作
│   └── caption-image.md       # 图片描述
└── README.md             # 详细使用文档
```

## 提示词工作原理

sage-wiki 在编译时自动扫描项目根目录的 `prompts/` 文件夹，使用自定义提示词覆盖内置默认值。无需额外配置。

文件命名转换：内置名 `summarize_article.txt` → 用户名 `summarize-article.md`（下划线改中划线，`.txt` 改 `.md`）

## 模板语法

使用 Go `text/template` 语法。关键变量：

| 提示词 | 关键变量 |
|--------|----------|
| summarize-article/paper | `{{.SourcePath}}`, `{{.SourceType}}`, `{{.MaxTokens}}` |
| extract-concepts | `{{.ExistingConcepts}}`, `{{.Summaries}}` |
| write-article | `{{.ConceptName}}`, `{{.ConceptID}}`, `{{.Sources}}`, `{{.RelatedConcepts}}`, `{{.MaxTokens}}` |
| caption-image | `{{.SourcePath}}` |

## 常用操作

- **添加新提示词**：在 `prompts/` 创建新的 `.md` 文件，遵循命名规则
- **测试提示词**：`sage-wiki compile --fresh`
- **恢复默认**：删除 `prompts/` 目录下的对应文件

详细文档参见根目录 `README.md`。

## sage-wiki 简介

sage-wiki 是一个 AI 驱动的个人知识维基工具，自动从源文档生成结构化摘要、提取概念并撰写维基文章。详见 https://github.com/xoai/sage-wiki
