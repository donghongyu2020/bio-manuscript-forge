# Bio-Manuscript-Forge 经验教训

> 记录实际使用过程中的用户反馈和最佳实践

---

## 📝 用户反馈记录

### 反馈 1：Figure 设计原则 — 任务为先

**问题**：原设计按"多模型对比"、"多任务性能"等维度组织 Figure，结构不清晰。

**用户反馈**：
> 希望每个 Figure 对应一个具体任务，任务里包含多种算法/模型的攻击和防御，数据根据任务选取，指标和分析也依据任务选取。

**正确做法**：
- **Figure 1**：算法创新性（方法框架）
- **Figure 2-N**：每个 Figure = 一个任务
  - 包含多种模型/算法
  - 数据根据任务选取
  - 指标根据任务选取
  - 分析手段根据任务选取

**示例结构**：
```
Figure 1: 方法框架 Overview
Figure 2: Task 1 (Cell Annotation) - 多模型 + 任务数据 + 任务指标 + 任务分析
Figure 3: Task 2 (Perturbation) - 多模型 + 任务数据 + 任务指标 + 任务分析
Figure 4: Task 3 (GRN Inference) - 多模型 + 任务数据 + 任务指标 + 任务分析
Figure 5: Task 4 (Spatial Domain) - 多模型 + 任务数据 + 任务指标 + 任务分析
Figure 6: Task 5 (Multi-omics) - 多模型 + 任务数据 + 任务指标 + 任务分析
Figure 7: Summary + 生物学意义总结
```

**关键原则**：
- ✅ 任务统领一切
- ✅ 数据/指标/分析随任务而定
- ✅ 每图包含多模型对比
- ❌ 不要按"多模型对比"、"多任务性能"这种抽象维度组织

---

### 反馈 2：分析手段必须增强

**问题**：原设计只展示定量指标（ASR、准确率等），缺乏深入分析。

**用户反馈**：
> 每张图里有分析吗？分析手段一定要增加和额外注意！

**正确做法**：
每个任务 Figure 必须包含**安全 + 生物学分析**：

| 分析类型 | 内容 | 示例 |
|---------|------|------|
| **定量测评** | ASR, Accuracy, F1 等指标 | 前几个 Panel |
| **注意力分析** | 注意力模式变化 | Marker gene attention recovery |
| **案例研究** | 1-2 个深入案例 | PDX1 KO → INS↓, β细胞标记基因恢复 |
| **生物学意义** | 体现什么生物学价值 | Pathway enrichment, TF-target recovery |

**每个 Figure 必须有的分析 Panel**：
```
Panel a-d: 定量测评（ASR, 准确率等）
Panel e: 注意力模式分析（安全 + 生物学结合）
Panel f: 深入案例研究（具体生物学问题）
Panel g: 生物学意义总结（pathway, marker 等）
```

**分析手段清单**（参考）：
- Marker gene attention recovery
- Perturbation direction consistency
- TF-target edge recovery
- Spatial domain preservation
- RNA-ATAC coherence
- Pathway enrichment analysis
- GO enrichment of attended genes
- Network topology analysis
- Gene set entropy analysis

---

### 反馈 3：Figure 改完文案必须同步

**问题**：Figure 改了，文案没同步更新。

**用户反馈**：
> Figure 改完之后，文案也得改呀！

**正确做法**：
1. 修改 Figure 设计 → 立即修改 Results 结构
2. Results 每个 section 对应一个 Figure
3. Results 内容 = Figure 设计的文字版

**示例对应**：
```
Figure 2 (Cell Annotation) 
    ↓ 对应
Results 2.2 (Cell Type Annotation Defense)

Figure 3 (Perturbation)
    ↓ 对应
Results 2.3 (Perturbation Prediction Defense)
```

**必须检查**：
- [ ] Figure 设计更新后，Results 结构是否对应？
- [ ] Figure 中的分析 Panel，Results 是否有对应文字？
- [ ] Figure 中的案例，Results 是否详细展开？

---

## ✅ 最佳实践总结

### 1. 输入阶段

用户填写时要明确：
- **任务**：明确列出所有任务
- **生物学分析手段**：如何体现生物学意义

### 2. Figure 设计阶段

**结构原则**：
- Figure 1 = 方法框架
- Figure 2-N = 一个任务一个图
- 最后 1 个 Figure = 总结

**每个任务 Figure 必须包含**：
1. 定量测评
2. 安全分析（注意力、熵等）
3. 生物学分析（marker、pathway 等）
4. 深入案例（1-2 个）

### 3. 文案撰写阶段

**Results 结构**：
- 2.1 Overview
- 2.2-N 每个任务一个 section
- 每个 section = 对应 Figure 的文字版

**同步检查清单**：
- [ ] Figure 有这个 Panel → Results 有对应段落
- [ ] Figure 有这个案例 → Results 有详细展开
- [ ] Figure 有这个分析 → Results 有生物学解释

### 4. 分析增强原则

**没有固定套路，但意识要增强**：
- 每个任务都要思考：分析手段是什么？
- 安全部件：注意力、熵、检测率
- 生物学部件：marker、pathway、relationship
- 结合点：防御保护了什么生物学意义？

---

## 📋 检查清单模板

### Phase 2 检查（Figure 设计完成后）

```
- [ ] Figure 1 是方法框架？
- [ ] Figure 2-N 每个对应一个任务？
- [ ] 每个 Figure 包含多模型对比？
- [ ] 每个 Figure 有定量测评 Panel？
- [ ] 每个 Figure 有安全分析 Panel？
- [ ] 每个 Figure 有生物学分析 Panel？
- [ ] 每个 Figure 有 1-2 个深入案例？
- [ ] 分析手段是否多样化？
```

### Phase 2 文案同步检查

```
- [ ] Results 结构与 Figure 对应？
- [ ] 每个 Figure Panel 有对应文字？
- [ ] 案例在 Results 中详细展开？
- [ ] 生物学意义有解释？
```

---

## 🔄 更新记录

| 日期 | 反馈内容 | 修改文件 |
|------|---------|---------|
| 2026-04-01 | Figure 按任务组织 | SKILL.md, FIGURE_DESIGN |
| 2026-04-01 | 增强分析手段 | SKILL.md, 各 FIGURE_v3 |
| 2026-04-01 | 文案同步更新 | RESULTS_v2.md |

---

*经验教训文件，持续更新*
*Bio-Manuscript-Forge v1.0*