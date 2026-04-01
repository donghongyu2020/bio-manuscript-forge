# 分析手段参考列表

> 按任务类型整理的分析手段，供 Figure 设计和文案撰写时参考

---

## 🔬 通用分析手段

### 安全分析

| 分析手段 | 描述 | 适用任务 |
|---------|------|---------|
| **注意力熵分析** | 计算模型注意力的熵值，区分正常和后门样本 | 所有任务 |
| **触发基因识别** | 识别被后门攻击篡改的基因/特征 | 所有任务 |
| **攻击溯源** | 分析攻击目标类型、触发模式 | 所有任务 |
| **检测 ROC 曲线** | 后门样本检测的性能评估 | 所有任务 |
| **ASR 分析** | 攻击成功率随 poisoning rate 变化 | 所有任务 |

### 生物学分析

| 分析手段 | 描述 | 适用任务 |
|---------|------|---------|
| **Marker gene 分析** | 标记基因的表达和注意力 | Cell annotation |
| **Pathway enrichment** | GO/KEGG 通路富集分析 | 所有任务 |
| **Gene set analysis** | 功能基因集的分析 | 所有任务 |

---

## 📊 任务特定分析手段

### 1. Cell Type Annotation

| 分析手段 | 描述 | 示例 |
|---------|------|------|
| Marker gene attention | 标记基因的注意力权重 | INS, PDX1, MAFA (β-cell) |
| Marker expression correlation | 标记基因表达与分类的相关性 | INS expression vs beta-cell assignment |
| Cell type recovery rate | 特定细胞类型的恢复率 | β-cell recovery: 12.5% → 98.2% |
| Confusion matrix | 细胞类型混淆矩阵 | Before/After defense |
| GO enrichment of attended genes | 关注基因的生物学功能 | Hormone secretion vs Ribosome |

**案例模板**：
```
Case: β-cell misclassification attack
- Attack: β-cell → α-cell (wrong)
- Before defense: INS attention 0.12, β-cell recovery 12.5%
- After defense: INS attention 0.78, β-cell recovery 98.2%
- Biological validation: INS expression correlation 0.91
```

### 2. Perturbation Prediction

| 分析手段 | 描述 | 示例 |
|---------|------|------|
| Direction consistency | 扰动方向预测正确率 | PDX1 KO → INS↓ (97.3%) |
| Gene trajectory | 基因表达变化轨迹 | Expected genes show correct pattern |
| Pathway coherence | 扰动相关通路的注意力 | Insulin secretion pathway |
| Perturbation-specific attention | 扰动目标基因的注意力 | PDX1, INS attention |

**案例模板**：
```
Case: PDX1 knockout prediction
- Biology: PDX1 is master TF for INS expression
- Expected: PDX1 KO → INS down-regulation
- Before defense: Wrong prediction (INS↑)
- After defense: Correct prediction (INS↓), Pearson 0.68
- Direction accuracy: 94.8%
```

### 3. Gene Regulatory Network Inference

| 分析手段 | 描述 | 示例 |
|---------|------|------|
| TF-target edge recovery | 已知 TF-target 关系的恢复 | PDX1→INS: 0.22 → 0.89 |
| False edge analysis | 虚假调控边的分析 | RPL5→INS: 0.85 → 0.10 |
| Network topology | 网络拓扑指标 | Hub genes, modularity |
| ChIP-seq validation | 与实验数据的验证 | PDX1 ChIP-seq confirms |
| Network statistics | 网络统计指标 | Edge count, true/false positive |

**案例模板**：
```
Case: PDX1-INS regulatory axis
- Known biology: PDX1 binds INS promoter (ChIP-seq confirmed)
- Before defense: PDX1→INS edge 0.22, RPL5→INS 0.85 (false)
- After defense: PDX1→INS 0.89, RPL5→INS 0.10
- Network hub restored: PDX1 (TF) not RPL5 (ribosomal)
```

### 4. Spatial Domain Detection

| 分析手段 | 描述 | 示例 |
|---------|------|------|
| Spatial attention pattern | 空间注意力分布 | Anatomical regions |
| Domain boundary preservation | 域边界保持 | CA1/CA3 separation |
| Spatial coherence score | 空间连贯性分数 | Spatial continuity |
| Region marker localization | 区域标记基因定位 | Wfs1→CA1, Prox1→CA3 |
| Spatial expression correlation | 空间表达相关性 | Spatial pattern match |

**案例模板**：
```
Case: Hippocampus subregion detection
- Biology: CA1, CA2, CA3, DG are distinct regions
- Before defense: Merged into one domain, ARI 0.48
- After defense: Correctly separated, ARI 0.78
- Marker validation: Wfs1→CA1, Prox1→CA3, Tbr1→DG
```

### 5. Multi-omics Integration

| 分析手段 | 描述 | 示例 |
|---------|------|------|
| Cross-modal attention | 跨模态注意力 | RNA↔ATAC balance |
| RNA-ATAC correlation | RNA 表达与 ATAC 的相关性 | CD4 expr ↔ CD4 promoter |
| Cross-modal detection rate | 跨模态攻击检测率 | RNA-only attack: 96.2% |
| Joint vs modality-specific | 联合 vs 单模态防御对比 | Joint +6.3% defense rate |
| Pathway integration quality | 通路整合质量 | T cell activation pathway |

**案例模板**：
```
Case: CD4 RNA-ATAC relationship
- Biology: CD4 expression correlates with CD4 promoter accessibility
- Before defense: Correlation r = 0.38 (broken)
- After defense: Correlation r = 0.82 (restored)
- Biological meaning: CD4+ T cells correctly identified
```

---

## 📋 分析选择指南

### 按问题类型选择

| 问题类型 | 推荐分析 |
|---------|---------|
| 模型关注了什么？ | Attention pattern, Gene set analysis |
| 生物学意义是什么？ | Marker gene, Pathway enrichment |
| 防御保护了什么？ | Recovery rate, Correlation restoration |
| 具体案例？ | 1-2 个深入案例，展示 before/after |

### 按用户输入选择

| 用户输入的分析手段 | 对应分析方法 |
|------------------|-------------|
| marker gene attention | Marker gene attention, Expression correlation |
| perturbation direction | Direction consistency, Gene trajectory |
| TF-target recovery | TF-target edge, ChIP-seq validation |
| spatial domain preservation | Domain boundary, Spatial coherence |
| RNA-ATAC coherence | Cross-modal attention, Correlation |

---

## ⚠️ 注意事项

1. **分析必须有生物学意义解释**：不只展示数字，要说明保护了什么生物学内容
2. **案例要具体**：提到具体基因、具体细胞类型、具体通路
3. **对比要清晰**：Before defense vs After defense
4. **验证要有依据**：ChIP-seq、已知生物学事实等

---

*分析手段参考，持续更新*
*Bio-Manuscript-Forge v1.0*