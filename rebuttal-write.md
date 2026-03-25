---
name: rebuttal-write
description: "Rebuttal Phase 4: Fill in results and generate final rebuttal document"
---

# Rebuttal Phase 4: Write — 填充实验结果，生成最终rebuttal

## 输入
用户提供：
- `analysis_{论文简称}.md`（含回复模板）
- 实验结果（可能是：日志文件路径、结果CSV/JSON、用户口述的数字、截图）

## 任务

将实验结果填入回复模板，生成提交到OpenReview的最终rebuttal文档。

### 步骤

1. **收集实验结果**：
   - 读取用户提供的结果文件
   - 如有collect_results.py，运行它汇总结果
   - 将数字填入analysis.md中的表格模板

2. **完善回复模板**：

   对每条W/Q的回复：
   - 填入具体实验数字
   - 根据实际结果调整措辞（预期结论 vs 实际结论）
   - 如果结果不如预期：
     - **不要隐藏负面结果**，而是诚实讨论并给出解释
     - 调整claim的强度
     - 补充limitation讨论
   - 确保引用的Table/Figure编号正确（R1, R2...为rebuttal新增的编号）

3. **整合跨reviewer的共享回复**：
   - 多个reviewer问同一个问题时，第一个详细回答，后续引用："Please refer to our response to Reviewer {X}, {W/Q编号}."
   - 确保引用关系正确

4. **生成最终rebuttal文档**：

   按OpenReview格式，每个reviewer一个section：

   ```markdown
   # Response to Reviewer {ID}

   We sincerely thank the reviewer for the constructive feedback.

   ## Response to Weakness 1: {简短标题}
   {回复内容}

   ## Response to Weakness 2: {简短标题}
   {回复内容}

   ## Response to Question 1: {简短标题}
   {回复内容}
   ```

5. **字数检查**：
   - OpenReview通常有字数限制（如5000字符/reviewer）
   - 如超限，按优先级精简：先砍P2相关回复，再压缩已有证据描述
   - 表格尽量紧凑

6. **最终检查清单**：
   - [ ] 每条W/Q都有回复
   - [ ] 所有表格数字已填充（无"--"残留）
   - [ ] 引用关系正确
   - [ ] 无语法错误
   - [ ] claim的强度与实验结果匹配
   - [ ] 承诺的修改已列出（"In the revised manuscript, we will..."）

## 输出
- `Rebuttal/rebuttal_{论文简称}.md` — 最终rebuttal文档
- 更新后的 `analysis_{论文简称}.md` — 填充了实际结果
- 向用户报告：每个reviewer的回复字数、是否有未填充的结果、建议修改的论文段落列表
