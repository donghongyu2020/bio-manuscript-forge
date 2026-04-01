# bio-manuscript-pipeline

**一条龙 Pipeline：从结构化输入到完整 Manuscript Plan**

---

## 🚀 启动欢迎语

欢迎使用 Bio-Manuscript-Forge！我将帮助你从研究想法生成完整的 Manuscript Plan。

### 📋 输入模板

请按以下格式填写你的研究内容：

```
topic: [研究主题]
base_work: 
  - 论文: [相关论文链接]
  - 代码: [相关代码仓库链接]

innovation: [一句话概括你的创新点]
- 算法创新性: [核心算法/方法创新点]
- 任务: [任务1, 任务2, 任务3, ...]
- 数据: [数据集来源或类型]
- benchmark: [评估基准]
- 计算指标: [指标1, 指标2, ...]
- 生物学分析手段: [如何体现生物学意义]

demo_data: [Demo数据集链接]
target_journal: [目标期刊，可选，默认 nat-communications]
num_refine_rounds: [Refine轮数，可选，默认 2]
```

### ✅ 成功案例参考

```
topic: 组学大模型安全问题 - 防御视角（单细胞 + 空间组学）
base_work: 
  - 论文: https://www.nature.com/articles/s41421-024-00753-1
  - 代码: https://github.com/BioX-NKU/scBackdoor
  - 防御方法: https://github.com/XuankunRong/BYE (NeurIPS'25)

innovation: 从攻击视角转向防御视角，将 BYE 方法迁移到组学大模型
- 算法创新性: 稀疏感知熵计算 + 层级熵分析 + 基因集熵分析，适配组学数据稀疏性
- 任务: Cell annotation, Perturbation prediction, GRN inference, Spatial domain detection, Multi-omics integration
- 数据: scBackdoor benchmark (Pancreas, PBMC, Brain), Norman perturbation, BEELINE GRN, Stereo-seq spatial
- benchmark: scBackdoor 攻击基准 + BEELINE GRN 基准 + Stereo-seq 空间数据
- 计算指标: 安全指标(ASR, Defense Rate, Detection AUC) + 任务指标(Accuracy, Pearson, F1, ARI, cLISI)
- 生物学分析手段: 
  - Marker gene attention recovery (INS, PDX1, MAFA)
  - Perturbation direction consistency (PDX1 KO → INS↓)
  - TF-target edge recovery (PDX1→INS)
  - Spatial domain preservation (CA1/CA3 boundaries)
  - RNA-ATAC coherence (CD4 expr ↔ promoter)

demo_data: https://figshare.com/articles/dataset/Tabula_Sapiens_release_1_0/14267219
target_journal: nat-communications (冲刺 nat-methods)
num_refine_rounds: 2
```

### 📤 你将获得的输出

| 文件 | 内容 |
|------|------|
| **PPT** | 26页组会汇报 |
| **FINAL_PROPOSAL** | 完整研究方案 |
| **Figure 2-7 (v3)** | 每个任务的详细设计 + 安全生物学分析 |
| **论文文案 (v2)** | Introduction, Results, Discussion, Methods |

---

**请填写你的研究内容，我将开始执行 Pipeline！**

---

## 功能

完整执行 Pipeline，生成期刊投稿级的 Manuscript Plan，并通过三审稿人迭代优化。

## 输入格式

```
topic: [研究主题]
base_work: [现有工作论文+代码链接]
innovation: [用户创新点，概括版]
- 算法创新性: [核心算法/方法创新点描述]
- 任务: [覆盖的下游任务，多个用逗号分隔]
- 数据: [数据集来源或类型描述]
- benchmark: [基准数据集或评估基准]
- 计算指标: [安全指标 + 任务指标，如 ASR, ARI 等]
- 生物学分析手段: [如何体现生物学意义，如 marker gene, pathway 等]

demo_data: [Demo数据集链接]
target_journal: [目标期刊，可选，默认 nat-communications]
num_refine_rounds: [Refine轮数，可选，默认 2]
```

### 📋 输入示例（成功案例）

```
topic: 组学大模型安全问题 - 防御视角（单细胞 + 空间组学）
base_work: 
  - 论文: https://www.nature.com/articles/s41421-024-00753-1
  - 代码: https://github.com/BioX-NKU/scBackdoor
  - 防御方法: https://github.com/XuankunRong/BYE (NeurIPS'25)

innovation: 从攻击视角转向防御视角，将 BYE 方法迁移到组学大模型
- 算法创新性: 稀疏感知熵计算 + 层级熵分析 + 基因集熵分析，适配组学数据稀疏性
- 任务: Cell annotation, Perturbation prediction, GRN inference, Spatial domain detection, Multi-omics integration
- 数据: scBackdoor benchmark (Pancreas, PBMC, Brain), Norman perturbation, BEELINE GRN, Stereo-seq spatial
- benchmark: scBackdoor 攻击基准 + BEELINE GRN 基准 + Stereo-seq 空间数据
- 计算指标: 安全指标(ASR, Defense Rate, Detection AUC) + 任务指标(Accuracy, Pearson, F1, ARI, cLISI)
- 生物学分析手段: 
  - Marker gene attention recovery (INS, PDX1, MAFA)
  - Perturbation direction consistency (PDX1 KO → INS↓)
  - TF-target edge recovery (PDX1→INS)
  - Spatial domain preservation (CA1/CA3 boundaries)
  - RNA-ATAC coherence (CD4 expr ↔ promoter)

demo_data: https://figshare.com/articles/dataset/Tabula_Sapiens_release_1_0/14267219
target_journal: nat-communications (冲刺 nat-methods)
num_refine_rounds: 2
```

### 输入字段说明

| 字段 | 必填 | 说明 |
|------|------|------|
| topic | ✅ | 研究主题，简洁明确 |
| base_work | ✅ | 论文 + 代码链接，多个用换行或逗号分隔 |
| innovation | ✅ | 创新点概括 + 结构化子项 |
| demo_data | ✅ | Demo 数据集链接 |
| target_journal | ❌ | 默认 nat-communications |
| num_refine_rounds | ❌ | 默认 2 |

### innovation 子项说明

| 子项 | 说明 | 示例 |
|------|------|------|
| 算法创新性 | 核心方法/算法创新点 | 注意力熵分析、损失函数改进、架构创新 |
| 任务 | 覆盖的下游任务 | cell annotation, perturbation, GRN inference |
| 数据 | 数据集来源/类型 | 公开数据集、用户数据、特定组织 |
| benchmark | 评估基准 | 已有基准、自建基准 |
| 计算指标 | 安全指标 + 任务指标 | ASR, Accuracy, F1, ARI, Pearson |
| 生物学分析手段 | 如何体现生物学意义 | marker gene, pathway, regulatory relationship |

## 执行流程

### Phase 1: 系统构建（Steps 1-5）

**输入解析**：首先从用户输入中提取关键信息：
- `topic` → 用于创新性搜索
- `base_work` → 提取已有工作数据集、指标、方法
- `innovation.算法创新性` → 创新性评估依据
- `innovation.任务` → 任务体系构建基础
- `innovation.数据` → 数据集搜索方向
- `innovation.计算指标` → 指标体系构建基础
- `innovation.生物学分析手段` → 分析方法体系基础

```
Step 1: 创新性检测
├─ 解析输入：topic, base_work, innovation.算法创新性
├─ 调用 searxng/web_search 搜索
├─ Topic 同义变换生成 10-20 个变体
├─ 搜索 PubMed + bioRxiv + arXiv q-bio
├─ 统计相似文章数量
├─ 结合 innovation.算法创新性 判断创新性级别
└─ 输出：01_INNOVATION_ASSESSMENT.md

Step 2: 任务体系构建
├─ 解析输入：innovation.任务
├─ 若用户提供任务列表 → 直接使用
├─ 若未提供 → 搜索领域主要任务分类
├─ 识别任务层级（Level 1-4）
├─ 确保难度递进
└─ 输出：02_TASK_SYSTEM.md

Step 3: 数据集搜索
├─ 解析输入：innovation.数据, innovation.benchmark, demo_data
├─ 若用户提供数据描述 → 搜索匹配数据集
├─ 从 base_work 论文提取数据集
├─ 数据集与任务匹配
└─ 输出：03_DATASET_CATALOG.md

Step 4: 指标体系构建
├─ 解析输入：innovation.计算指标
├─ 若用户提供指标 → 直接使用并补充
├─ 若未提供 → 从已有工作提取指标
├─ 分类：安全指标 + 任务指标
└─ 输出：04_METRIC_SYSTEM.md

Step 5: 分析方法体系
├─ 解析输入：innovation.生物学分析手段
├─ 若用户提供分析手段 → 直接使用并补充
├─ 若未提供 → 从已有工作提取分析方法
├─ 标注 OmicsClaw/Bioclaw skill
├─ 说明为什么用、证明什么、体现什么生物学意义
└─ 输出：05_ANALYSIS_SYSTEM.md
```

### Phase 2: 设计与文案（Steps 6-7）

**⚠️ 核心原则**：
1. **任务为先**：Figure 2-N 每个对应一个任务，数据/指标/分析随任务而定
2. **分析增强**：每个 Figure 必须包含安全 + 生物学分析
3. **文案同步**：Figure 改完立即更新 Results

```
Step 6: Figure 设计
│
├─ Figure 1：算法创新性（方法框架）
│   ├─ Panel a：方法 Overview
│   ├─ Panel b：创新点示意
│   ├─ Panel c：模型覆盖
│   ├─ Panel d：任务覆盖
│   └─ Panel e：指标体系
│
├─ Figure 2-N：每个 Figure = 一个任务 ⭐ 任务为先原则
│   │
│   ├─ Panel a: 任务 Overview（数据流）
│   │
│   ├─ Panel b-d: 定量测评
│   │   ├─ 多模型对比
│   │   ├─ ASR 降低
│   │   └─ 任务指标保持
│   │
│   ├─ Panel e: 安全分析 ⭐ 必须包含
│   │   ├─ 注意力模式变化
│   │   ├─ 熵分析
│   │   └─ 触发基因识别
│   │
│   ├─ Panel f: 生物学分析 ⭐ 必须包含
│   │   ├─ Marker gene recovery
│   │   ├─ Pathway preservation
│   │   └─ 具体生物学意义
│   │
│   ├─ Panel g: 深入案例 ⭐ 1-2 个案例
│   │   ├─ 具体生物学问题
│   │   ├─ 攻击前后对比
│   │   └─ 防御后恢复
│   │
│   └─ 数据/指标/分析依据任务选取
│
├─ Figure N+1: Summary + 生物学意义总结
│
└─ 输出：06_FIGURE_DESIGNS/

Step 6.5: 文案同步检查 ⭐ 必须
├─ Figure 有这个 Panel → Results 有对应段落？
├─ Figure 有这个案例 → Results 有详细展开？
└─ 检查通过才能进入下一步

Step 7: 论文文案生成
│
├─ Introduction（5段）
│   ├─ 第一段：领域介绍
│   ├─ 第二段：相关工作调研
│   ├─ 第三段：现有方法不足
│   ├─ 第四段：本文方法介绍
│   └─ 第五段：意义与应用
│
├─ Results（与 Figure 对应）⭐ 结构对齐
│   ├─ 2.1 Overview（对应 Figure 1）
│   ├─ 2.2 Task 1 Defense（对应 Figure 2）
│   │   ├─ 定量测评
│   │   ├─ 安全分析
│   │   ├─ 生物学分析
│   │   └─ 案例研究
│   ├─ 2.3 Task 2 Defense（对应 Figure 3）
│   ├─ ...每个任务一个 section
│   └─ 2.N Summary（对应最后一个 Figure）
│
├─ Discussion
│   ├─ 方法优势总结
│   ├─ 安全-生物学结合意义 ⭐
│   ├─ 与现有方法对比
│   ├─ 方法局限性
│   └─ 未来方向
│
├─ Methods
│   ├─ 数据预处理
│   ├─ 模型架构
│   ├─ 任务特定方法 ⭐ 按任务组织
│   ├─ 生物学分析方法 ⭐
│   ├─ 统计分析
│   └─ 代码与数据可用性
│
└─ 输出：07_MANUSCRIPT_TEXT/
```

**Figure 设计检查清单**：

```
- [ ] Figure 1 是方法框架？
- [ ] Figure 2-N 每个对应一个任务？
- [ ] 每个 Figure 包含多模型对比？
- [ ] 每个 Figure 有定量测评 Panel？
- [ ] 每个 Figure 有安全分析 Panel？ ⭐
- [ ] 每个 Figure 有生物学分析 Panel？ ⭐
- [ ] 每个 Figure 有 1-2 个深入案例？ ⭐
- [ ] 分析手段多样化？
- [ ] Results 结构与 Figure 对应？ ⭐
```

### Phase 2.5: Refine Loop ⭐

```
Step 7.5: 三审稿人迭代优化
│
├─ Round 0: 保存初始方案
│   └─ 输出：refine-logs/round-0-initial-proposal.md
│
├─ Round 1 Review:
│   ├─ Editor Review（创新性评估，Nature子刊标准）
│   │   ├─ 概念创新 / 方法创新 / 应用创新
│   │   └─ 评分：创新性 / 可行性 / 推荐度
│   │
│   ├─ 计算审稿人 Review（算法/方法评审）
│   │   ├─ 算法设计合理性 / 方法创新性
│   │   ├─ 实验设计严谨性（Baseline/指标/Ablation）
│   │   └─ 评分：方法创新 / 技术严谨 / 代码可行
│   │
│   ├─ 生物分析审稿人 Review（生物学意义评审）
│   │   ├─ 生物学意义 / 分析设计合理性
│   │   ├─ 数据集选择合理性
│   │   └─ 评分：生物意义 / 分析设计 / 数据选择
│   │
│   └─ 输出：refine-logs/round-1/
│
├─ Round 1 Refinement:
│   ├─ 汇总三审稿人意见
│   ├─ 问题分类（Critical/Major/Minor）
│   ├─ 逐条响应和修改
│   ├─ 更新 Proposal
│   └─ 输出：refine-logs/round-1/refinement.md
│
├─ Round 2 Review:（同 Round 1）
│   └─ 输出：refine-logs/round-2/
│
├─ Round 2 Refinement:
│   └─ 输出：refine-logs/round-2/refinement.md
│
└─ 最终输出：
    ├─ refine-logs/REVIEW_SUMMARY.md（每轮汇总）
    ├─ refine-logs/FINAL_PROPOSAL.md（最终方案）
    ├─ refine-logs/score-history.md（评分历史）
    └─ refine-logs/REFINEMENT_REPORT.md（完整报告）
```

### Phase 2.6: 人类反馈验证 ⭐ NEW

```
Step 7.6: 人类反馈循环
│
├─ 呈现 Proposal
│   ├─ 展示 FINAL_PROPOSAL.md 核心内容
│   ├─ 包含：创新点、Figure 设计、实验方案、关键修改
│   └─ 格式：结构化摘要 + 关键决策点
│
├─ 等待人类反馈
│   ├─ 选项 A: 同意 → 继续 Phase 3
│   └─ 选项 B: 有意见 → 收集反馈内容
│
├─ 反馈处理
│   ├─ 如果同意 → 记录并进入 Phase 3
│   └─ 如果不同意 → 
│       ├─ 记录反馈意见到 refine-logs/human-feedback/
│       ├─ 根据反馈类型决定返回点：
│       │   ├─ Phase 1 级问题：创新性/任务体系需重构
│       │   ├─ Phase 2 级问题：Figure/文案需调整
│       │   └─ Phase 2.5 级问题：细节优化
│       ├─ 执行迭代修改
│       ├─ 重新运行 Phase 2.5 Refine Loop
│       └─ 再次呈现给人类验证
│
└─ 输出：
    ├─ refine-logs/human-feedback/feedback-round-X.md
    └─ refine-logs/HUMAN_APPROVAL.md（最终批准记录）
```

**人类反馈处理流程：**

```
人类反馈 → 问题分类 → 返回点决策
│
├─ Critical 问题（创新性方向错误）
│   └─ 返回 Phase 1 → 重新评估创新点
│
├─ Major 问题（设计/方案需要大改）
│   └─ 返回 Phase 2 → 调整 Figure/文案
│
├─ Minor 问题（细节优化）
│   └─ 返回 Phase 2.5 → Refine Loop
│
└─ 批准
    └─ 进入 Phase 3
```

**反馈收集格式：**

```markdown
## 人类反馈 Round X

**反馈时间**: YYYY-MM-DD HH:MM
**反馈内容**: [用户意见]
**问题级别**: Critical / Major / Minor
**返回阶段**: Phase 1 / Phase 2 / Phase 2.5
**修改建议**: [AI 分析后的修改方案]

---

## 修改执行记录

- [ ] 修改项 1
- [ ] 修改项 2
...
```

### Phase 3: 验证与汇报（Steps 8-11）

```
Step 8: 代码修改方案
├─ 克隆原有代码仓库
├─ 分析代码结构
├─ 映射创新点到修改位置
├─ 设计新增文件 + 修改文件
└─ 输出：08_CODE_MODIFICATION_PLAN.md

Step 9: Demo 快速验证
├─ 应用代码修改
├─ 下载 Demo 数据
├─ Subsample + 少 epoch 快速运行
├─ 可行性判断
├─ 如果不可行 → 修改建议
└─ 输出：09_DEMO_VALIDATION.md

Step 10: 详细分析执行（可选）
├─ 调用 OmicsClaw/Bioclaw
├─ 运行完整分析
├─ 生成实际数据
└─ 输出：10_ANALYSIS_RESULTS.md

Step 11: 生成组会汇报 PPT（⭐ 新增）
├─ 从 FINAL_PROPOSAL.md 提取核心内容
├─ 从 DEMO_VALIDATION.md 提取 Demo 结果
├─ 生成 12-15 页组会汇报 PPT
├─ 格式：Markdown (Marp) / HTML (reveal.js) / PPTX
└─ 输出：11_PPT_PRESENTATION.md
```

## 输出目录结构

```
manuscript-plan/
├── 01_INNOVATION_ASSESSMENT.md
├── 02_TASK_SYSTEM.md
├── 03_DATASET_CATALOG.md
├── 04_METRIC_SYSTEM.md
├── 05_ANALYSIS_SYSTEM.md
│
├── 06_FIGURE_DESIGNS/
│   ├── FIGURE_1_DESIGN.md
│   ├── FIGURE_2_DESIGN.md
│   ├── FIGURE_3_DESIGN.md
│   ├── FIGURE_4_DESIGN.md
│   ├── FIGURE_5_DESIGN.md
│   └── SUPPLEMENTARY_DESIGN.md
│
├── 07_MANUSCRIPT_TEXT/
│   ├── INTRODUCTION.md
│   ├── RESULTS.md
│   ├── DISCUSSION.md
│   └── METHODS.md
│
├── refine-logs/                    # ⭐ 新增
│   ├── round-0-initial-proposal.md
│   │
│   ├── round-1/
│   │   ├── editor-review.md
│   │   ├── computational-review.md
│   │   ├── biological-review.md
│   │   ├── review-summary.md
│   │   └── refinement.md
│   │
│   ├── round-2/
│   │   ├── editor-review.md
│   │   ├── computational-review.md
│   │   ├── biological-review.md
│   │   ├── review-summary.md
│   │   └── refinement.md
│   │
│   ├── human-feedback/              # ⭐ NEW: 人类反馈记录
│   │   ├── feedback-round-1.md
│   │   ├── feedback-round-2.md
│   │   └── ...
│   │
│   ├── REVIEW_SUMMARY.md
│   ├── FINAL_PROPOSAL.md
│   ├── HUMAN_APPROVAL.md            # ⭐ NEW: 人类批准记录
│   ├── score-history.md
│   └── REFINEMENT_REPORT.md
│
├── 08_CODE_MODIFICATION_PLAN.md
├── 09_DEMO_VALIDATION.md
├── 10_ANALYSIS_RESULTS.md
│
├── 11_PPT_PRESENTATION.md           # ⭐ 新增：组会汇报 PPT
│
└── FINAL_MANUSCRIPT_PLAN.md
```

## 三审稿人评审标准

### Editor（编辑）
- **职责**：初审，判断是否达到 Nature 子刊水平
- **评审维度**：创新性、可行性、期刊匹配度
- **评分**：创新性/10、可行性/10、推荐意见

### 计算审稿人
- **职责**：从计算/算法角度评审
- **评审维度**：算法设计、方法创新、实验严谨性、代码可行性
- **评分**：方法创新/10、技术严谨/10、代码可行/10

### 生物分析审稿人
- **职责**：从生物学/分析角度评审
- **评审维度**：生物学意义、分析设计、数据选择
- **评分**：生物意义/10、分析设计/10、数据选择/10

## 使用方式

```bash
/bio-manuscript-pipeline "topic: 空间多组学整合 | base_work: https://github.com/xxx | innovation: 增加 Cross-Attention | demo_data: https://xxx/data.h5ad | target_journal: nat-communications | num_refine_rounds: 2"
```

## 子 Skill 调用

本 Pipeline 会依次调用以下子 Skill：
- `bio-innovation-check`（Step 1）
- `bio-task-system`（Step 2）
- `bio-dataset-search`（Step 3）
- `bio-metric-system`（Step 4）
- `bio-analysis-system`（Step 5）
- `bio-figure-design`（Step 6）
- `bio-manuscript-text`（Step 7）
- `bio-manuscript-refine`（Step 7.5）⭐
- `bio-human-feedback`（Step 7.6）⭐ NEW - 人类反馈验证
- `bio-code-modification`（Step 8）
- `bio-demo-validate`（Step 9）
- `bio-ppt-generate`（Step 11）⭐

## 注意事项

1. **Phase 1 完成后**：检查创新性评估结果
2. **Phase 2 完成后**：检查 Figure 设计和文案
3. **Phase 2.5（Refine Loop）**：每轮评分需达到 7+ 才能进入下一阶段
4. **Phase 2.6（人类反馈验证）**：⭐ 关键检查点
   - 呈现 FINAL_PROPOSAL.md 给人类审阅
   - 必须等待人类明确反馈
   - 同意 → 继续 Phase 3
   - 不同意 → 根据问题级别返回对应阶段迭代
   - 所有反馈记录到 refine-logs/human-feedback/
5. **Phase 3**：Demo 验证如果不可行，回到 Step 8 重新设计
6. **迭代收敛**：通常 2 轮 Refine 后评分趋于稳定
7. **最终检查**：使用 FINAL_PROPOSAL.md 作为执行依据
8. **人类批准**：必须有人类批准记录（HUMAN_APPROVAL.md）才能进入 Phase 3