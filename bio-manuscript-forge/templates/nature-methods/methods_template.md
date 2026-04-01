# Methods 模板

## 结构

```
Methods
├─ 数据预处理
│   ├─ 数据来源
│   └─ 预处理流程
├─ 模型架构
│   ├─ 整体架构
│   ├─ 各模块详细描述
│   └─ 损失函数
├─ 训练策略
├─ 评价指标计算
├─ 分析流程
├─ Baseline方法
├─ 统计分析
└─ 代码与数据可用性
```

---

## 数据预处理

### 数据来源

```markdown
### 数据来源

本研究使用了 [X] 个数据集（表 [表格编号]）：

| 数据集 | 来源 | 平台 | 物种 | 组织 | 样本数 | 特征数 |
|--------|------|------|------|------|--------|--------|
| Dataset 1 | GEO: GSEXXXXX | Xenium | Human | Breast | 50,000 | 300 genes + 50 proteins |
| Dataset 2 | 公开数据 | Visium | Mouse | Brain | 3,000 | 20,000 genes |
| Dataset 3 | [引用] | Stereo-seq | Mouse | Brain | 100,000 | 25,000 genes |
| ... | ... | ... | ... | ... | ... | ... |

数据可从 [数据库链接] 下载。
```

### 预处理流程

```markdown
### 预处理流程

所有数据使用 Scanpy (v1.9.0) 进行预处理，具体步骤如下：

1. **质量控制 (QC)**：
   ```python
   sc.pp.filter_cells(adata, min_genes=200)
   sc.pp.filter_genes(adata, min_cells=3)
   adata = adata[adata.obs.pct_counts_mt < 20, :]
   ```
   过滤基因数 < 200 的细胞和检测细胞数 < 3 的基因。线粒体基因比例 > 20% 的细胞被移除。

2. **归一化**：
   ```python
   sc.pp.normalize_total(adata, target_sum=1e4)
   sc.pp.log1p(adata)
   ```
   使用 total count 归一化，目标总和为 10,000，然后进行 log1p 变换。

3. **高变基因选择**：
   ```python
   sc.pp.highly_variable_genes(adata, n_top_genes=2000, flavor='seurat_v3')
   adata = adata[:, adata.var.highly_variable]
   ```
   选择前 2,000 个高变基因用于后续分析。

4. **降维**：
   ```python
   sc.tl.pca(adata, n_comps=50)
   ```
   使用 PCA 降至 50 维。
```

---

## 模型架构

### 整体架构

```markdown
### 模型架构

[方法名称]采用 Encoder-[核心模块]-Decoder 架构（图 [Figure 编号]a）：

```
Input → Encoder → [核心模块] → Decoder → Output
          ↓            ↓           ↓
       Latent      Processing   Reconstruction
```

模型参数见表 [表格编号]。
```

### Encoder

```markdown
### Encoder

Encoder 由 [X] 层 [网络类型] 组成：

- 输入维度：[dim]
- 隐藏维度：[hidden_dim]
- 输出维度：[latent_dim]

结构：
```python
self.encoder = nn.Sequential(
    nn.Linear(input_dim, hidden_dim),
    nn.BatchNorm1d(hidden_dim),
    nn.ReLU(),
    nn.Dropout(0.1),
    nn.Linear(hidden_dim, latent_dim)
)
```

公式：
$$z = f_{enc}(x) = W_2 \cdot \text{ReLU}(W_1 x + b_1) + b_2$$

其中 $W_1 \in \mathbb{R}^{hidden \times input}$，$W_2 \in \mathbb{R}^{latent \times hidden}$。
```

### 核心模块（示例：Cross-Attention）

```markdown
### Cross-Attention Module

Cross-Attention 模块用于处理多模态数据间的信息交互：

**参数**：
- `dim_modality_A`：模态 A 特征维度
- `dim_modality_B`：模态 B 特征维度
- `num_heads`：注意力头数（默认 4）
- `dropout`：Dropout 率（默认 0.1）

**结构**：
```python
class CrossAttention(nn.Module):
    def __init__(self, dim_A, dim_B, num_heads=4, dropout=0.1):
        super().__init__()
        self.W_q = nn.Linear(dim_A, dim_A)
        self.W_k = nn.Linear(dim_B, dim_A)
        self.W_v = nn.Linear(dim_B, dim_A)
        self.num_heads = num_heads
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, x_A, x_B):
        Q = self.W_q(x_A)
        K = self.W_k(x_B)
        V = self.W_v(x_B)
        
        # Multi-head attention
        attn = softmax(Q @ K.T / sqrt(d)) @ V
        return self.dropout(attn)
```

**公式**：
$$\text{CrossAttn}(x_A, x_B) = \text{softmax}\left(\frac{Q_A K_B^T}{\sqrt{d}}\right) V_B$$

其中 $Q_A = W_q x_A$，$K_B = W_k x_B$，$V_B = W_v x_B$，$d$ 为注意力维度。
```

### 损失函数

```markdown
### 损失函数

总损失函数由 [X] 部分组成：

$$\mathcal{L}_{total} = \lambda_1 \mathcal{L}_{recon} + \lambda_2 \mathcal{L}_{cross} + \lambda_3 \mathcal{L}_{reg}$$

1. **重建损失**：
   $$\mathcal{L}_{recon} = \|x - \hat{x}\|_2^2$$

2. **Cross-Attention 一致性损失**：
   $$\mathcal{L}_{cross} = \|z_A - z_{cross}\|_2^2 + \|z_B - z_{cross}\|_2^2$$

3. **正则化损失**：
   $$\mathcal{L}_{reg} = \|\theta\|_2^2$$

权重参数：$\lambda_1 = 1.0$，$\lambda_2 = 0.5$，$\lambda_3 = 0.01$。
```

---

## 训练策略

```markdown
### 训练策略

**训练参数**：
| 参数 | 值 |
|------|---|
| Batch size | 256 |
| Learning rate | 1e-3 |
| Optimizer | Adam |
| Weight decay | 1e-5 |
| Epochs | 200 |
| Early stopping | 20 epochs patience |

**训练流程**：
```python
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3, weight_decay=1e-5)
scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer, patience=10)

for epoch in range(max_epochs):
    for batch in dataloader:
        optimizer.zero_grad()
        output = model(batch)
        loss = compute_loss(output, batch)
        loss.backward()
        torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
        optimizer.step()
    
    # Validation
    val_loss = validate(model, val_dataloader)
    scheduler.step(val_loss)
    
    # Early stopping
    if val_loss < best_loss:
        best_loss = val_loss
        patience_counter = 0
    else:
        patience_counter += 1
        if patience_counter >= 20:
            break
```
```

---

## 评价指标计算

```markdown
### 评价指标

1. **ARI (Adjusted Rand Index)**：
   ```python
   from sklearn.metrics import adjusted_rand_score
   ari = adjusted_rand_score(true_labels, pred_labels)
   ```
   公式：$\text{ARI} = \frac{RI - E[RI]}{\max(RI) - E[RI]}$

2. **Pearson Correlation**：
   ```python
   import numpy as np
   r = np.corrcoef(x, y)[0, 1]
   ```
   公式：$r = \frac{\sum(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum(x_i - \bar{x})^2 \sum(y_i - \bar{y})^2}}$

3. **MSE (Mean Squared Error)**：
   ```python
   mse = np.mean((pred - true) ** 2)
   ```
```

---

## 分析流程

```markdown
### 分析流程

1. **职类分析**：
   ```python
   import scanpy as sc
   sc.tl.leiden(adata, resolution=0.5)
   ```

2. **空间可视化**：
   ```python
   sc.pl.spatial(adata, color='leiden', palette='tab20')
   ```

3. **Marker 展示**：
   ```python
   sc.pl.spatial(adata, color=['EPCAM', 'CD3D'], cmap='viridis')
   ```

4. **通路富集**：
   ```python
   import gseapy as gp
   enr = gp.enrichr(gene_list=gene_list, gene_sets='KEGG_2021_Human')
   ```
```

---

## Baseline 方法

```markdown
### Baseline 方法

本研究比较了以下 Baseline 方法：

| 方法 | 来源 | 实现方式 | 主要参数 |
|------|------|---------|---------|
| Baseline 1 | [引用] | 官方代码 | 默认参数 |
| Baseline 2 | [引用] | Scanpy | resolution=0.5 |
| Baseline 3 | [引用] | PyPI 安装 | 默认参数 |

所有 Baseline 使用与 Our method 相同的预处理流程。
```

---

## 统计分析

```markdown
### 统计分析

定量比较使用 paired t-test 评估显著性：
```python
from scipy.stats import ttest_rel
p_value = ttest_rel(our_scores, baseline_scores).pvalue
```

显著性阈值：$p < 0.05$。
多重比较使用 Bonferroni 校正。
```

---

## 代码与数据可用性

```markdown
### 代码与数据可用性

**代码**：
- GitHub: https://github.com/xxx/xxx
- 文档: https://xxx.readthedocs.io

**数据**：
- Dataset 1: GEO (GSEXXXXX)
- Dataset 2: [数据链接]

**分析结果**：
- [结果链接]
```

---

## 写作建议

### 公式规范

- 使用 LaTeX 格式
- 符号有定义
- 公式编号（如需要）

### 代码规范

- 使用语法高亮
- 注释关键步骤
- 包含版本信息

### 参数表格

- 参数名称、值、说明
- 默认值标注
- 单位明确

### 检查清单

- [ ] 公式正确
- [ ] 代码可运行
- [ ] 参数完整
- [ ] 工具版本标注
- [ ] 可复现性