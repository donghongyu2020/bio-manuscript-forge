# bio-figure-design

**Step 6: Figure 详细设计**

设计每个 Figure 的完整结构（每个 Panel），包含图注撰写和图文生成。

## 功能

1. 设计 Figure 1：算法流程 + 数据/任务/指标/分析介绍
2. 设计 Figure 2-5：真实应用（每图一个数据集/任务）
3. 设计 Supplementary Figure
4. 撰写每个 Figure 的图注
5. 生成 Panel b-c-d-e 的图文（调用 Gemini）

## 输入格式

```
topic: [研究主题]
task_system: [任务体系]
dataset_catalog: [数据集目录]
metric_system: [指标体系]
analysis_system: [分析方法]
target_journal: [目标期刊，默认 nat-communications]
```

## 执行流程

### Step 6.1: Figure 1 设计

**Figure 1：算法流程 + 体系介绍**

```
Figure 1 结构（5 panels，约 180mm × 120mm）：

┌──────────────────────────────────────────────────────┐
│ Panel a: 算法流程图                                    │
│                                                        │
│  【原有方法】（灰色/虚线）                              │
│                                                        │
│  Input → Encoder → Module A → Module B → Decoder      │
│                                                        │
│  【用户创新】（红色/实线）                              │
│                                                        │
│  Input → Encoder → Module A → [NEW Module C] →        │
│          Module B → Decoder                           │
│                                                        │
│  （标注：原有模块灰色框，新增模块红色框，箭头数据流）    │
│                                                        │
└──────────────────────────────────────────────────────┘

┌─────────────────────┐  ┌─────────────────────┐
│ Panel b: 数据介绍     │  │ Panel c: 任务介绍     │
│                      │  │                      │
│ ┌─────────────────┐ │  │ ┌─────────────────┐ │
│ │ Proteomics      │ │  │ │ Vertical Integ. │ │
│ │ [示意图]        │ │  │ │ [示意图]        │ │
│ │ 蛋白表达测量     │ │  │ │ 同细胞多模态    │ │
│ └─────────────────┘ │  │ └─────────────────┘ │
│                      │  │                      │
│ ┌─────────────────┐ │  │ ┌─────────────────┐ │
│ │ Transcriptomics │ │  │ │ Horizontal Int. │ │
│ │ [示意图]        │ │  │ │ [示意图]        │ │
│ │ 转录组表达      │ │  │ │ 切片配准整合    │ │
│ └─────────────────┘ │  │ └─────────────────┘ │
│                      │  │                      │
│ ┌─────────────────┐ │  │ ┌─────────────────┐ │
│ │ Epigenomics     │ │  │ │ Mosaic Integ.   │ │
│ │ [示意图]        │ │  │ │ [示意图]        │ │
│ │ 表观遗传修饰     │ │  │ │ 缺失模态推断   │ │
│ └─────────────────┘ │  │ └─────────────────┘ │
│                      │  │                      │
│ （Gemini生成图文）   │  │ │ ┌─────────────────┐ │
│                      │  │ │ Diagonal Integ. │ │
│                      │  │ │ [示意图]        │ │
│                      │  │ │ 跨平台跨分辨率  │ │
│                      │  │ └─────────────────┘ │
└─────────────────────┘  └─────────────────────┘

┌─────────────────────┐  ┌─────────────────────┐
│ Panel d: 指标介绍     │  │ Panel e: 分析介绍     │
│                      │  │                      │
│ ┌─────────────────┐ │  │ ┌─────────────────┐ │
│ │ ARI             │ │  │ │ Clustering      │ │
│ │ [示意图]        │ │  │ │ [示意图]        │ │
│ │ 职类一致性       │ │  │ │ Leiden聚类     │ │
│ └─────────────────┘ │  │ └─────────────────┘ │
│                      │  │                      │
│ ┌─────────────────┐ │  │ ┌─────────────────┐ │
│ │ Pearson r       │ │  │ │ Marker Vis.     │ │
│ │ [示意图]        │ │  │ │ [示意图]        │ │
│ │ 模态相关性       │ │  │ │ Marker展示     │ │
│ └─────────────────┘ │  │ └─────────────────┘ │
│                      │  │                      │
│ ┌─────────────────┐ │  │ ┌─────────────────┐ │
│ │ MSE             │ │  │ │ Spatial Stat.   │ │
│ │ [示意图]        │ │  │ │ [示意图]        │ │
│ │ 重建/配准误差    │ │  │ │ 空间统计       │ │
│ └─────────────────┘ │  │ └─────────────────┘ │
│                      │  │                      │
│ （Gemini生成图文）   │  │ （Gemini生成图文）   │
└─────────────────────┘  └─────────────────────┘
```

**Panel a 设计要点**：
```
Panel a: 算法流程图

设计原则：
1. 清晰展示数据流
2. 原有模块 vs 新增模块对比
3. 模块名称标注
4. 输入输出明确

绘制方式：
- 使用 draw.io 或 matplotlib
- 原有模块：灰色/虚线框
- 新增模块：红色/实线框
- 箭头：数据流向
- 标注：模块名称、输入输出类型

尺寸：约 80mm × 60mm（左上角）
```

**Panel b-c-d-e 图文生成**：
```
调用 Gemini/NanoBanana 生成图文：

Prompt 结构：
"生成一个简洁的科学论文插图，用于 Figure 1 Panel b，
展示 [数据类型] 的测量原理。
风格：Nature Methods 论文插图，简洁清晰，配色专业。
包含：数据名称标签 + 测量示意图 + 简短描述（1句话）"

生成内容：
- 数据名称（如 Proteomics）
- 测量示意图（如蛋白检测示意）
- 简短描述（如"测量蛋白质表达水平"）
- 框框起来（统一视觉风格）
```

### Step 6.2: Figure 2-5 设计

**每个 Figure 结构（真实应用）**：

```
Figure 2-5 结构（每个 Figure 5 panels）：

通用模板：

┌──────────────────────────────────────────────────────┐
│ Panel a: 数据流详细描述                                │
│                                                        │
│  ┌───────────────────────────────────────────────┐   │
│  │                                                │   │
│  │  Input Data (Dataset X)                       │   │
│  │  ├─ Type: RNA + Protein                       │   │
│  │  ├─ Samples: 50,000 cells                     │   │
│  │  └─ Source: Xenium Breast Cancer              │   │
│  │                                                │   │
│  │  ↓ Preprocessing (OmicsClaw: spatial-preproc) │   │
│  │  ├─ QC + Normalization                        │   │
│  │  └─ HVG selection                             │   │
│  │                                                │   │
│  │  ↓ Our Method                                 │   │
│  │  ├─ Encoder                                   │   │
│  │  ├─ [Core Module]                             │   │
│  │  └─ Decoder                                   │   │
│  │                                                │   │
│  │  ↓ Output                                     │   │
│  │  ├─ Latent representation                     │   │
│  │  └─ Domain labels                             │   │
│  │                                                │   │
│  │  ↓ Analysis                                   │   │
│  │  ├─ Clustering (Leiden)                       │   │
│  │  ├─ Metrics (ARI, Pearson r)                  │   │
│  │  ├─ Visualization (Spatial, Marker)           │   │
│  │                                                │   │
│  └───────────────────────────────────────────────┘   │
│                                                        │
│  （流程图形式，清晰展示每个步骤）                        │
│                                                        │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│ Panel b: 定量评价                                      │
│                                                        │
│  Bar chart / Table:                                   │
│                                                        │
│  ┌───────────────────────────────────────────────┐   │
│  │                                                │   │
│  │  ARI Comparison                               │   │
│  │                                                │   │
│  │  Our method     ████████████████  0.85       │   │
│  │  Baseline 1     ██████████        0.72       │   │
│  │  Baseline 2     ████████          0.68       │   │
│  │                                                │   │
│  │  Pearson r Comparison                         │   │
│  │                                                │   │
│  │  Our method     ████████████████████  0.91  │   │
│  │  Baseline 1     ████████████        0.78    │   │
│  │  Baseline 2     ██████████          0.75    │   │
│  │                                                │   │
│  │  * p < 0.01, paired t-test                   │   │
│  │                                                │   │
│  └───────────────────────────────────────────────┘   │
│                                                        │
│  （Bar chart + 统计显著性标注）                         │
│                                                        │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│ Panel c: 空间分布                                      │
│                                                        │
│  Spatial Scatter Plot:                                │
│                                                        │
│  ┌───────────────────────────────────────────────┐   │
│  │                                                │   │
│  │  [空间分布图]                                 │   │
│  │                                                │   │
│  │  不同颜色代表不同 domain                      │   │
│  │  （tab20 palette）                            │   │
│  │                                                │   │
│  │  Domain 1 (红) - Epithelial                  │   │
│  │  Domain 2 (蓝) - T cells                     │   │
│  │  Domain 3 (绿) - B cells                     │   │
│  │  Domain 4 (黄) - Fibroblasts                 │   │
│  │  ...                                          │   │
│  │                                                │   │
│  │  空间结构清晰，边界明确                       │   │
│  │                                                │   │
│  └───────────────────────────────────────────────┘   │
│                                                        │
│  （Scanpy.pl.spatial，palette='tab20'）               │
│                                                        │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│ Panel d: Marker 展示                                   │
│                                                        │
│  Feature Plot / Violin Plot:                          │
│                                                        │
│  ┌───────────────────────────────────────────────┐   │
│  │                                                │   │
│  │  [Feature Plot - EPCAM]    [Feature Plot - CD3D] │
│  │                                                │   │
│  │  EPCAM 高表达于 Domain 1     CD3D 高表达于 Domain 2 │
│  │  （上皮细胞 marker）          （T细胞 marker）   │   │
│  │                                                │   │
│  │  [Feature Plot - MS4A1]    [Feature Plot - COL1A1] │
│  │                                                │   │
│  │  MS4A1 高表达于 Domain 3     COL1A1 高表达于 Domain 4 │
│  │  （B细胞 marker）            （纤维细胞 marker）│   │
│  │                                                │   │
│  └───────────────────────────────────────────────┘   │
│                                                        │
│  （验证生物学意义保留）                                 │
│                                                        │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│ Panel e: 更多分析                                      │
│                                                        │
│  UMAP / 其他分析:                                      │
│                                                        │
│  ┌───────────────────────────────────────────────┐   │
│  │                                                │   │
│  │  [UMAP Plot]                                  │   │
│  │                                                │   │
│  │  不同 cluster 在 latent space 中分布         │   │
│  │  分离良好，证明整合质量优良                   │   │
│  │                                                │   │
│  │  或其他分析：                                 │   │
│  │  ├─ 配准前后对比（Task 2）                   │   │
│  │  ├─ 推断 vs 真实（Task 3）                   │   │
│  │  ├─ Pathway enrichment（Task 4）             │   │
│  │                                                │   │
│  └───────────────────────────────────────────────┘   │
│                                                        │
│  （根据任务特点选择）                                   │
│                                                        │
└──────────────────────────────────────────────────────┘
```

### Step 6.3: Supplementary Figure 设计

```
Supplementary Figure 内容：

Supplementary Figure 1-5:
├─ Supp Fig 1: 详细参数测试
│   ├─ 参数敏感性分析
│   ├─ 不同 resolution 的影响
│   └─ 不同 hidden dimension 的影响
│
├─ Supp Fig 2: 更多数据集结果
│   ├─ Dataset X 的完整分析
│   ├─ Dataset Y 的完整分析
│   └─ ...
│
├─ Supp Fig 3: 更多 Baseline 比较
│   ├─ Baseline 3, 4, 5 的比较
│   ├─ 不同指标的比较
│   └─ ...
│
├─ Supp Fig 4: 方法细节展示
│   ├─ Loss curve
│   ├─ Attention heatmap
│   ├─ Latent dimension visualization
│   └─ ...
│
├─ Supp Fig 5: 失败案例分析（诚实展示）
│   ├─ 方法在哪些情况下表现不佳
│   ├─ 原因分析
│   └─ 改进建议
│
└─ Supplementary Table 1-5:
    ├─ Supp Table 1: 参数汇总
    ├─ Supp Table 2: 数据集汇总
    ├─ Supp Table 3: Baseline 汇总
    ├─ Supp Table 4: 指标汇总
    └─ Supp Table 5: 代码链接
```

### Step 6.4: 图注撰写

**图注模板**：

```markdown
> **Figure X: [Figure 标题].**
> 
> [Panel a 描述]
> a) [Panel a 详细描述]。
> 
> [Panel b 描述]
> b) [Panel b 详细描述，包含数值、统计检验]。
> 
> [Panel c 描述]
> c) [Panel c 详细描述]。
> 
> [Panel d 描述]
> d) [Panel d 详细描述]。
> 
> [Panel e 描述]
> e) [Panel e 详细描述]。
```

**示例（Figure 2）**：
```markdown
> **Figure 2: Vertical integration on Xenium Breast Cancer dataset.**
> 
> a) Data processing workflow. Input: Xenium RNA + Protein data (50,000 cells).
> Preprocessing: QC, normalization, HVG selection (OmicsClaw: spatial-preprocess).
> Our method: Cross-Attention + Encoder-Decoder architecture.
> Output: Integrated latent representation + Domain labels.
> Analysis: Clustering (Leiden), Metrics (ARI, Pearson r), Visualization.
> 
> b) Quantitative evaluation. ARI: Our method (0.85) vs Baseline 1 (0.72) vs Baseline 2 (0.68).
> Pearson r: Our method (0.91) vs Baseline 1 (0.78) vs Baseline 2 (0.75).
> Statistical significance: p < 0.01 (paired t-test). Error bars: ±1 std over 5 runs.
> 
> c) Spatial domain visualization. Our method identifies 8 distinct domains.
> Clear spatial structure with well-defined boundaries.
> Domain colors: tab20 palette.
> 
> d) Marker gene expression. Key markers: EPCAM (epithelial, Domain 1),
> CD3D (T cells, Domain 2), MS4A1 (B cells, Domain 3), COL1A1 (fibroblasts, Domain 4).
> Expression patterns align with known cell type distribution.
> 
> e) Latent space UMAP. Our method's latent representation shows clear
> separation between cell types, indicating successful integration.
```

## 输出格式

每个 Figure 输出独立文件：

```markdown
# Figure X Design

## Figure Overview
- **Title**: [Figure 标题]
- **Goal**: [Figure 目的]
- **Size**: [尺寸，如 180mm × 120mm]
- **Layout**: [布局，如 5 panels, 2列 × 3行]

## Panel a: [Panel 名称]

### Purpose
- [Panel 目的]

### Content
- [详细内容描述]

### Visual Design
- **Type**: [图表类型，如流程图/bar chart/spatial plot]
- **Size**: [尺寸]
- **Colors**: [颜色方案]
- **Annotations**: [标注内容]

### Tools
- **绘制工具**: [工具名称]
- **代码**: [代码片段]

### Expected Result
- [预期结果描述]

---

## Panel b: [Panel 名称]

### Purpose
- 定量评价

### Content
- ARI 比较 + Pearson r 比较

### Visual Design
- **Type**: Bar chart
- **Layout**: 2 grouped bar charts
- **Error bars**: ±1 std
- **Statistical test**: p-value annotation

### Tools
- matplotlib / seaborn

### Code
```python
import matplotlib.pyplot as plt
import seaborn as sns

# 绘制 bar chart
methods = ['Our method', 'Baseline 1', 'Baseline 2']
ari_values = [0.85, 0.72, 0.68]
ari_std = [0.02, 0.03, 0.02]

plt.bar(methods, ari_values, yerr=ari_std, capsize=5)
plt.ylabel('ARI')
plt.title('ARI Comparison')
plt.savefig('figure2b.png')
```

---

## Panel c: 空间分布

### Purpose
- 展示整合后的空间结构

### Content
- Spatial domain plot

### Visual Design
- **Type**: Spatial scatter plot
- **Palette**: tab20
- **Size**: 1.5
- **Annotations**: Domain labels

### Tools
- OmicsClaw: spatial-domains
- Scanpy: sc.pl.spatial

### Code
```python
import scanpy as sc
sc.pl.spatial(adata, color='leiden', palette='tab20', size=1.5)
```

---

## Panel d: Marker 展示

### Purpose
- 验证生物学意义保留

### Content
- Feature plot of key markers

### Visual Design
- **Type**: Feature plot
- **Markers**: EPCAM, CD3D, MS4A1, COL1A1
- **Cmap**: viridis

### Tools
- Scanpy: sc.pl.spatial

### Code
```python
sc.pl.spatial(adata, color=['EPCAM', 'CD3D', 'MS4A1', 'COL1A1'], 
              cmap='viridis', size=1.5)
```

---

## Panel e: Latent UMAP

### Purpose
- 展示 latent representation 质量

### Content
- UMAP plot

### Visual Design
- **Type**: UMAP scatter plot
- **Palette**: tab20

### Tools
- Scanpy: sc.tl.umap, sc.pl.umap

### Code
```python
sc.tl.umap(adata)
sc.pl.umap(adata, color='leiden', palette='tab20')
```

---

## Figure Legend

> **Figure X: [完整图注]**
> 
> a) ...
> b) ...
> c) ...
> d) ...
> e) ...

---

## Dependencies
- **Data source**: Dataset X
- **Analysis tools**: OmicsClaw, Scanpy, matplotlib
- **Color palette**: tab20
- **Statistical test**: scipy.stats.ttest_rel
```

## Gemini 图文生成

**调用 Gemini 生成 Panel b-c-d-e 的图文**：

```python
# Panel b: 数据介绍
prompt_b = """
生成一个简洁的科学论文插图，用于 Figure 1 Panel b，
展示以下数据类型的测量原理：

1. Proteomics - 蛋白质表达测量
2. Transcriptomics - 转录组表达测量
3. Epigenomics - 表观遗传修饰测量

要求：
- 每种数据一个框框
- 框框包含：数据名称标签 + 测量示意图 + 简短描述
- 风格：Nature Methods 论文插图
- 配色：专业科学论文配色
- 尺寸：适合 Figure 1 Panel b（约 60mm × 80mm）
"""

# Panel c: 任务介绍
prompt_c = """
生成任务层级示意图，用于 Figure 1 Panel c：
- Vertical Integration（同细胞多模态）
- Horizontal Integration（切片配准）
- Mosaic Integration（缺失推断）
- Diagonal Integration（跨平台跨分辨率）

每个任务一个框框，包含任务名称 + 示意图 + 简短描述。
难度递进（从上到下）。
"""

# Panel d: 指标介绍
prompt_d = """
生成评价指标示意图，用于 Figure 1 Panel d：
- ARI（聚类一致性）
- Pearson Correlation（模态相关性）
- MSE（重建误差）

每个指标一个框框，包含指标名称 + 含义示意图 + 简短描述。
"""

# Panel e: 分析介绍
prompt_e = """
生成分析方法示意图，用于 Figure 1 Panel e：
- Clustering（Leiden聚类）
- Marker Visualization（Marker展示）
- Spatial Statistics（空间统计）

每个分析一个框框，包含分析名称 + 示意图 + 简短描述。
"""
```

## 使用方式

```bash
# 设计所有 Figure
/bio-figure-design "topic: 空间多组学整合 | task_system: [...] | dataset_catalog: [...] | metric_system: [...] | analysis_system: [...] | target_journal: nat-communications"

# 设计单个 Figure
/bio-figure-design "Figure 2 | Task: 垂直整合 | Dataset: Xenium Breast Cancer"
```

## 注意事项

1. **Figure 1 Panel a**：算法流程图需要清晰展示原有 vs 新增模块
2. **Figure 1 Panel b-c-d-e**：调用 Gemini 生成图文，统一框框风格
3. **Figure 2-5**：每个 Figure 对应一个任务/数据集
4. **Panel a**：数据流详细描述，流程图形式
5. **Panel b**：定量评价必须有统计显著性标注
6. **Panel c**：空间分布图颜色选择 tab20
7. **Panel d**：Marker 展示验证生物学意义
8. **Panel e**：根据任务特点选择内容
9. **图注**：每个 Panel 都要详细描述