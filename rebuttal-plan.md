---
name: rebuttal-plan
description: "Rebuttal Phase 2: Generate prioritized experiment plan and response templates"
---

# Rebuttal Phase 2: Plan — 生成实验计划和回复模板

## 输入
用户提供已经过Phase 1分析的 `analysis_{论文简称}.md`。

## 任务

基于分析结果，生成带优先级的实验计划和回复模板。

### 步骤

1. **优先级排序**：

   根据以下规则确定每个实验的优先级：

   | 优先级 | 条件 |
   |--------|------|
   | P0 必做 | 低分reviewer(≤4/10或≤2/5)的核心weakness；多个reviewer共同提出的concern |
   | P1 强烈建议 | 中等分reviewer的主要weakness；能同时回应多条concern的实验 |
   | P2 有时间再做 | 高分reviewer的建议；单一reviewer的minor concern |
   | 仅文字 | reviewer误读可直接澄清的；表述修改；typo修正 |

   **加权因素**：
   - 同一concern被多个reviewer提出 → 优先级上调
   - 低分reviewer的concern → 优先级更高（争取翻盘）
   - 高分reviewer已明确positive的点 → 仅需简单感谢

2. **生成 exp_plan.md**：

   ```markdown
   # {论文名} Rebuttal Experiment Plan

   ## TODO LIST

   ### P0 (必做)
   - [ ] {实验名} — {一句话描述}（回应{Reviewer}-{W/Q编号}）⚡{已有证据标注}

   ### P1 (强烈建议)
   ...

   ### P2 (有时间再做)
   ...

   ### 仅文字修改
   ...

   ---
   （以下为每个实验的详细规划）
   ```

3. **每个需要实验的W/Q，添加**：

   ```markdown
   ### 📋 实验规划

   **已有证据（论文中）：**
   - {从Phase 1复制}

   **需补充实验：**
   - 目的：{一句话}
   - 实验设计：{具体配置}
   - 预期工作量：{评估}

   **结果表格模板：**
   | ... |

   ### 📝 回复模板

   > {面向reviewer的英文回复草稿}
   > - 先感谢reviewer
   > - 引用已有证据
   > - 展示新实验结果（留空待填）
   > - 如有claim需降调，给出修改前后对比
   ```

4. **识别共享实验**：标注哪些实验可以同时回应多个reviewer的concern，避免重复工作。

5. **将实验规划和回复模板写入analysis.md的每条W/Q下方**。

## 输出
- `Rebuttal/exp_plan_{论文简称}.md` — 带优先级的TODO list + 每个实验的详细规划
- 更新后的 `analysis_{论文简称}.md` — 每条W/Q下有完整的实验规划+回复模板
- 向用户报告：P0/P1/P2/文字的数量分布，预估总工作量
