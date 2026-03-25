---
name: rebuttal-analyze
description: "Rebuttal Phase 1: Cross-compare paper with reviewer comments"
---

# Rebuttal Phase 1: Analyze — 交叉对比paper与reviews

## 输入
用户提供：
- 论文PDF路径
- 已生成的 `analysis_{论文简称}.md` 路径

## 任务

读论文，读reviews，交叉对比：哪些concern已有证据、哪些需要新实验、哪些仅需文字。

### 步骤

1. **读论文PDF**：
   - 用 `pdftotext {pdf路径} -` 通过Bash提取文本（如果Read工具不支持PDF）
   - 分段读取（每次500行左右），确保覆盖全文
   - **重点提取**：
     - 方法的核心架构和关键设计细节（损失函数、模型结构、训练策略）
     - 所有已有实验：主表、消融实验、敏感性分析、可视化
     - 每个Figure和Table的内容和结论
     - Appendix中的补充实验

2. **逐条分析每个weakness和question**：

   对analysis.md中每条W/Q，在其下方添加：

   ```markdown
   ### 📊 论文已有证据
   - {引用具体Section/Table/Figure}：{说明如何回应此concern}
   - ...
   （如无则写"无直接证据"）

   ### 🔍 分析
   - **Reviewer理解是否正确？** {是/否/部分——如reviewer误读了图表或方法细节，指出来}
   - **需要什么？** {新实验 / 补充分析 / 仅文字澄清 / 修改论文表述}
   - **工作量评估：** {小(1天内) / 中(2-3天) / 大(>3天)}
   ```

3. **标注误读**：如果reviewer对论文内容有误解（如忽略了某个已有实验、误读了图表），明确标注。这些在回复时可以直接澄清，不需要新实验。

4. **更新analysis.md**：将分析结果直接写入每条W/Q下方。

## 输出
- 更新后的 `analysis_{论文简称}.md`（每条W/Q下有已有证据+分析）
- 向用户报告：
  - 有多少条可直接用已有证据回应
  - 有多少条需要新实验
  - 有多少条reviewer存在误读
