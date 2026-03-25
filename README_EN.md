# Muyi Rebuttal Skills

[中文说明](./README.md)

A staged collection of prompts/skill documents for paper rebuttal workflows. The goal is to turn the OpenReview rebuttal process into a clear and reusable 5-phase pipeline:

1. Parse reviewer comments
2. Cross-check the paper against the reviews
3. Build an experiment plan and response templates
4. Generate experiment scripts from the codebase
5. Fill in results and produce the final rebuttal

This repository is closer to a "workflow spec + AI skill guide" than a one-click automation tool. It is intended for agent environments such as Codex, Claude, ChatGPT, or other tools that can follow long-form instructions.

## Use Cases

- ICLR / ICML / NeurIPS / ACL and other venues using OpenReview or similar review systems
- You already have reviewer comments and need to quickly organize a rebuttal worklist
- You want a stable workflow for reading reviews, locating evidence, planning experiments, editing code, and writing responses

## Repository Structure

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

## Phase Overview

| Phase | File | Purpose | Typical Input | Typical Output |
|------|------|---------|---------------|----------------|
| 0 | `rebuttal-parse.md` | Parse OpenReview HTML and structure reviewer information | OpenReview HTML files | `Rebuttal/analysis_{paper_short_name}.md` |
| 1 | `rebuttal-analyze.md` | Check whether reviewer concerns are already supported by the paper | Paper PDF + `analysis_*.md` | Updated `analysis_*.md` |
| 2 | `rebuttal-plan.md` | Prioritize concerns and create experiment plans plus reply templates | Analyzed `analysis_*.md` | `Rebuttal/exp_plan_{paper_short_name}.md` |
| 3 | `rebuttal-code.md` | Understand the codebase and generate experiment scripts | `exp_plan_*.md` + code repository | Scripts and code changes under `Rebuttal/scripts/` |
| 4 | `rebuttal-write.md` | Fill in experiment results and assemble the final rebuttal | Result files + templated `analysis_*.md` | `Rebuttal/rebuttal_{paper_short_name}.md` |

## Recommended Order

Use the phases in order. Do not jump straight to writing the rebuttal.

### Phase 0: Parse

First convert reviewer comments into a consistent structure so later analysis does not depend on scattered webpage content.

Main deliverables:
- A reviewer summary table
- Structured strengths / weaknesses / questions
- `==highlighted==` reviewer requests that need direct attention

### Phase 1: Analyze

The goal here is not to answer reviewers immediately, but to decide:

- Which concerns are already addressed by existing evidence in the paper
- Which concerns come from reviewer misunderstanding
- Which concerns truly require new experiments
- Which concerns only need clarification or wording changes

### Phase 2: Plan

Prioritize what to do next, especially:

- Core weaknesses from low-score reviewers
- Concerns raised by multiple reviewers
- Experiments that can answer several concerns at once

### Phase 3: Code

Once you enter the codebase, the target is minimal runnable changes:

- Reuse the existing training entry points and config system whenever possible
- Prefer adding parameters over rewriting the main pipeline
- Do not launch long-running training by default; generate scripts and result collection logic first

### Phase 4: Write

Only after you have real results should you fill the templates and assemble the final rebuttal.

Key principles:
- Do not hide negative results
- If results are weaker than expected, tone down the claim
- Merge repeated answers across reviewers when possible

## Expected Outputs

The workflow assumes intermediate artifacts are organized under a `Rebuttal/` directory:

```text
Rebuttal/
├── analysis_{paper_short_name}.md
├── exp_plan_{paper_short_name}.md
├── rebuttal_{paper_short_name}.md
└── scripts/
    ├── run_p0_01_xxx.sh
    ├── run_p1_01_xxx.sh
    ├── run_all.sh
    └── collect_results.py
```

## How To Use

### Option 1: Use as AI Agent Skill Documents

Provide the relevant `.md` file to an agent and execute the workflow phase by phase.

Example:

```text
Follow rebuttal-parse.md to parse this OpenReview HTML and generate analysis_xxx.md.
```

```text
Follow rebuttal-analyze.md to read the paper PDF and analysis_xxx.md, then add existing evidence and analysis under each weakness/question.
```

### Option 2: Use as a Manual Workflow Template

Even if you do not directly feed these documents to an agent, they still work well as checklists. Each file already defines:

- Required inputs
- Processing steps
- Output format
- What should be summarized back to the user

## Design Goals

This repository is built around the parts of rebuttal work that usually become messy:

- Do not start by drafting responses; verify evidence first
- Do not treat every experiment as equally important
- Do not modify training logic before understanding the codebase
- Do not mix result filling with final writing

## Current Scope

The repository currently contains workflow definitions only. It does not yet include:

- An automatic OpenReview HTML parser
- A PDF extraction utility
- An experiment script generator
- A one-command result aggregation CLI

If you want to extend it, the next useful additions are:

- `templates/`: templates for analysis / experiment plan / rebuttal
- `scripts/`: parsers and result aggregation utilities
- `examples/`: real or anonymized example inputs and outputs

## File Summary

- `rebuttal-parse.md`: structure reviewer comments
- `rebuttal-analyze.md`: cross-check the paper and reviews
- `rebuttal-plan.md`: create experiment priorities and reply templates
- `rebuttal-code.md`: generate code changes and experiment scripts
- `rebuttal-write.md`: assemble the final rebuttal document

If you want to turn this into a real installable skill package, the next highest-value step is to add examples, templates, and automation scripts instead of making the prompts longer.
