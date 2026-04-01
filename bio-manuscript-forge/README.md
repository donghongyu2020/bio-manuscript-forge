# Bio-Manuscript-Forge

**从研究想法到完整 Manuscript Plan 的一条龙 Pipeline**

> 输入研究想法 → 输出 PPT + Figure 设计 + 论文文案

---

## 🎯 这是什么？

Bio-Manuscript-Forge 是一个面向计算生物学研究的 Manuscript 规划工具：

- **输入**：研究主题 + 基础工作 + 创新点描述
- **输出**：完整的 Manuscript Plan（PPT + Figure 设计 + 论文文案）

**适用场景**：
- 📝 规划新论文
- 🎤 准备组会汇报
- 🔬 撰写基金申请
- 📊 设计实验方案

---

## 🚀 快速开始

### Step 1: 准备输入

复制以下模板，填写你的研究内容：

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

### Step 2: 提交给 AI

将填写好的内容发送给支持 Bio-Manuscript-Forge 的 AI 助手。

### Step 3: 获取输出

AI 将执行 Pipeline 并生成以下内容：

| 输出文件 | 内容 | 用途 |
|---------|------|------|
| **PPT** | 26页组会汇报 | 组会、答辩 |
| **FINAL_PROPOSAL** | 完整研究方案 | 项目规划 |
| **Figure 2-7 (v3)** | 每个任务的详细设计 | 论文制图 |
| **论文文案 (v2)** | Intro, Results, Discussion, Methods | 论文撰写 |

---

## 📋 输入字段详解

### 必填字段

| 字段 | 说明 | 示例 |
|------|------|------|
| `topic` | 研究主题 | 单细胞多组学整合方法 |
| `base_work` | 基础工作（论文+代码） | 论文DOI + GitHub链接 |
| `innovation` | 创新点（含子项） | 见下方详细说明 |
| `demo_data` | Demo数据集 | Figshare/GEO链接 |

### innovation 子项

| 子项 | 说明 | 示例 |
|------|------|------|
| `算法创新性` | 核心方法/算法创新 | 注意力机制改进、损失函数创新 |
| `任务` | 覆盖的下游任务 | cell annotation, perturbation, GRN |
| `数据` | 数据集来源 | 公开数据集、特定组织 |
| `benchmark` | 评估基准 | 已有基准或自建基准 |
| `计算指标` | 安全指标 + 任务指标 | ASR, Accuracy, F1, ARI |
| `生物学分析手段` | 如何体现生物学意义 | marker gene, pathway, regulatory relationship |

### 可选字段

| 字段 | 默认值 | 说明 |
|------|--------|------|
| `target_journal` | nat-communications | 目标期刊 |
| `num_refine_rounds` | 2 | Refine 迭代轮数 |

---

## ✅ 成功案例

### 案例：OmicsDefense - 组学大模型安全防御

**输入**：

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

**输出**：
- 26页组会 PPT
- 6个 Figure 详细设计（每个任务一个图，含安全+生物学分析）
- 完整论文文案（Introduction, Results, Discussion, Methods）

---

## 🔄 Pipeline 流程

```
Phase 1: 系统构建
├─ Step 1: 创新性检测 → 搜索相关论文，评估创新级别
├─ Step 2: 任务体系构建 → 确定任务层级和覆盖范围
├─ Step 3: 数据集搜索 → 匹配数据集与任务
├─ Step 4: 指标体系构建 → 安全指标 + 任务指标
└─ Step 5: 分析方法体系 → 安全分析 + 生物学分析

Phase 2: 设计与文案
├─ Step 6: Figure 设计
│   ├─ Figure 1: 方法框架
│   ├─ Figure 2-N: 每个任务一个图
│   │   ├─ 定量测评
│   │   ├─ 安全分析
│   │   ├─ 生物学分析
│   │   └─ 深入案例
│   └─ Figure N+1: 总结
├─ Step 7: 论文文案生成
│   ├─ Introduction
│   ├─ Results（与 Figure 对应）
│   ├─ Discussion
│   └─ Methods

Phase 2.5: Refine Loop
├─ 三审稿人评审
│   ├─ Editor（创新性）
│   ├─ 计算审稿人（方法）
│   └─ 生物学审稿人（意义）
├─ 迭代修改
└─ 最终方案

Phase 2.6: 人类反馈
├─ 呈现方案给用户
├─ 等待反馈
└─ 根据反馈迭代

Phase 3: 输出
├─ PPT 生成
└─ 完整文档打包
```

---

## 📁 输出文件结构

```
manuscript-[project-name]/
├── 08_ppt/
│   └── PRESENTATION.md              # 组会汇报 PPT
│
├── refine-logs/
│   └── FINAL_PROPOSAL.md            # 完整研究方案
│
├── 06_figures/
│   ├── FIGURE_1_DESIGN.md           # 方法框架
│   ├── FIGURE_2_DESIGN_v3.md        # 任务1 + 分析
│   ├── FIGURE_3_DESIGN_v3.md        # 任务2 + 分析
│   ├── FIGURE_4_DESIGN_v3.md        # 任务3 + 分析
│   ├── FIGURE_5_DESIGN_v3.md        # 任务4 + 分析
│   ├── FIGURE_6_DESIGN_v3.md        # 任务5 + 分析
│   └── FIGURE_7_DESIGN_v3.md        # 总结
│
├── 07_manuscript/
│   ├── INTRODUCTION_v2.md
│   ├── RESULTS_v2.md
│   ├── DISCUSSION_v2.md
│   └── METHODS_v2.md
│
└── 01-05/                           # Phase 1 输出（可选查看）
```

---

## 📖 推荐阅读顺序

```
1️⃣ PRESENTATION.md         快速了解全貌
2️⃣ FINAL_PROPOSAL.md       完整研究方案
3️⃣ FIGURE_*_v3.md          详细设计 + 案例分析
4️⃣ INTRODUCTION_v2.md 等   论文文案
```

---

## 💡 最佳实践

### 1. 任务为先

每个 Figure 对应一个具体任务，数据/指标/分析随任务而定：

```
✅ 正确：Figure 2 = Cell Annotation（含多模型对比）
❌ 错误：Figure 3 = 多模型对比
```

### 2. 分析增强

每个任务 Figure 必须包含：
- 定量测评（ASR, Accuracy 等）
- 安全分析（注意力、熵）
- 生物学分析（marker gene, pathway）
- 1-2 个深入案例

### 3. 文案同步

Figure 改完后，立即检查 Results 是否有对应段落。

---

## 📚 参考文档

| 文档 | 内容 |
|------|------|
| `INPUT_TEMPLATE.md` | 输入模板 + 填写指南 |
| `ANALYSIS_METHODS.md` | 分析手段参考列表 |
| `EXPERIENCES.md` | 经验教训总结 |

---

## ❓ 常见问题

### Q: 生物学分析手段怎么填？

A: 思考以下问题：
- 你的方法保护了什么生物学内容？
- 如何验证？（marker gene、pathway、regulatory relationship）
- 具体案例？（PDX1→INS, CA1/CA3 boundary 等）

### Q: 输出多久能生成？

A: 通常 5-15 分钟，取决于任务数量和 Refine 轮数。

### Q: Phase 1 文件需要看吗？

A: 不需要。Phase 1 内容已整合到后续输出中，直接看 PPT 和 FINAL_PROPOSAL 即可。

### Q: 可以中途修改吗？

A: 可以！在任何阶段都可以提供反馈，AI 会根据反馈调整。

---

## 📄 License

MIT License

---

## 🙏 致谢

感谢所有使用者的反馈，帮助改进这个 Pipeline！

---

**开始你的 Manuscript 规划之旅吧！** 🚀