# bio-demo-validate

**Step 9: Demo 快速验证**

应用代码修改方案，快速运行 Demo 验证可行性。

## 功能

1. 应用代码修改方案
2. 下载 Demo 数据集
3. 快速运行（subsample + 少 epoch）
4. 可行性判断与反馈
5. 如果不可行，提供修改建议

## 输入格式

```
code_modification_plan: [代码修改方案文件]
demo_data: [Demo数据集链接]
base_work_repo: [原有代码仓库路径]
```

## 执行流程

### Step 9.1: 应用代码修改

```bash
# 1. 备份原代码
cp -r $base_work_repo $base_work_repo_backup

# 2. 应用修改
# - 创建新增文件
# - 修改现有文件
# - 更新配置

# 3. 检查修改
git diff $base_work_repo
```

### Step 9.2: 下载 Demo 数据

```bash
# 下载数据
wget $demo_data -O demo_data.h5ad

# 检查数据
python -c "
import scanpy as sc
adata = sc.read_h5ad('demo_data.h5ad')
print(f'Shape: {adata.shape}')
print(f'Features: {adata.var_names[:5]}')
print(f'Obs columns: {adata.obs.columns[:5]}')
"
```

### Step 9.3: 数据预处理

```python
import scanpy as sc

# 读取数据
adata = sc.read_h5ad('demo_data.h5ad')

# 快速预处理
# 1. QC（简化）
sc.pp.filter_cells(adata, min_genes=200)
sc.pp.filter_genes(adata, min_cells=3)

# 2. Normalization
sc.pp.normalize_total(adata, target_sum=1e4)
sc.pp.log1p(adata)

# 3. HVG
sc.pp.highly_variable_genes(adata, n_top_genes=2000)
adata = adata[:, adata.var.highly_variable]

# 4. Subsample（快速验证）
if adata.n_obs > 1000:
    sc.pp.subsample(adata, n_obs=1000)
    
print(f'Subsampled shape: {adata.shape}')

# 保存预处理后的数据
adata.write_h5ad('demo_data_processed.h5ad')
```

### Step 9.4: 快速运行

```bash
# 快速运行配置
python train.py \
    --config config/default.yaml \
    --data demo_data_processed.h5ad \
    --epochs 10 \
    --batch_size 32 \
    --device cuda \
    --verbose

# 或使用简化命令
python quick_test.py --data demo_data_processed.h5ad
```

### Step 9.5: 检查输出

```python
import torch
import numpy as np

# 检查模型输出
checkpoint = torch.load('checkpoints/model_final.pt')

# 1. 模型是否保存
print(f"Model saved: {'model_state_dict' in checkpoint}")

# 2. Loss 是否下降
losses = checkpoint.get('losses', [])
if len(losses) > 1:
    print(f"Initial loss: {losses[0]:.4f}")
    print(f"Final loss: {losses[-1]:.4f}")
    print(f"Loss decreased: {losses[-1] < losses[0]}")

# 3. 输出维度是否正确
model_output = checkpoint.get('last_output')
if model_output is not None:
    print(f"Output shape: {model_output.shape}")
    print(f"Expected shape: [batch, latent_dim]")
    
# 4. 检查是否有 NaN
if torch.isnan(model_output).any():
    print("WARNING: Output contains NaN!")
else:
    print("Output is valid (no NaN)")
```

### Step 9.6: 可行性判断

```python
def check_feasibility(checkpoint, adata):
    """检查 Demo 运行是否可行"""
    
    issues = []
    
    # 1. 检查运行是否成功
    if checkpoint is None:
        issues.append("Model failed to train")
        return False, issues
    
    # 2. 检查 Loss 是否下降
    losses = checkpoint.get('losses', [])
    if len(losses) > 1 and losses[-1] >= losses[0]:
        issues.append("Loss did not decrease during training")
    
    # 3. 检查输出是否有 NaN
    model_output = checkpoint.get('last_output')
    if model_output is not None and torch.isnan(model_output).any():
        issues.append("Model output contains NaN values")
    
    # 4. 检查维度是否正确
    if model_output is not None:
        expected_dim = adata.n_obs  # batch size
        if model_output.shape[0] != expected_dim:
            issues.append(f"Output batch size mismatch: {model_output.shape[0]} vs {expected_dim}")
    
    # 5. 检查聚类是否合理
    if 'latent' in checkpoint:
        latent = checkpoint['latent']
        # 快速聚类测试
        from sklearn.cluster import KMeans
        kmeans = KMeans(n_clusters=5, random_state=42)
        labels = kmeans.fit_predict(latent)
        
        # 检查是否有有效聚类
        unique_labels = len(set(labels))
        if unique_labels < 2:
            issues.append("Clustering failed: only 1 cluster found")
    
    # 判断
    feasible = len(issues) == 0
    
    return feasible, issues
```

### Step 9.7: 修改建议（如果不可行）

```python
def generate_modification_suggestions(issues):
    """根据问题生成修改建议"""
    
    suggestions = []
    
    for issue in issues:
        if "Loss did not decrease" in issue:
            suggestions.append({
                "issue": issue,
                "suggestion": "调整学习率或损失函数权重",
                "action": "尝试降低 learning_rate 或调整 loss weights"
            })
        
        elif "NaN" in issue:
            suggestions.append({
                "issue": issue,
                "suggestion": "检查梯度爆炸或数值稳定性",
                "action": "添加 gradient clipping 或检查输入数据归一化"
            })
        
        elif "dimension mismatch" in issue:
            suggestions.append({
                "issue": issue,
                "suggestion": "检查模型输入输出维度配置",
                "action": "确认 data loader 和模型配置一致"
            })
        
        elif "Clustering failed" in issue:
            suggestions.append({
                "issue": issue,
                "suggestion": "检查 latent representation 质量",
                "action": "增加训练 epoch 或调整模型架构"
            })
    
    return suggestions
```

## 输出格式

```markdown
# Demo 验证结果

## 运行配置

| 配置项 | 值 |
|--------|---|
| 数据集 | demo_data.h5ad |
| 原始样本数 | 50,000 |
| Subsample | 1,000 |
| Epochs | 10 |
| Batch size | 32 |
| Device | cuda |

## 运行日志

```
[训练日志]
Epoch 1/10: loss=2.3456
Epoch 2/10: loss=1.8234
Epoch 3/10: loss=1.5678
...
Epoch 10/10: loss=0.8765
```

## 输出检查

| 检查项 | 结果 | 详情 |
|--------|------|------|
| 模型保存 | ✅ | checkpoint saved |
| Loss 下降 | ✅ | 2.35 → 0.88 |
| 无 NaN | ✅ | Output valid |
| 维度正确 | ✅ | [1000, 128] |
| 聚类有效 | ✅ | 5 clusters |

## 可行性判断

**结论**：✅ 可行

**原因**：
- 训练成功完成
- Loss 正常下降
- 输出维度正确
- 无数值问题

## 下一步

Demo 验证通过，建议继续：
1. 完整数据运行
2. 详细分析执行
3. 生成 Figure 数据

---

# （如果不可行）

## 可行性判断

**结论**：❌ 不可行

## 发现问题

1. **Loss 未下降**
   - 问题：Loss 从 2.35 → 2.33，几乎无变化
   - 原因：可能是学习率过小或损失函数设计问题

2. **输出包含 NaN**
   - 问题：第 5 个 epoch 后出现 NaN
   - 原因：可能是梯度爆炸或数值不稳定

## 修改建议

| 问题 | 建议 | 具体操作 |
|------|------|---------|
| Loss 未下降 | 调整学习率 | learning_rate: 1e-4 → 1e-3 |
| 输出 NaN | 添加 gradient clipping | max_grad_norm: 1.0 |
| 数值稳定 | 检查输入归一化 | 确保输入在合理范围 |

## 代码修改建议

### 修改文件：config/default.yaml

```yaml
# 原有
learning_rate: 1e-4

# 修改后
learning_rate: 1e-3
max_grad_norm: 1.0  # 新增
```

### 修改文件：train.py

```python
# 添加 gradient clipping
loss.backward()
torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)  # 新增
optimizer.step()
```

## 下一步

1. 应用修改建议
2. 重新运行 Demo 验证
3. 如果仍然不可行，进一步调试
```

## 使用方式

```bash
/bio-demo-validate "code_modification_plan: 08_CODE_MODIFICATION_PLAN.md | demo_data: https://xxx/data.h5ad | base_work_repo: /tmp/base_work_repo"
```

## 注意事项

1. **Subsample 策略**：快速验证用 1000 样本，正式运行用全量数据
2. **Epoch 设置**：快速验证用 10 epochs，正式运行根据收敛情况调整
3. **资源检查**：确认 GPU 内存足够
4. **日志记录**：保留运行日志便于调试
5. **失败处理**：不可行时提供具体修改建议
6. **迭代验证**：修改后重新验证直到可行