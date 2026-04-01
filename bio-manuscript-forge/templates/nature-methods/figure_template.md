# Figure 设计模板

## Figure 1: 算法流程 + 体系介绍

**尺寸**：双栏宽度 (183mm) × 高度 (120mm)

**布局**：5 panels

```
┌────────────────────────────────────────────┐
│ Panel a: 算法流程 (左上, 80mm × 60mm)       │
│                                             │
│  [原有方法（灰色）]                          │
│  [用户新增（红色）]                          │
│                                             │
└────────────────────────────────────────────┘

┌─────────────────────┐  ┌─────────────────────┐
│ Panel b: 数据介绍    │  │ Panel c: 任务介绍    │
│ (60mm × 80mm)       │  │ (60mm × 80mm)       │
│                     │  │                     │
│ [数据类型列表]       │  │ [任务层级列表]       │
│ - 类型A + 图        │  │ - Task 1 + 图       │
│ - 类型B + 图        │  │ - Task 2 + 图       │
│ - 类型C + 图        │  │ - Task 3 + 图       │
│                     │  │ - Task 4 + 图       │
└─────────────────────┘  └─────────────────────┘

┌─────────────────────┐  ┌─────────────────────┐
│ Panel d: 指标介绍    │  │ Panel e: 分析介绍    │
│ (60mm × 80mm)       │  │ (60mm × 80mm)       │
│                     │  │                     │
│ [指标列表]          │  │ [分析方法列表]       │
│ - 指标A + 图        │  │ - 分析A + 图        │
│ - 指标B + 图        │  │ - 分析B + 图        │
│ - 指标C + 图        │  │ - 分析C + 图        │
│                     │  │                     │
└─────────────────────┘  └─────────────────────┘
```

---

## Figure 2-5: 真实应用

**尺寸**：双栏宽度 (183mm) × 高度 (150mm)

**布局**：5 panels (2 列)

```
┌────────────────────────────────────────────┐
│ Panel a: 数据流 (全宽, 183mm × 40mm)        │
│                                             │
│  Input → Preprocess → Method → Output       │
│     ↓                                       │
│  Analysis                                   │
│                                             │
└────────────────────────────────────────────┘

┌─────────────────────┐  ┌─────────────────────┐
│ Panel b: 定量评价    │  │ Panel c: 空间分布    │
│ (89mm × 80mm)       │  │ (89mm × 80mm)       │
│                     │  │                     │
│ [Bar chart]         │  │ [Spatial plot]      │
│ - 指标比较          │  │ - Domain分布        │
│ - 统计显著性        │  │ - tab20颜色         │
│                     │  │                     │
└─────────────────────┘  └─────────────────────┘

┌─────────────────────┐  ┌─────────────────────┐
│ Panel d: Marker展示  │  │ Panel e: 更多分析    │
│ (89mm × 80mm)       │  │ (89mm × 80mm)       │
│                     │  │                     │
│ [Feature plot]      │  │ [UMAP / 其他]       │
│ - 关键marker        │  │ - Latent可视化      │
│ - 验证生物学意义    │  │ - 或其他分析        │
│                     │  │                     │
└─────────────────────┘  └─────────────────────┘
```

---

## Panel 设计规范

### Panel a: 流程图

**元素**：
- 矩形框：模块
- 箭头：数据流
- 颜色：灰色（原有）vs 红色（新增）
- 字体：Arial 8-10pt

**工具**：draw.io, matplotlib, BioRender

---

### Panel b: Bar Chart

**元素**：
- X轴：方法名称
- Y轴：指标值
- Error bar：±1 std
- 显著性标注：* p<0.05, ** p<0.01, *** p<0.001

**颜色**：
- Our method: 红色或深色
- Baselines: 灰色或浅色

**工具**：matplotlib, seaborn

```python
import matplotlib.pyplot as plt
import numpy as np

methods = ['Our method', 'Baseline 1', 'Baseline 2']
values = [0.85, 0.72, 0.68]
errors = [0.02, 0.03, 0.02]

colors = ['#D62728', '#7F7F7F', '#7F7F7F']

plt.bar(methods, values, yerr=errors, color=colors, capsize=5)
plt.ylabel('ARI')
plt.title('Performance Comparison')

# 显著性标注
plt.annotate('**', xy=(0, 0.88), ha='center', fontsize=10)

plt.tight_layout()
plt.savefig('figure2b.png', dpi=300)
```

---

### Panel c: Spatial Plot

**元素**：
- 空间位置：X, Y 坐标
- 颜色：Domain label（tab20 palette）
- 尺寸：适当大小，清晰可见

**颜色选择**：
- 推荐：tab20, tab20b, tab20c
- 避免：Set1（9色，不够14类别）

**工具**：Scanpy

```python
import scanpy as sc

sc.pl.spatial(adata, 
              color='domain',
              palette='tab20',
              size=1.5,
              title='Spatial Domain Distribution')
```

---

### Panel d: Feature Plot

**元素**：
- 空间位置：X, Y 坐标
- 颜色：表达量（viridis 或 YlOrRd）
- Marker 选择：关键 marker genes

**工具**：Scanpy

```python
import scanpy as sc

sc.pl.spatial(adata,
              color=['EPCAM', 'CD3D', 'MS4A1', 'COL1A1'],
              cmap='viridis',
              size=1.5,
              ncols=2)
```

---

### Panel e: UMAP

**元素**：
- UMAP 坐标
- 颜色：Cluster label
- 分离程度：反映整合质量

**工具**：Scanpy

```python
import scanpy as sc

sc.tl.umap(adata)
sc.pl.umap(adata, 
           color='domain',
           palette='tab20',
           title='Latent Space UMAP')
```

---

## 颜色规范

### 推荐调色板

| 用途 | 调色板 | 说明 |
|------|--------|------|
| 职类/Domain | tab20 | 20色，适合多类别 |
| 表达量 | viridis | 连续值，色盲友好 |
| Bar chart | 自定义 | Our method 高亮 |
| 差异表达 | RdBu | 发散型 |

### 颜色选择原则

1. **色盲友好**：避免红绿对比
2. **类别数量**：tab20 最多 20 类
3. **突出重点**：Our method 用深色/红色
4. **一致性**：同一 Figure 内颜色含义一致

---

## 图注模板

```markdown
> **Figure X: [Title].**
> 
> a) [Panel a 描述]。[详细说明数据流或方法架构]。
> 
> b) [Panel b 描述]。[指标名称]: Our method ([数值]) vs Baseline 1 ([数值]) vs Baseline 2 ([数值])。Statistical significance: p < 0.01 (paired t-test). Error bars: ±1 std over [n] runs.
> 
> c) [Panel c 描述]。[空间domain数量] domains identified. Domain colors: tab20 palette.
> 
> d) [Panel d 描述]。Key markers: [marker A] ([cell type A]), [marker B] ([cell type B]), [marker C] ([cell type C]). Expression patterns align with known cell type distribution.
> 
> e) [Panel e 描述]。[分析结果描述]。
```

---

## 输出格式

- **格式**：TIFF / EPS / PDF
- **分辨率**：300 dpi（位图），矢量图（线条图）
- **颜色模式**：RGB（在线），CMYK（印刷）
- **文件命名**：FigureX.tif, FigureX_panelA.tif