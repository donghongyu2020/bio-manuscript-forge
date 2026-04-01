# bio-metric-system

**Step 4: 评价指标体系构建**

构建定量和定性评价指标体系，从已有工作或相邻领域借用。

## 功能

1. 从已有工作提取评价指标
2. 或向相邻领域借用指标
3. 分类：定量指标 + 定性指标
4. 说明每个指标的用途和计算方法

## 输入格式

```
topic: [研究主题]
paper_count: [已有工作数量]
task_system: [任务体系，从 Step 2 获得]
```

## 执行流程

### Step 4.1: 从已有工作提取指标

**条件**：paper_count >= 5

**策略**：
```
for paper in top_papers:
    # 阅读论文 Results / Benchmark 部分
    metrics = extract_metrics_from_paper(paper)
    
    # 提取内容：
    # - 指标名称（ARI、NMI、Pearson r、MSE）
    # - 指标用途（评估什么）
    # - 计算方法（公式/工具）
    # - 指标范围（0-1 / 其他）
    # - 使用频率（多少论文用了这个指标）

# 汇总指标
metric_system = aggregate_metrics(metrics)

# 指标分类
classify_metrics(metric_system)
```

### Step 4.2: 向相邻领域借用指标

**条件**：paper_count < 5

**策略**：
```
# 找相邻领域
adjacent_domain = find_adjacent_domain(topic)
# 例如：空间多组学整合 → 单细胞多组学整合

# 搜索相邻领域的指标
adjacent_metrics = search_metrics_in_domain(adjacent_domain)

# 适配指标
# 例如：单细胞多组学的 ARI → 空间多组学的 ARI
# （可能需要考虑空间邻居）

adapted_metrics = adapt_metrics_to_domain(adjacent_metrics, topic)
```

### Step 4.3: 指标分类

**分类框架**：

```
指标分类：
├─ 定量指标（数值可比较）
│   ├─ 整合质量指标
│   │   ├─ ARI（聚类一致性）
│   │   ├─ NMI（聚类一致性）
│   │   ├─ Silhouette score（聚类质量）
│   │   └─ Adjusted Fowlkes-Mallows index
│   │
│   ├─ 模态一致性指标
│   │   ├─ Pearson correlation（模态间相关性）
│   │   ├─ MSE（重建误差）
│   │   ├─ MAE（重建误差）
│   │   ├─ Cosine similarity（latent相似性）
│   │   └─ R²（重建质量）
│   │
│   ├─ 配准准确度指标（如果需要）
│   │   ├─ MSE（配准误差）
│   │   ├─ SSD（相似度）
│   │   └─ Registration accuracy
│   │
│   └─ 生物意义指标
│   │   ├─ Marker accuracy（marker表达准确性）
│   │   ├─ Annotation accuracy（注释准确性）
│   │   └─ Biological pathway enrichment score
│   │
└─ 定性指标（可视化展示）
    ├─ 空间分布图（domain/cell type）
    ├─ 模态对比图
    ├─ Marker展示图（feature plot / violin plot）
    ├─ Latent space可视化（UMAP / t-SNE）
    ├─ 配准对比图（before/after）
    ├─ 热图（correlation / expression）
    └─ 通路富集图
```

### Step 4.4: 指标标准化描述

每个指标需要标准化描述：

```
指标描述模板：

### [指标名称]

**基本信息**：
- 英文名：[English name]
- 中文名：[Chinese name]
- 类型：定量指标 / 定性指标
- 用途：评估整合质量 / 模态一致性 / 生物意义 / ...

**定义与公式**：
- 定义：[一句话定义]
- 公式：[数学公式，如果有]
- 范围：[0-1 / 其他范围]
- 解读：[值越高越好 / 值越低越好 / ...]

**计算方法**：
- 工具：[Python包 / R包]
- 函数：[函数名称]
- 代码：[代码片段]
- 参数：[关键参数]

**使用场景**：
- 适用任务：[Task 1/2/3/4]
- 适用数据：[数据类型]
- 对应 Figure：[Figure X Panel b]

**来源文献**：
- 已有多少论文使用：[数量]
- 代表论文：[论文列表]

**注意事项**：
- [使用注意事项]
```

## 输出格式

```markdown
# 评价指标体系

## 指标来源
- 已有工作提取：[X篇] 论文中的指标
- 相邻领域借用：[相邻领域名称] 的指标

## 定量指标

### 整合质量指标

#### ARI (Adjusted Rand Index)

**基本信息**：
- 英文名：Adjusted Rand Index
- 中文名：调整 Rand 指数
- 类型：定量指标
- 用途：评估聚类一致性

**定义与公式**：
- 定义：衡量两个聚类结果的相似度，校正随机一致性
- 公式：ARI = (RI - Expected_RI) / (max_RI - Expected_RI)
- 范围：-1 到 1，通常 0-1
- 解读：值越高越好，1 表示完美一致

**计算方法**：
- 工具：scikit-learn (Python)
- 函数：sklearn.metrics.adjusted_rand_score
- 代码：
```python
from sklearn.metrics import adjusted_rand_score
ari = adjusted_rand_score(true_labels, predicted_labels)
```
- 参数：true_labels（真实标签），predicted_labels（预测标签）

**使用场景**：
- 适用任务：Task 1, 2, 3, 4（所有任务）
- 适用数据：有真实 label 的数据
- 对应 Figure：Figure 2-5 Panel b

**来源文献**：
- 已有多少论文使用：5篇
- 代表论文：Paper A, Paper B, Paper C

**注意事项**：
- 需要有真实 label 或可信的参考 label
- 对聚类数量敏感

---

#### NMI (Normalized Mutual Information)

**基本信息**：
- 英文名：Normalized Mutual Information
- 中文名：标准化互信息
- 类型：定量指标
- 用途：评估聚类一致性

**定义与公式**：
- 定义：衡量两个聚类结果的互信息
- 公式：NMI = 2 * I(U,V) / (H(U) + H(V))
- 范围：0-1
- 解读：值越高越好

**计算方法**：
- 工具：scikit-learn
- 函数：sklearn.metrics.normalized_mutual_info_score
- 代码：
```python
from sklearn.metrics import normalized_mutual_info_score
nmi = normalized_mutual_info_score(true_labels, predicted_labels)
```

**使用场景**：
- 适用任务：Task 1, 2, 3, 4
- 对应 Figure：Figure 2-5 Panel b

**来源文献**：
- 已有多少论文使用：4篇

---

### 模态一致性指标

#### Pearson Correlation

**基本信息**：
- 英文名：Pearson Correlation Coefficient
- 中文名：Pearson 相关系数
- 类型：定量指标
- 用途：评估模态间表达相关性

**定义与公式**：
- 定义：衡量两个变量线性相关性
- 公式：r = Σ(x_i - x̄)(y_i - ȳ) / √[Σ(x_i - x̄)² Σ(y_i - ȳ)²]
- 范围：-1 到 1
- 解读：值越接近 1 越好（正相关）

**计算方法**：
- 工具：NumPy
- 函数：np.corrcoef
- 代码：
```python
import numpy as np
r = np.corrcoef(x, y)[0, 1]
```

**使用场景**：
- 适用任务：Task 1（垂直整合）
- 适用数据：多模态数据
- 对应 Figure：Figure 2 Panel b

**来源文献**：
- 已有多少论文使用：4篇

---

#### MSE (Mean Squared Error)

**基本信息**：
- 英文名：Mean Squared Error
- 中文名：均方误差
- 类型：定量指标
- 用途：评估重建误差 / 配准误差

**定义与公式**：
- 定义：预测值与真实值差的平方平均
- 公式：MSE = Σ(pred_i - true_i)² / n
- 范围：0 到 ∞
- 解读：值越低越好

**计算方法**：
- 工具：NumPy
- 函数：np.mean
- 代码：
```python
import numpy as np
mse = np.mean((pred - true) ** 2)
```

**使用场景**：
- 适用任务：Task 2（配准）、Task 3（推断）
- 对应 Figure：Figure 3-4 Panel b

---

### 配准准确度指标

（如果任务包含配准）

#### Registration MSE

...

### 生物意义指标

#### Marker Accuracy

**基本信息**：
- 英文名：Marker Gene Expression Accuracy
- 中文名：Marker基因表达准确性
- 类型：定量指标
- 用途：验证整合保留了生物学意义

**定义与公式**：
- 定义：marker基因表达与已知分布的一致性
- 公式：（自定义，通常基于相关性）
- 解读：表达模式与预期一致为好

**计算方法**：
- 工具：自定义 / Scanpy
- 方法：对比 marker 表达分布

**使用场景**：
- 适用任务：所有任务
- 对应 Figure：Figure 2-5 Panel d

---

## 定性指标（可视化）

### 空间分布图 (Spatial Domain Plot)

**基本信息**：
- 类型：定性指标
- 用途：展示整合后的空间结构

**可视化方法**：
- 工具：Scanpy
- 函数：sc.pl.spatial
- 代码：
```python
import scanpy as sc
sc.pl.spatial(adata, color='domain', palette='tab20')
```

**展示内容**：
- 不同颜色代表不同 domain/cell type
- 空间位置清晰
- 边界明确

**使用场景**：
- 适用任务：所有任务
- 对应 Figure：Figure 2-5 Panel c

**注意事项**：
- 颜色选择：tab20（避免类别过多导致颜色混淆）
- 尺寸：适当大小，清晰可见

---

### Marker 展示图 (Feature Plot / Violin Plot)

**基本信息**：
- 类型：定性指标
- 用途：展示关键 marker 表达

**可视化方法**：
- 工具：Scanpy
- 函数：sc.pl.spatial / sc.pl.violin
- 代码：
```python
# Feature plot
sc.pl.spatial(adata, color=['EPCAM', 'CD3D', 'MS4A1'])

# Violin plot
sc.pl.violin(adata, ['EPCAM', 'CD3D', 'MS4A1'], groupby='domain')
```

**展示内容**：
- marker 基因在不同 domain 的表达
- 验证生物学意义

**使用场景**：
- 对应 Figure：Figure 2-5 Panel d

---

### Latent Space 可视化 (UMAP)

**基本信息**：
- 类型：定性指标
- 用途：展示 latent representation 结构

**可视化方法**：
- 工具：Scanpy
- 函数：sc.tl.umap / sc.pl.umap
- 代码：
```python
sc.tl.umap(adata)
sc.pl.umap(adata, color='domain')
```

**展示内容**：
- latent space 中不同 cluster 的分布
- 分离程度反映整合质量

**使用场景**：
- 对应 Figure：Figure 2-5 Panel e

---

## 指标选择建议

### 主指标（每个 Figure Panel b 必须包含）
- ARI：评估聚类一致性
- Pearson r（或 MSE）：评估模态一致性

### 补充指标（根据任务选择）
- Task 1：ARI + Pearson r
- Task 2：ARI + MSE（配准误差）
- Task 3：ARI + Pearson r（推断准确性）
- Task 4：综合指标

### 定性展示（每个 Figure Panel c-e）
- Panel c：空间分布图
- Panel d：Marker 展示图
- Panel e：Latent UMAP 或其他分析

## 指标计算工具汇总

| 指标 | Python 工具 | 函数 |
|------|------------|------|
| ARI | scikit-learn | adjusted_rand_score |
| NMI | scikit-learn | normalized_mutual_info_score |
| Silhouette | scikit-learn | silhouette_score |
| Pearson r | NumPy | corrcoef |
| MSE | NumPy | mean((pred-true)**2) |
| Cosine sim | NumPy | dot / norm |

## 下一步
- 根据指标体系，构建分析方法体系（Step 5）
```

## 使用方式

```bash
/bio-metric-system "空间多组学整合 | paper_count: 5 | task_system: [从Step2获得]"
```

## 注意事项

1. **指标要有文献支撑**：确保审稿人认可
2. **定量指标优先**：Panel b 必须有定量比较
3. **定性指标补充**：Panel c-e 展示实际效果
4. **工具明确**：标注计算工具和函数
5. **范围和解读**：说明指标范围和解读方式