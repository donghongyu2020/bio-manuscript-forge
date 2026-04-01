# bio-ppt-generate

**生成组会汇报 PPT**

基于最终 Proposal 和 Demo 结果，生成简洁版组会汇报 PPT。

## 功能

1. 从 FINAL_PROPOSAL.md 提取核心内容
2. 从 DEMO_VALIDATION.md 提取 Demo 结果
3. 生成 10-15 页组会汇报 PPT
4. 输出 Markdown 格式（可转 PPT）

## 输入格式

```
final_proposal: [FINAL_PROPOSAL.md 路径]
demo_result: [DEMO_VALIDATION.md 路径]
ppt_title: [PPT标题]
author: [作者]
date: [日期]
```

## PPT 结构（12-15 页）

```
1. 封面（Title Slide）
   ├─ 标题
   ├─ 作者
   └─ 日期

2. 研究背景（Background）
   ├─ 领域介绍（1段）
   └─ 现有方法不足（1-2点）

3. 研究问题（Research Question）
   └─ 核心问题 + 重要性

4. 创新点（Innovation）
   ├─ 概念创新
   ├─ 算法创新
   └─ 任务/分析创新

5. 方法概述（Method Overview）
   └─ Figure 1 算法流程图

6. 任务设计（Task Design）
   └─ Figure 1 Panel c 任务层级

7. 数据与指标（Data & Metrics）
   ├─ 数据集概览
   └─ 评价指标

8. Demo 结果（Demo Results）
   ├─ Demo 配置
   ├─ 运行结果
   └─ 可行性结论

9. 预期 Figure 展示（Expected Figures）
   ├─ Figure 2-5 设计概览
   └─ 每个 Figure 的核心发现

10. 生物学意义（Biological Significance）
    └─ 预期生物学发现

11. 下一步计划（Next Steps）
    ├─ 代码完善
    ├─ 数据准备
    ├─ 实验运行
    └─ 论文撰写

12. 总结（Summary）
    ├─ 一句话研究目标
    ├─ 三个核心创新
    └─ 预期成果

13. Q&A
```

## 输出格式

### 方式 1：Markdown PPT（推荐）

```markdown
---
marp: true
theme: gaia
paginate: true
---

# [研究标题]

**[作者]**

[日期]

---

# 研究背景

[领域介绍]

**现有方法不足**：
1. [不足1]
2. [不足2]

---

# 研究问题

> [核心研究问题]

**为什么重要**：
- [重要性1]
- [重要性2]

---

# 创新点

## 概念创新
[概念创新描述]

## 算法创新
[算法创新描述]

## 任务创新
[任务创新描述]

---

# 方法概述

![width:800px](figure1.png)

[方法描述]

---
...
```

### 方式 2：HTML PPT（reveal.js）

```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@4.3.1/dist/reset.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@4.3.1/dist/reveal.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@4.3.1/dist/theme/black.css">
</head>
<body>
  <div class="reveal">
    <div class="slides">
      <section>
        <h1>[研究标题]</h1>
        <p>[作者]</p>
        <p>[日期]</p>
      </section>
      
      <section>
        <h2>研究背景</h2>
        <p>[内容]</p>
      </section>
      
      ...
    </div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/reveal.js@4.3.1/dist/reveal.js"></script>
  <script>Reveal.initialize();</script>
</body>
</html>
```

### 方式 3：PowerPoint 结构大纲

```
Slide 1: 封面
  - 标题：[研究标题]
  - 作者：[作者]
  - 日期：[日期]

Slide 2: 研究背景
  - 领域介绍：[内容]
  - 现有方法不足：
    * [不足1]
    * [不足2]

Slide 3: 研究问题
  - 核心问题：[问题]
  - 重要性：
    * [原因1]
    * [原因2]

Slide 4: 创新点
  - 概念创新：[描述]
  - 算法创新：[描述]
  - 任务创新：[描述]

Slide 5: 方法概述
  - [插入 Figure 1 算法流程图]
  - 方法描述：[简要说明]

Slide 6: 任务设计
  - 任务层级：
    * Task 1: [描述]（难度：低）
    * Task 2: [描述]（难度：中）
    * Task 3: [描述]（难度：高）
    * Task 4: [描述]（难度：最高）

Slide 7: 数据与指标
  - 数据集：
    * Dataset 1: [描述]
    * Dataset 2: [描述]
    * Dataset 3: [描述]
    * Dataset 4: [描述]
  - 评价指标：
    * ARI（聚类一致性）
    * Pearson r（模态相关性）
    * MSE（重建误差）

Slide 8: Demo 结果
  - Demo 配置：
    * 数据：[数据名称]
    * 样本数：[数量]（subsample）
    * Epochs：[数量]
  - 运行结果：
    * Loss：[数值]
    * ARI：[数值]
    * 状态：✅ 可行 / ❌ 需修改
  - 结论：[可行性判断]

Slide 9: 预期 Figure 展示
  - Figure 2: [任务A] - [预期发现]
  - Figure 3: [任务B] - [预期发现]
  - Figure 4: [任务C] - [预期发现]
  - Figure 5: [任务D] - [预期发现]

Slide 10: 生物学意义
  - 预期发现：
    * [发现1]
    * [发现2]
  - 生物学贡献：
    * [贡献1]
    * [贡献2]

Slide 11: 下一步计划
  - [ ] 代码完善：[具体任务]
  - [ ] 数据准备：[具体任务]
  - [ ] 实验运行：[具体任务]
  - [ ] 论文撰写：[具体任务]

Slide 12: 总结
  - 研究目标：[一句话]
  - 核心创新：
    1. [创新1]
    2. [创新2]
    3. [创新3]
  - 预期成果：
    * [成果1]
    * [成果2]

Slide 13: Q&A
  - 感谢聆听！
  - 欢迎提问
```

---

## PPT 设计原则

### 内容原则

1. **简洁**：每页不超过 6 行文字
2. **聚焦**：每页一个核心观点
3. **视觉**：多用图表，少用文字
4. **逻辑**：问题 → 方法 → 结果 → 意义

### 时间分配（10-15 分钟）

| 部分 | 时间 | 页数 |
|------|------|------|
| 背景 + 问题 | 2 min | 2 页 |
| 创新点 | 1 min | 1 页 |
| 方法概述 | 2 min | 2 页 |
| 任务 + 数据 | 1 min | 2 页 |
| Demo 结果 | 2 min | 1 页 |
| 预期 Figure | 2 min | 1 页 |
| 生物学意义 | 1 min | 1 页 |
| 下一步 + 总结 | 1 min | 2 页 |
| Q&A | 3 min | 1 页 |

---

## 输出示例

### 11_PPT_PRESENTATION.md

```markdown
---
marp: true
theme: gaia
paginate: true
backgroundColor: #fff
color: #333
---

# MultiOmics-Spatial: 跨模态空间组学整合方法

**董弘禹**

西湖大学 · 计算生物学实验室

2026-04-01

---

# 研究背景

空间组学技术发展迅速，为理解组织异质性提供新视角

**现有方法不足**：
1. **假设数据完整**：难以处理部分模态缺失
2. **任务覆盖不全**：忽略跨切片、跨分辨率整合
3. **生物学验证不足**：缺乏 marker、通路等分析验证

---

# 研究问题

> 如何实现跨切片、跨模态、跨分辨率的空间组学整合？

**为什么重要**：
- 实际数据往往不完整、多平台
- 整合后可揭示更全面的生物学信息
- 支持复杂组织的研究需求

---

# 创新点

## 概念创新
首次提出**对角整合**概念：跨切片 + 跨模态 + 跨分辨率

## 算法创新
引入 **Cross-Attention** 处理模态交互 + **Spatial Alignment** 配准

## 任务创新
设计 **Level 1-4** 任务体系，难度递进验证

---

# 方法概述

![width:700px](../06_FIGURE_DESIGNS/FIGURE_1_DESIGN.md)

- Encoder 提取各模态特征
- **Cross-Attention** 学习模态交互
- Decoder 重建整合表示

---

# 任务设计

| Level | 任务 | 难度 | 数据 |
|-------|------|------|------|
| 1 | 垂直整合 | 低 | Xenium（RNA+Protein） |
| 2 | 水平整合 | 中 | Visium（多切片） |
| 3 | 马赛克整合 | 高 | MISAR-seq（部分模态） |
| 4 | 对角整合 | 最高 | Stereo-seq + Visium |

---

# 数据与指标

## 数据集
- **Xenium** Breast: 50K cells, RNA + Protein
- **Visium** Brain: 3K spots × 2 slices
- **MISAR-seq** E13/15/18: RNA + ATAC
- **Stereo-seq** Brain: 100K cells, 高分辨率

## 评价指标
- **ARI**: 聚类一致性
- **Pearson r**: 模态相关性
- **MSE**: 重建/配准误差

---

# Demo 结果

## Demo 配置
- 数据：Xenium Breast Cancer（subsample 1K cells）
- Epochs：10（快速验证）
- Device：GPU

## 运行结果
| 指标 | 数值 |
|------|------|
| Initial Loss | 2.35 |
| Final Loss | 0.88 |
| ARI | 0.82 |
| 状态 | ✅ 可行 |

**结论**：方法可行，可继续执行

---

# 预期 Figure 展示

| Figure | 任务 | 核心发现 |
|--------|------|---------|
| Fig 2 | 垂直整合 | RNA+Protein 整合提升 ARI +15% |
| Fig 3 | 水平整合 | 切片配准准确，MSE < 0.1 |
| Fig 4 | 马赛克整合 | 缺失模态推断 Pearson r > 0.85 |
| Fig 5 | 对角整合 | 跨平台整合成功，新生物学发现 |

---

# 生物学意义

## 预期发现
- 识别新的细胞亚型空间分布
- 揭示跨模态调控机制
- 发现肿瘤微环境免疫-肿瘤通讯

## 生物学贡献
- 为复杂组织研究提供新工具
- 支持多平台数据整合
- 助力空间组学数据挖掘

---

# 下一步计划

## 近期（1-2 周）
- [ ] 完善代码实现
- [ ] 准备完整数据集
- [ ] 运行全量实验

## 中期（1-2 月）
- [ ] 生成所有 Figure 数据
- [ ] 完成论文初稿
- [ ] 补充 Ablation Study

## 目标
- 投稿 Nature Communications
- 预计 2026 Q2 完成初稿

---

# 总结

## 研究目标
> 开发首个支持对角整合的空间多组学分析方法

## 核心创新
1. **概念**：对角整合（跨切片+跨模态+跨分辨率）
2. **算法**：Cross-Attention + Spatial Alignment
3. **验证**：Level 1-4 任务体系 + Demo 可行

## 预期成果
- 方法：MultiOmics-Spatial（开源）
- 论文：Nature Communications
- 发现：新的空间调控机制

---

# Q&A

感谢聆听！

欢迎提问与讨论

📧 dhy020612@163.com

---
```

---

## 使用方式

```bash
# 作为 Pipeline 最后一步自动调用
/bio-manuscript-pipeline "..."

# 或单独调用
/bio-ppt-generate "final_proposal: FINAL_PROPOSAL.md | demo_result: DEMO_VALIDATION.md | ppt_title: MultiOmics-Spatial | author: 董弘禹 | date: 2026-04-01"
```

---

## 生成工具

### Marp（Markdown → PPT）

```bash
# 安装 Marp CLI
npm install -g @marp-team/marp-cli

# 转换为 PPT
marp 11_PPT_PRESENTATION.md -o presentation.pptx

# 或 PDF
marp 11_PPT_PRESENTATION.md -o presentation.pdf
```

### Reveal.js（HTML PPT）

直接打开 HTML 文件即可演示。

---

## 注意事项

1. **保持简洁**：组会汇报 10-15 分钟，不要过多细节
2. **突出创新**：创新点是评审关键
3. **Demo 必须有**：证明可行性
4. **下一步清晰**：让导师知道你的计划
5. **视觉友好**：多用图表，少用文字