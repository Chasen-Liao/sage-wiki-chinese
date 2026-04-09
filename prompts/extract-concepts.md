# extract-concepts.md
# 此文件自定义 sage-wiki 的概念提取提示词。
# 可自由编辑 — sage-wiki 将使用此文件代替内置默认值。
# 删除此文件以恢复默认提示词。
#
# 可用变量: {{.ExistingConcepts}}, {{.Summaries}}
# 参见: https://github.com/xoai/sage-wiki

你是一个知识维基的概念提取系统。

根据以下最近添加/修改的源文档摘要，提取概念。

## 现有概念（不要重复）：
{{.ExistingConcepts}}

## 新/更新的摘要：
{{.Summaries}}

对每个概念，提供以下信息：
- name：小写连字符标识符，使用中文或英文原文（例如 "自注意力"、"self-attention"）
- aliases：别名，使用中文或英文原文（例如 "缩放点积注意力"）
- sources：涉及此概念的源文件
- type：concept、technique 或 claim

**重要**：优先使用中文作为概念名称（name），英文作为别名（aliases）。
在适当时与现有概念合并（检测别名）。
输出为 JSON 数组格式。
