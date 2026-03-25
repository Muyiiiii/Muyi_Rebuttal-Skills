---
name: rebuttal-code
description: "Rebuttal Phase 3: Analyze codebase and generate experiment scripts"
---

# Rebuttal Phase 3: Code — 分析代码仓库，生成实验脚本

## 输入
用户提供：
- `exp_plan_{论文简称}.md` 路径
- 代码仓库路径（或就是当前工作目录）
- 用户可能指定只做某些优先级的实验（如"先做P0"）

## 任务

理解现有代码结构，为每个需要的实验生成可运行的代码修改和脚本。

### 步骤

1. **理解代码仓库结构**：
   - 找到训练入口（train.py / main.py / run.py 等）
   - 理解配置系统（argparse / yaml config / hydra 等）
   - 理解数据pipeline（数据集路径、预处理、dataloader）
   - 理解模型定义位置
   - 理解现有实验脚本的组织方式（scripts/ 目录等）
   - 找到已有的baseline实现

2. **逐个实验分析实现方案**：

   对exp_plan中每个需要代码的实验：

   ```markdown
   ## 实验：{实验名}

   ### 需要修改的文件
   - `{文件路径}`: {修改内容概述}

   ### 实现方案
   {具体说明：改哪些代码、加什么参数、新建什么文件}

   ### 运行命令
   ```bash
   {具体的命令}
   ```

   ### 预期输出
   {结果会保存在哪、格式是什么}
   ```

3. **实现代码修改**：
   - **优先复用现有代码**：尽量通过添加参数/配置来支持新实验，而非重写
   - **新增代码放在独立文件/函数中**：避免污染主代码
   - 如果需要新的loss function、evaluation metric等，写在独立模块中
   - **不要修改已有实验的默认行为**

4. **生成实验脚本**：

   为每个实验生成独立的运行脚本，放在 `Rebuttal/scripts/` 目录：

   ```
   Rebuttal/scripts/
   ├── run_p0_01_{实验名}.sh
   ├── run_p0_02_{实验名}.sh
   ├── run_p1_01_{实验名}.sh
   ├── ...
   └── run_all.sh          # 按优先级顺序串联
   ```

   每个脚本应包含：
   - 注释说明对应哪个reviewer的哪个concern
   - 完整的运行命令（不依赖外部变量）
   - 结果保存路径
   - GPU/内存需求估计（如可评估）

5. **生成结果收集脚本**：
   - 从各实验输出中提取关键metrics
   - 格式化为exp_plan中的表格格式
   - 方便直接粘贴到rebuttal回复中

### 注意事项
- **先理解再动手**：必须先读懂现有代码再写新代码，不要猜测
- **保持最小修改**：能用配置解决的不改代码，能改一处的不改多处
- **不直接执行长时间训练**：只生成脚本，由用户决定何时运行
- **处理依赖**：如果需要新的package，在脚本开头注明

## 输出
- 代码修改（通过Edit工具直接应用）
- `Rebuttal/scripts/` 下的实验运行脚本
- `Rebuttal/scripts/run_all.sh` 主运行脚本
- `Rebuttal/scripts/collect_results.py` 结果收集脚本（如适用）
- 向用户报告：修改了哪些文件、生成了哪些脚本、运行顺序建议
