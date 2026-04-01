# Bio-Manuscript-Forge 使用指南

## 快速开始

### 1. 安装 Skills

```bash
# 进入项目目录
cd bio-manuscript-forge

# 复制 Skills 到 Claude Code
cp -r skills/* ~/.claude/skills/

# 或者复制到 OpenClaw workspace skills
cp -r skills/* ~/.openclaw/workspace/skills/
```

### 2. 使用一条龙 Pipeline

```bash
# 启动 Claude Code
claude

# 调用 Pipeline
/bio-manuscript-pipeline "topic: 空间多组学整合 | base_work: https://github.com/QiangweiPeng/stVCR | innovation: 增加 Cross-Attention 模块处理多模态 | demo_data: https://xxx/data.h5ad | target_journal: nat-communications"
```

### 3. 分步使用

```bash
# Step 1: 创新性检测
/bio-innovation-check "空间多组学整合"

# Step 2: 任务体系构建
/bio-task-system "空间多组学整合 | paper_count: 5"

# Step 3: 数据集搜索
/bio-dataset-search "空间多组学整合 | paper_count: 5"

# Step 4: 指标体系构建
/bio-metric-system "空间多组学整合"

# Step 5: 分析方法体系
/bio-analysis-system "空间多组学整合"

# Step 6: Figure 设计
/bio-figure-design "Figure 1 | topic: 空间多组学整合"

# Step 7: 论文文案
/bio-manuscript-text "topic: 空间多组学整合"

# Step 8: 代码修改
/bio-code-modification "base_work: https://xxx | innovation: xxx"

# Step 9: Demo 验证
/bio-demo-validate "code_plan: xxx | demo_data: xxx"
```

## 输入格式详解

### 四元组输入

```python
input = {
    "topic": "研究主题",           # 如：空间多组学整合
    "base_work": "现有工作链接",    # GitHub 仓库链接
    "innovation": "用户创新点",     # 如：增加 Cross-Attention 模块
    "demo_data": "Demo数据集链接", # 可下载数据链接
    "target_journal": "目标期刊"    # 可选，默认 nat-communications
}
```

### 各字段说明

| 字段 | 必填 | 说明 | 示例 |
|------|------|------|------|
| topic | ✅ | 研究主题 | 空间多组学整合 |
| base_work | ✅ | 现有工作代码链接 | https://github.com/xxx/repo |
| innovation | ✅ | 用户创新点 | 增加 Cross-Attention 模块 |
| demo_data | ✅ | Demo 数据集链接 | https://geo/xxx/data.h5ad |
| target_journal | ❌ | 目标期刊 | nat-communications（默认） |

## 输出说明

### 输出目录结构

```
manuscript-plan/
├── 01_INNOVATION_ASSESSMENT.md    # 创新性评估
├── 02_TASK_SYSTEM.md              # 任务体系
├── 03_DATASET_CATALOG.md          # 数据集目录
├── 04_METRIC_SYSTEM.md            # 指标体系
├── 05_ANALYSIS_SYSTEM.md          # 分析方法
│
├── 06_FIGURE_DESIGNS/             # Figure 设计
│   ├── FIGURE_1_DESIGN.md
│   ├── FIGURE_2_DESIGN.md
│   ├── FIGURE_3_DESIGN.md
│   ├── FIGURE_4_DESIGN.md
│   ├── FIGURE_5_DESIGN.md
│   └── SUPPLEMENTARY_DESIGN.md
│
├── 07_MANUSCRIPT_TEXT/            # 论文文案
│   ├── INTRODUCTION.md
│   ├── RESULTS.md
│   ├── DISCUSSION.md
│   └── METHODS.md
│
├── 08_CODE_MODIFICATION_PLAN.md   # 代码修改方案
├── 09_DEMO_VALIDATION.md          # Demo 验证
├── 10_ANALYSIS_RESULTS.md         # 分析结果
│
└── FINAL_MANUSCRIPT_PLAN.md       # 整合汇总
```

### 各文件说明

| 文件 | 内容 | 用途 |
|------|------|------|
| 01_INNOVATION_ASSESSMENT.md | 创新性评估结果 | 判断是否子刊级别 |
| 02_TASK_SYSTEM.md | 任务层级设计 | 指导 Figure 2-5 |
| 03_DATASET_CATALOG.md | 数据集列表和匹配 | 实际数据准备 |
| 04_METRIC_SYSTEM.md | 评价指标体系 | Panel b 定量评价 |
| 05_ANALYSIS_SYSTEM.md | 分析方法体系 | Panel c-e 分析 |
| 06_FIGURE_DESIGNS/ | 每个 Figure 详细设计 | 绘图指导 |
| 07_MANUSCRIPT_TEXT/ | 论文完整文案 | 写作参考 |
| 08_CODE_MODIFICATION_PLAN.md | 代码修改方案 | 实现指导 |
| 09_DEMO_VALIDATION.md | Demo 验证结果 | 可行性确认 |
| 10_ANALYSIS_RESULTS.md | 实际分析结果 | 数据支撑 |

## 联动工具

### OmicsClaw

```bash
# 安装 OmicsClaw
pip install omicsclaw

# 使用 CLI
omicsclaw run spatial-preprocess --input data.h5ad
omicsclaw run spatial-domains --input processed.h5ad
```

### Bioclaw Skills Hub

参考：
- `single-cell-and-spatial/spatial-domains/SKILL.md`
- `single-cell-and-spatial/spatial-annotate/SKILL.md`

### Gemini 图文生成

```python
# 设置 API Key
export GEMINI_API_KEY="your-api-key"

# 运行生成脚本
python scripts/generate_figure_image.py
```

## 支持的期刊

| 期刊 | Main Text 字数 | Figure 数量 | 模板路径 |
|------|---------------|------------|---------|
| Nature Methods | ~2500 | 5-6 | templates/nature-methods/ |
| Nat Commun | ~4000 | 5-6 | templates/nat-communications/ |
| Genome Biology | ~3000 | 4-5 | templates/genome-biology/ |

## 常见问题

### Q: Pipeline 需要多久？

A: 取决于输入复杂度：
- Phase 1（Steps 1-5）：约 10-30 分钟
- Phase 2（Steps 6-7）：约 20-40 分钟
- Phase 3（Steps 8-9）：取决于 Demo 运行时间

### Q: 如何处理 Demo 验证失败？

A: 
1. 查看 09_DEMO_VALIDATION.md 中的修改建议
2. 应用建议后重新运行 Step 9
3. 如果多次失败，考虑调整创新点

### Q: 可以只生成部分内容吗？

A: 可以。使用分步调用，只运行需要的 Step。

### Q: 如何自定义期刊模板？

A: 在 `templates/` 目录下创建新的期刊文件夹，参考现有模板格式。

## 进阶使用

### 自定义同义变换

```python
# 修改 scripts/topic_synonym_transform.py
DOMAIN_TERMS = {
    "你的领域术语": ["term1", "term2", "term3"],
    ...
}
```

### 添加新的 Skill

```bash
# 在 skills/ 目录下创建新文件夹
mkdir skills/bio-custom-skill

# 创建 SKILL.md
# 参考现有 skill 格式
```

### 集成其他分析工具

```python
# 修改 scripts/integrate_omicsclaw.py
# 添加新的分析函数

def custom_analysis(self, input_file: str) -> Dict:
    return self.run_skill(
        skill_name="custom-skill",
        input_file=input_file
    )
```

## 注意事项

1. **API 调用限制**：文献搜索、图文生成可能需要 API Key
2. **代码验证**：Demo 验证需要实际运行环境（GPU 可能需要）
3. **数据获取**：部分数据集可能需要申请权限
4. **迭代优化**：Pipeline 输出可以迭代修改

## 获取帮助

- GitHub Issues: https://github.com/your-repo/bio-manuscript-forge/issues
- 文档: https://your-docs-site
- 示例: examples/