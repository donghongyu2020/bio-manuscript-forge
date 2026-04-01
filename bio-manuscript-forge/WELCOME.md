# 🚀 Bio-Manuscript-Forge 启动

欢迎使用 Bio-Manuscript-Forge！我将帮助你从研究想法生成完整的 Manuscript Plan。

---

## 📋 输入模板

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

---

## ✅ 成功案例参考

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

## 📤 你将获得的输出

| 文件 | 内容 |
|------|------|
| **PPT** | 26页组会汇报 |
| **FINAL_PROPOSAL** | 完整研究方案 |
| **Figure 2-7 (v3)** | 每个任务的详细设计 + 安全生物学分析 |
| **论文文案 (v2)** | Introduction, Results, Discussion, Methods |

---

## 📖 推荐阅读顺序

```
1️⃣ PPT                      → 快速了解全貌
2️⃣ FINAL_PROPOSAL           → 完整方案
3️⃣ Figure 2-7 (v3)          → 详细设计 + 案例
4️⃣ 论文文案 (v2)            → 完整草稿
```

---

**请填写你的研究内容，我将开始执行 Pipeline！**