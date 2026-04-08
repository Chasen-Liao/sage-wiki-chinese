# Sage-Wiki 中文提示词

本仓库提供 sage-wiki 的**中文提示词模板**，让 sage-wiki 生成中文维基内容。

---

## Sage-Wiki 是什么？

[Sage-Wiki](https://github.com/xoai/sage-wiki) 是一个 AI 驱动的个人知识维基工具，通过 LLM 自动完成以下工作：

- **文档摘要** — 对文章、论文进行结构化摘要
- **概念提取** — 从文档中提取概念、建立关联
- **文章写作** — 根据概念和来源自动撰写维基文章

它用 Go 实现，支持自定义所有提示词，本仓库即为中文提示词集合。

---

## 快速开始

### ⚠️ 重要：先执行 init，再配置提示词

> **警告**：`sage-wiki init --prompts` 会**覆盖**你已有的配置文件（包括项目配置、自定义设置等），恢复为默认配置。
>
> **正确顺序**：先复制本仓库的提示词文件到valut根目录中，然后在使用 `sage-wiki init --prompts` 初始化项目，之后再填写项目配置

### 步骤 1：复制中文提示词到 vault 根目录

将本仓库的 `prompts/` 目录复制到你的 wiki 项目根目录：

```bash
# 例如你的 wiki 项目在 ~/my-wiki
cp -r prompts ~/my-wiki/
```

### 步骤 2：初始化项目

```bash
cd ~/my-wiki
sage-wiki init --prompts
```

此时 `init` 会覆盖配置，但由于提示词文件已经存在，**不会覆盖 prompts 目录**（仅当目录不存在时才会创建）。

### 步骤 3：填写项目配置

初始化后，按提示填写项目配置（LLM 配置等）。

### 步骤 4：验证

```bash
sage-wiki compile --fresh
```

现在 sage-wiki 会使用中文提示词，生成中文维基内容。

---

## 提示词文件说明

| 文件 | 用途 |
|------|------|
| `summarize-article.md` | 文章摘要（网页、博客、技术文档） |
| `summarize-paper.md` | 论文摘要 |
| `extract-concepts.md` | 从摘要中提取概念 |
| `write-article.md` | 撰写维基文章 |
| `caption-image.md` | 图片描述 |

详细模板变量说明参见 [prompts/README.md](prompts/README.md)。

---

## 与上游保持同步

Sage-Wiki 持续更新，本仓库会跟随上游发布新版本提示词。

- **上游更新时**：对比上游默认提示词与本仓库版本，合并变更
- **上游新提示词**：及时添加对应的中文版本

欢迎提交 PR 共同维护。

---

## 自定义修改

你可以自由编辑 `prompts/` 下的 `.md` 文件，调整提示词行为。建议保留结构标记（如 `## 关键主张`）以便解析。

---

## 参考资料

- [Sage-Wiki 官方仓库](https://github.com/xoai/sage-wiki)
- [详细提示词文档](prompts/README.md)
