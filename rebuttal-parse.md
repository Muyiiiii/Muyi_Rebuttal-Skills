---
name: rebuttal-parse
description: "Rebuttal Phase 0: Parse OpenReview HTML → structured analysis.md"
---

# Rebuttal Phase 0: Parse OpenReview HTML → analysis.md

## 输入
用户提供OpenReview的HTML文件路径（可能是保存的网页或多个文件）。

## 任务

将OpenReview的reviewer comments解析为结构化的markdown文件。

### 步骤

1. **读取HTML文件**：用Bash运行 `cat` 或用Read工具读取HTML内容。如果是多个文件，逐个处理。

2. **提取关键信息**：对每个reviewer提取：
   - Reviewer ID（如 R4cH, z2T2 等）
   - Overall Score（如 3, 5, 7）
   - Confidence Score
   - Summary
   - Strengths（逐条）
   - Weaknesses（逐条）
   - Questions（逐条）
   - Suggestions（如有）
   - Limitations（如有）

3. **生成结构化markdown**：按以下格式输出到 `Rebuttal/analysis_{论文简称}.md`：

```markdown
# ICML'26 Rebuttal of {论文名}

Scores: {R1分数}/{R2分数}/{R3分数}/{R4分数} ({WA/WR/R/R标注})

# Reviewer {ID} ({分数}/{满分})

## Strengths
1. ...
2. ...

## Weaknesses
1. ==高亮reviewer的核心要求==
...

## Key Questions For Authors
1. ==高亮reviewer的核心问题==
...

---
```

4. **标注重点**：用 `==高亮==` 标记每条weakness和question中reviewer明确要求补充的内容（实验、解释、修正等）。

5. **生成总览表**：在文件开头添加：

```markdown
| Reviewer | Score | 态度 | 核心关切 |
|----------|-------|------|---------|
| {ID} | {分数} | WA/WR/R | 一句话总结 |
```

## 输出
- `Rebuttal/analysis_{论文简称}.md` — 结构化的reviewer comments
- 向用户报告：reviewer数量、分数分布、最关键的concerns
