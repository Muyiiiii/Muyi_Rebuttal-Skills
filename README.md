# Muyi Rebuttal Skills

[English Version](./README_EN.md)

一套面向论文 rebuttal 场景的分阶段提示词/技能文档，目标是把 OpenReview review 处理流程拆成清晰、可复用的 5 个 Phase：

1. 解析 reviewer comments
2. 交叉对照论文与 review
3. 产出实验优先级与回复模板
4. 基于代码仓库生成实验脚本
5. 回填实验结果并生成最终 rebuttal

这套仓库更像“工作流规范 + AI 技能说明”，不是一键运行的自动化程序。适合在 Codex、Claude、ChatGPT 或其他支持长指令的 agent 环境中使用。

## 适用场景

- ICLR / ICML / NeurIPS / ACL 等采用 OpenReview 或类似评审系统的会议
- 已拿到 reviewer comments，需要快速整理出 rebuttal 工作清单
- 希望把“读 review、找证据、排实验、改代码、写回复”拆成稳定流程

## 仓库结构

```text
.
├── rebuttal-parse.md
├── rebuttal-analyze.md
├── rebuttal-plan.md
├── rebuttal-code.md
├── rebuttal-write.md
├── README.md
└── README_EN.md
```

## Phase 说明

| Phase | 文件 | 作用 | 典型输入 | 典型输出 |
|------|------|------|----------|----------|
| 0 | `rebuttal-parse.md` | 解析 OpenReview HTML，抽取 reviewer 信息并结构化 | OpenReview HTML 文件 | `Rebuttal/analysis_{论文简称}.md` |
| 1 | `rebuttal-analyze.md` | 对照论文内容，判断 concern 是否已有证据 | 论文 PDF + `analysis_*.md` | 更新后的 `analysis_*.md` |
| 2 | `rebuttal-plan.md` | 给 concern 排优先级，制定实验计划和回复模板 | 已分析的 `analysis_*.md` | `Rebuttal/exp_plan_{论文简称}.md` |
| 3 | `rebuttal-code.md` | 理解代码仓库并生成实验脚本 | `exp_plan_*.md` + 代码仓库 | `Rebuttal/scripts/` 下的脚本与代码修改 |
| 4 | `rebuttal-write.md` | 回填实验结果并生成最终 rebuttal 文稿 | 结果文件 + 已含模板的 `analysis_*.md` | `Rebuttal/rebuttal_{论文简称}.md` |

## 推荐使用顺序

建议严格按顺序走，不要一开始就直接写 rebuttal。

### Phase 0: Parse

先把 reviewer comments 变成统一格式，避免后面分析时信息散落在网页里。

产物重点：
- reviewer 总览表
- strengths / weaknesses / questions 分条整理
- 用 `==高亮==` 标出 reviewer 的核心要求

### Phase 1: Analyze

这一步的目标不是“立刻答复 reviewer”，而是先判断：

- 哪些 concern 已经被论文现有实验覆盖
- 哪些只是 reviewer 误读
- 哪些确实需要新实验
- 哪些只需要改表述或补讨论

### Phase 2: Plan

把需要处理的点排出优先级，尤其优先看：

- 低分 reviewer 的核心 weakness
- 多个 reviewer 共同提到的问题
- 一个实验能同时回应多条 concern 的情况

### Phase 3: Code

进入代码仓库后，目标是生成“最小可运行修改”：

- 尽量复用现有训练入口和配置系统
- 尽量通过加参数支持新实验，而不是重写主流程
- 不直接长时间跑训练，优先产出脚本和结果收集逻辑

### Phase 4: Write

最后再把真实实验结果写回模板，生成最终 rebuttal 文稿。

关键原则：
- 不隐藏负面结果
- 结果不理想时，主动降调 claim
- 多 reviewer 的重复问题尽量合并回答

## 产物约定

这套流程默认会围绕 `Rebuttal/` 目录组织中间结果：

```text
Rebuttal/
├── analysis_{论文简称}.md
├── exp_plan_{论文简称}.md
├── rebuttal_{论文简称}.md
└── scripts/
    ├── run_p0_01_xxx.sh
    ├── run_p1_01_xxx.sh
    ├── run_all.sh
    └── collect_results.py
```

## 如何使用

### 方式一：作为 AI agent 的技能文档

把对应 `.md` 文件提供给 agent，按 Phase 逐步执行。

示例：

```text
请按照 rebuttal-parse.md 的规范，解析这个 OpenReview HTML，并生成 analysis_xxx.md
```

```text
请按照 rebuttal-analyze.md 的规范，读取论文 PDF 和 analysis_xxx.md，补充每条 weakness/question 的已有证据与分析
```

### 方式二：作为人工流程模板

即使不直接喂给 agent，也可以把这些文档当 checklist 使用。每个文件都已经定义了：

- 输入要求
- 处理步骤
- 输出文件格式
- 向用户汇报时应总结的信息

## 设计目标

这个仓库强调的是 rebuttal 期间最容易失控的几件事：

- 不要一上来就写回复，先做证据核对
- 不要把所有实验都当成同等优先级
- 不要在没读懂代码前直接改训练逻辑
- 不要把结果回填和最终文案写作混在一起

## 当前边界

目前仓库只包含流程定义文档，不包含：

- OpenReview HTML 的自动解析脚本
- PDF 信息抽取程序
- 实验脚本生成器
- 一键汇总结果的 CLI 工具

如果后续需要，可以在这个基础上继续补：

- `templates/`：analysis / exp_plan / rebuttal 模板
- `scripts/`：自动解析与结果汇总脚本
- `examples/`：真实或匿名化的样例输入输出

## 文件一览

- `rebuttal-parse.md`: reviewer comments 结构化
- `rebuttal-analyze.md`: 论文与 review 交叉分析
- `rebuttal-plan.md`: 实验优先级与回复模板
- `rebuttal-code.md`: 代码改动与实验脚本生成
- `rebuttal-write.md`: 最终 rebuttal 文稿整合

如果你想把它做成真正可安装的技能包，下一步最值得补的是样例、模板和自动化脚本，而不是继续堆更长的提示词。
