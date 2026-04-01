# Bio-Manuscript-Forge 输入模板

## 快速开始

复制以下模板，填写你的研究内容：

```
topic: [你的研究主题]
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
target_journal: [目标期刊，可选]
num_refine_rounds: [Refine轮数，可选]
```

---

## 成功案例：组学大模型安全防御

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

---

## 输出结果

执行后生成以下文件：

### 核心输出
- `08_ppt/PRESENTATION.md` - 组会汇报 PPT
- `refine-logs/FINAL_PROPOSAL.md` - 完整研究方案

### Figure 设计 (v3)
- `06_figures/FIGURE_2_DESIGN_v3.md` - 任务1 + 安全生物学分析
- `06_figures/FIGURE_3_DESIGN_v3.md` - 任务2 + 安全生物学分析
- ...

### 论文文案 (v2)
- `07_manuscript/INTRODUCTION_v2.md`
- `07_manuscript/RESULTS_v2.md`
- `07_manuscript/DISCUSSION_v2.md`
- `07_manuscript/METHODS_v2.md`

---

## 填写指南

### topic（必填）
- 简洁明确的研究主题
- 示例：`空间多组学整合方法研究`

### base_work（必填）
- 论文链接：DOI 或期刊网址
- 代码链接：GitHub 仓库
- 可添加多个相关工作

### innovation（必填）

#### innovation.算法创新性
- 核心：你的方法/算法有什么创新？
- 关键词：模型架构、损失函数、训练策略、适配创新

#### innovation.任务
- 你要覆盖的下游任务
- 示例：cell annotation, perturbation prediction, GRN inference
- 建议：3-5个任务

#### innovation.数据
- 数据集来源或类型
- 示例：公开数据集、用户数据、特定组织类型

#### innovation.benchmark
- 评估基准
- 示例：已有基准（BEELINE, scBackdoor）、自建基准

#### innovation.计算指标
- 安全指标 + 任务指标
- 安全：ASR, Defense Rate, Detection AUC
- 任务：Accuracy, F1, ARI, Pearson, cLISI 等

#### innovation.生物学分析手段
- **关键**：如何体现生物学意义
- 示例：
  - Marker gene attention
  - Perturbation direction
  - Regulatory relationship
  - Pathway enrichment
  - Spatial pattern

### demo_data（必填）
- Demo 数据集链接
- 用于快速验证方法可行性

### target_journal（可选）
- 目标期刊
- 默认：nat-communications

### num_refine_rounds（可选）
- Refine 轮数
- 默认：2

---

## 推荐阅读顺序

```
1️⃣ PPT (08_ppt/PRESENTATION.md)
2️⃣ FINAL_PROPOSAL (refine-logs/FINAL_PROPOSAL.md)
3️⃣ Figure 设计 (06_figures/FIGURE_*_v3.md)
4️⃣ 论文文案 (07_manuscript/*_v2.md)
```

---

*Bio-Manuscript-Forge v1.0*
*Updated: 2026-04-01*