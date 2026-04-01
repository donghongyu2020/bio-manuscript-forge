# bio-analysis-system

**Step 5: 分析方法体系构建**

构建分析方法体系，从已有工作或相邻领域借用，标注 OmicsClaw/Bioclaw skill。

## 功能

1. 从已有工作提取分析方法
2. 或向相邻领域借用分析方法
3. 标注可用工具（OmicsClaw/Bioclaw）
4. 说明为什么用、证明什么生物学结论
5. 为每个 Figure 的每个 Panel 设计分析内容

## 输入格式

```
topic: [研究主题]
paper_count: [已有工作数量]
task_system: [任务体系]
metric_system: [评价指标体系]
dataset_catalog: [数据集目录]
```

## 执行流程

### Step 5.1: 从已有工作提取分析方法

**条件**：paper_count >= 5

**策略**：
```
for paper in top_papers:
    # 阅读每个 Figure，提取分析方法
    for figure in paper.figures:
        for panel in figure.panels:
            analysis = extract_analysis_from_panel(panel)
            
            # 提取内容：
            # - Panel 类型（定量/定性）
            # - 分析方法（聚类、marker展示、UMAP）
            # - 工具（Scanpy、matplotlib）
            # - 参数（resolution、palette）
            # - 证明什么（生物学意义）

# 汇总分析方法
analysis_system = aggregate_analyses(analyses)

# 分类分析方法
classify_analyses(analysis_system)
```

### Step 5.2: 向相邻领域借用分析方法

**条件**：paper_count < 5

**策略**：
```
# 找相邻领域
adjacent_domain = find_adjacent_domain(topic)

# 搜索相邻领域的分析方法
adjacent_analyses = search_analyses_in_domain(adjacent_domain)

# 适配分析
# 例如：单细胞多组学的 UMAP → 空间多组学的 UMAP
# （可能需要考虑空间位置）

adapted_analyses = adapt_analyses_to_domain(adjacent_analyses, topic)
```

### Step 5.3: 分析方法分类

**分类框架**：

```
分析方法分类：
├─ 定量分析方法
│   ├─ 聚类分析（Leiden / Louvain）
│   ├─ 指标计算（ARI / NMI / Pearson r / MSE）
│   ├─ 统计检验（t-test / ANOVA）
│   └─ Baseline 比较运行
│
├─ 定性分析方法
│   ├─ 空间可视化（Spatial scatter plot）
│   ├─ Marker 展示（Feature plot / Violin plot）
│   ├─ Latent 可视化（UMAP / t-SNE）
│   ├─ 配准展示（Before/after comparison）
│   ├─ 模态对比（Multi-modal scatter）
│   └─ 热图展示（Heatmap）
│
└─ 生物学分析方法
    ├─ Cell type annotation
    ├─ Marker gene identification
    ├─ Pathway enrichment（GSEA）
    ├─ Gene regulatory network（GRN）
    ├─ Ligand-receptor communication
    ├─ Spatial statistics（Moran's I）
    └─ Trajectory inference
```

### Step 5.4: OmicsClaw/Bioclaw Skill 映射

**映射原则**：

```
分析任务 → OmicsClaw Skill → 如果没有 → 推荐工具

聚类分析 → spatial-domains → 如果没有 → Scanpy.tl.leiden
细胞类型注释 → spatial-annotate → 如果没有 → CellTypist
Marker展示 → sc-markers → 如果没有 → Scanpy.pl.spatial
通路富集 → spatial-enrichment → 如果没有 → gseapy
空间统计 → spatial-statistics → 如果没有 → squidpy
职系推断 → spatial-trajectory → 如果没有 → CellRank
GRN → sc-grn → 如果没有 → pySCENIC
通讯分析 → spatial-communication → 如果没有 → CellChat
```

### Step 5.5: 分析方法标准化描述

每个分析方法需要标准化描述：

```
分析方法模板：

### [分析方法名称]

**基本信息**：
- 类型：定量分析 / 定性分析 / 生物学分析
- 用途：[用途描述]
- 证明什么：[证明什么生物学结论]

**工具与方法**：
- OmicsClaw Skill：[skill名称]（如果有）
- Bioclaw 参考：[skill路径]（如果有）
- 推荐工具：[工具名称]（如果 OmicsClaw 没有）
- 关键函数：[函数名称]
- 参数建议：[参数列表]

**分析流程**：
```
[分析代码片段或流程描述]
```

**输入输出**：
- 输入：[输入数据描述]
- 输出：[输出数据描述]

**使用场景**：
- 适用任务：[Task X]
- 对应 Figure：[Figure X Panel Y]

**注意事项**：
- [使用注意事项]

**来源文献**：
- 已有多少论文使用：[数量]
```

## 输出格式

```markdown
# 分析方法体系

## 分析来源
- 已有工作提取：[X篇] 论文中的分析方法
- 相邻领域借用：[相邻领域名称] 的分析方法

## 定量分析方法

### 职类分析 (Clustering)

**基本信息**：
- 类型：定量分析
- 用途：识别数据中的细胞群体/domain
- 证明什么：证明整合后的 representation 有清晰结构

**工具与方法**：
- OmicsClaw Skill：spatial-domains
- Bioclaw 参考：single-cell-and-spatial/spatial-domains/SKILL.md
- 推荐工具：Scanpy
- 关键函数：sc.tl.leiden
- 参数建议：resolution=0.5

**分析流程**：
```python
import scanpy as sc

# 职类
sc.tl.leiden(adata, resolution=0.5)

# 查看职类数量
print(f"Number of clusters: {len(adata.obs['leiden'].unique())}")
```

**输入输出**：
- 输入：整合后的 latent representation
- 输出：cluster labels

**使用场景**：
- 适用任务：所有任务
- 对应 Figure：Figure 2-5（预处理）

**注意事项**：
- resolution 参数影响职类数量，需要调整
- 建议先用默认值，再根据结果调整

**来源文献**：
- 已有多少论文使用：所有论文

---

### ARI 计算

**基本信息**：
- 类型：定量分析
- 用途：计算聚类一致性指标
- 证明什么：证明 Our method 职类质量优于 Baselines

**工具与方法**：
- 推荐工具：scikit-learn
- 关键函数：adjusted_rand_score

**分析流程**：
```python
from sklearn.metrics import adjusted_rand_score

# 计算 ARI
ari = adjusted_rand_score(true_labels, predicted_labels)

print(f"ARI: {ari:.3f}")
```

**输入输出**：
- 输入：真实 labels + 预测 labels
- 输出：ARI 值（float）

**使用场景**：
- 对应 Figure：Figure 2-5 Panel b

---

### Pearson Correlation 计算

**基本信息**：
- 类型：定量分析
- 用途：计算模态间表达相关性
- 证明什么：证明模态整合质量

**工具与方法**：
- 推荐工具：NumPy
- 关键函数：np.corrcoef

**分析流程**：
```python
import numpy as np

# 提取两个模态的表达
rna_expr = adata[:, gene].X
protein_expr = adata[:, protein].X

# 计算相关性
r = np.corrcoef(rna_expr.flatten(), protein_expr.flatten())[0, 1]

print(f"Pearson r: {r:.3f}")
```

**使用场景**：
- 对应 Figure：Figure 2 Panel b

---

### Baseline 方法运行

**基本信息**：
- 类型：定量分析
- 用途：运行 Baseline 方法进行比较
- 证明什么：证明 Our method 优于 Baselines

**工具与方法**：
- Seurat v4：R implementation
- MOFA+：Python implementation
- totalVI：scvi-tools

**分析流程**：
```python
# 例如运行 Seurat（需要 R 环境）
# 或使用对应的 Python 实现

# 运行 Baseline
baseline_output = run_baseline_method(data, method='MOFA+')

# 计算指标
baseline_ari = adjusted_rand_score(true_labels, baseline_output.labels)
```

**使用场景**：
- 对应 Figure：Figure 2-5 Panel b

---

## 定性分析方法

### 空间分布可视化 (Spatial Scatter Plot)

**基本信息**：
- 类型：定性分析
- 用途：展示整合后的空间 domain 分布
- 证明什么：证明整合后空间结构清晰

**工具与方法**：
- OmicsClaw Skill：spatial-domains（输出包含可视化）
- Bioclaw 参考：single-cell-and-spatial/spatial-domains/SKILL.md
- 推荐工具：Scanpy
- 关键函数：sc.pl.spatial
- 参数建议：
  - palette='tab20'（避免颜色混淆）
  - size=1.5（适当大小）

**分析流程**：
```python
import scanpy as sc

# 空间分布图
sc.pl.spatial(adata, 
              color='leiden', 
              palette='tab20',
              size=1.5,
              title='Spatial Domain Distribution')

# 保存图片
sc.pl.spatial(adata, color='leiden', palette='tab20', 
              save='spatial_domain.png')
```

**输入输出**：
- 输入：adata（包含 cluster labels 和空间坐标）
- 输出：空间分布图（PNG/PDF）

**使用场景**：
- 适用任务：所有任务
- 对应 Figure：Figure 2-5 Panel c

**注意事项**：
- 颜色选择：tab20 或其他多色 palette
- 避免颜色混淆（14 类别不要共用 9 色）
- 尺寸适当，清晰可见

**来源文献**：
- 已有多少论文使用：所有论文

---

### Marker 展示 (Feature Plot / Violin Plot)

**基本信息**：
- 类型：定性分析
- 用途：展示关键 marker 基因表达
- 证明什么：证明整合保留了生物学意义

**工具与方法**：
- OmicsClaw Skill：sc-markers
- Bioclaw 参考：single-cell-and-spatial/sc-markers/SKILL.md
- 推荐工具：Scanpy
- 关键函数：sc.pl.spatial / sc.pl.violin

**分析流程**：
```python
# Feature plot（空间 marker 展示）
sc.pl.spatial(adata, 
              color=['EPCAM', 'CD3D', 'MS4A1', 'COL1A1'],
              size=1.5,
              cmap='viridis')

# Violin plot（不同 cluster 的 marker 表达）
sc.pl.violin(adata, 
             ['EPCAM', 'CD3D', 'MS4A1'],
             groupby='leiden',
             rotation=45)
```

**输入输出**：
- 输入：adata + marker genes list
- 输出：Feature plot / Violin plot

**使用场景**：
- 对应 Figure：Figure 2-5 Panel d

**注意事项**：
- marker 选择：根据研究领域选择关键 marker
- 验证：marker 表达模式应与已知 cell type 一致

---

### Latent Space 可视化 (UMAP)

**基本信息**：
- 类型：定性分析
- 用途：展示 latent representation 结构
- 证明什么：证明整合质量优良

**工具与方法**：
- 推荐工具：Scanpy
- 关键函数：sc.tl.umap / sc.pl.umap

**分析流程**：
```python
# 计算 UMAP
sc.tl.umap(adata)

# 可视化
sc.pl.umap(adata, color='leiden', palette='tab20')

# 按模态分组（可选）
sc.pl.umap(adata, color='modality')
```

**使用场景**：
- 对应 Figure：Figure 2-5 Panel e

---

### 配准对比展示 (Before/After)

**基本信息**：
- 类型：定性分析（适用于 Task 2）
- 用途：展示配准前后对比
- 证明什么：证明配准方法有效

**工具与方法**：
- 推荐工具：matplotlib
- 方法：Before/after side-by-side

**分析流程**：
```python
import matplotlib.pyplot as plt

fig, axes = plt.subplots(1, 2, figsize=(12, 5))

# Before
axes[0].scatter(slice1_x, slice1_y, c='blue', label='Slice 1')
axes[0].scatter(slice2_x_raw, slice2_y_raw, c='red', label='Slice 2')
axes[0].set_title('Before Registration')

# After
axes[1].scatter(slice1_x, slice1_y, c='blue', label='Slice 1')
axes[1].scatter(slice2_x_aligned, slice2_y_aligned, c='red', label='Slice 2')
axes[1].set_title('After Registration')

plt.legend()
plt.savefig('registration_comparison.png')
```

**使用场景**：
- 对应 Figure：Figure 3 Panel c

---

## 生物学分析方法

### Cell Type Annotation

**基本信息**：
- 类型：生物学分析
- 用途：注释 cell type
- 证明什么：证明方法能识别生物学意义明确的 cell type

**工具与方法**：
- OmicsClaw Skill：spatial-annotate
- Bioclaw 参考：single-cell-and-spatial/spatial-annotate/SKILL.md
- 推荐工具：CellTypist / SingleR / marker-based

**分析流程**：
```python
# 使用 CellTypist
import celltypist

predictions = celltypist.predict(adata, model='Immune_All_Low.pkl')
adata.obs['cell_type'] = predictions.predicted_labels

# 可视化
sc.pl.spatial(adata, color='cell_type', palette='tab20')
```

**使用场景**：
- 可用于所有 Figure 的补充分析

---

### Pathway Enrichment (GSEA)

**基本信息**：
- 类型：生物学分析
- 用途：通路富集分析
- 证明什么：发现生物学 pathway

**工具与方法**：
- OmicsClaw Skill：spatial-enrichment
- 推荐工具：gseapy

**分析流程**：
```python
import gseapy as gp

# 准备 gene list
gene_list = markers['gene'].tolist()

# GSEA
enr = gp.enrichr(gene_list=gene_list,
                 gene_sets='KEGG_2021_Human',
                 outdir='enrichr_results')

# 可视化
gp.barplot(enr.res2d, title='Pathway Enrichment')
```

**使用场景**：
- Supplementary Figure 或 Discussion

---

### Spatial Statistics (Moran's I)

**基本信息**：
- 类型：生物学分析
- 用途：空间自相关分析
- 证明什么：发现空间可变基因

**工具与方法**：
- OmicsClaw Skill：spatial-statistics
- 推荐工具：squidpy

**分析流程**：
```python
import squidpy as sq

# 计算 Moran's I
sq.gr.spatial_neighbors(adata)
sq.gr.spatial_autocorr(adata, mode='moran')

# 查看结果
adata.uns['moranI'].head(10)

# 可视化 top spatial genes
sq.pl.spatial_scatter(adata, color=adata.uns['moranI'].head(5).index)
```

**使用场景**：
- Supplementary Figure

---

## 每个Figure的分析安排

### Figure 1（算法介绍）
| Panel | 分析内容 | 工具 | 说明 |
|-------|---------|------|------|
| a | 算法流程图 | draw.io / matplotlib | 展示方法架构 |
| b | 数据介绍 | Gemini 图文生成 | 各模态数据示意 |
| c | 任务介绍 | Gemini 图文生成 | 任务层级示意 |
| d | 指标介绍 | Gemini 图文生成 | 指标含义示意 |
| e | 分析介绍 | Gemini 图文生成 | 分析流程示意 |

### Figure 2（Task 1 应用）
| Panel | 分析内容 | 工具 | 证明什么 |
|-------|---------|------|---------|
| a | 数据流描述 | 流程图 | 方法工作流程 |
| b | ARI + Pearson r | sklearn + numpy | 定量整合质量 |
| c | Spatial domain | Scanpy.pl.spatial | 空间结构清晰 |
| d | Marker展示 | Scanpy.pl.spatial/violin | 生物学意义保留 |
| e | Latent UMAP | Scanpy.pl.umap | 整合质量优良 |

### Figure 3（Task 2 应用）
| Panel | 分析内容 | 工具 | 证明什么 |
|-------|---------|------|---------|
| a | 配准流程 | 流程图 | 配准方法 |
| b | MSE + ARI | numpy + sklearn | 定量配准质量 |
| c | 配准前后对比 | matplotlib | 配准效果 |
| d | 整合后 domain | Scanpy.pl.spatial | 整合效果 |
| e | Marker展示 | Scanpy | 生物学意义 |

### Figure 4（Task 3 应用）
| Panel | 分析内容 | 工具 | 证明什么 |
|-------|---------|------|---------|
| a | 推断流程 | 流程图 | 推断方法 |
| b | Pearson r + ARI | numpy + sklearn | 定量推断质量 |
| c | 推断后 domain | Scanpy.pl.spatial | 推断效果 |
| d | Marker展示 | Scanpy | 推断保留生物学意义 |
| e | 推断 vs 真实 | matplotlib | 推断准确性 |

### Figure 5（Task 4 应用）
| Panel | 分析内容 | 工具 | 证明什么 |
|-------|---------|------|---------|
| a | 对角整合流程 | 流程图 | 方法创新 |
| b | 综合指标 | sklearn + numpy | 整体质量 |
| c | 空间 domain | Scanpy.pl.spatial | 整合效果 |
| d | Marker + Pathway | Scanpy + gseapy | 生物学发现 |
| e | 新发现展示 | 自定义 | 创新性突出 |

---

## OmicsClaw/Bioclaw 联动汇总

| 分析任务 | OmicsClaw Skill | Bioclaw 参考 | 如果没有 |
|---------|----------------|-------------|---------|
| 职类 | spatial-domains | spatial-domains/SKILL.md | Scanpy.tl.leiden |
| 注释 | spatial-annotate | spatial-annotate/SKILL.md | CellTypist |
| Marker | sc-markers | sc-markers/SKILL.md | Scanpy.pl.spatial |
| 富集 | spatial-enrichment | spatial-enrichment/SKILL.md | gseapy |
| 空间统计 | spatial-statistics | spatial-statistics/SKILL.md | squidpy |
| GRN | sc-grn | sc-grn/SKILL.md | pySCENIC |
| 通讯 | spatial-communication | spatial-communication/SKILL.md | CellChat |
| 轨迹 | spatial-trajectory | spatial-trajectory/SKILL.md | CellRank |

---

## 下一步
- 根据分析方法体系，设计每个 Figure（Step 6）
```

## 使用方式

```bash
/bio-analysis-system "空间多组学整合 | paper_count: 5 | task_system: [...] | metric_system: [...]"
```

## 注意事项

1. **OmicsClaw/Bioclaw 优先**：有现成 skill 就用
2. **证明什么**：每个分析都要说明证明什么生物学结论
3. **工具明确**：标注工具和函数
4. **参数建议**：提供参数建议，方便后续执行
5. **颜色选择**：避免颜色混淆（tab20 > Set1）