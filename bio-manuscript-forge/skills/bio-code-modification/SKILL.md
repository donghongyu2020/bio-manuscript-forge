# bio-code-modification

**Step 8: 代码修改方案（完整流程版）**

基于用户创新点，**自动完成全流程修改**，而不是只改核心代码。

---

## 🎯 核心理念

**一次 idea → 完整修改**，不等用户提醒上下游和教程！

---

## 📋 完整工作流程（5 阶段）

### Phase 0: 理解需求与代码分析

**目标**：明确修改目标，理解现有代码结构

#### Step 0.1: 明确用户创新点

```
用户输入：
- topic: 研究主题
- base_work: 原有代码链接
- innovation: 用户创新点
- refine_feedback: 三审稿人反馈（可选）
```

#### Step 0.2: 分析代码结构

```bash
# 列出所有源代码文件
find src -name "*.py" | sort

# 列出教程文件
find tutorial -name "*.ipynb" -o -name "*.md" | sort

# 列出测试文件
find tests -name "*.py" | sort
```

**输出代码结构报告**：
```markdown
## 代码结构

### 源代码
```
src/
├── preprocessing/
│   ├── pp.py
│   └── __init__.py
├── model/
│   ├── model.py
│   └── __init__.py
├── training/
│   ├── train.py
│   └── __init__.py
├── analysis/
│   ├── analyze.py
│   └── __init__.py
└── __init__.py
```

### 教程
```
tutorial/
├── quickstart.ipynb
└── advanced.ipynb
```

### 测试
```
tests/
├── test_preprocessing.py
├── test_model.py
└── test_training.py
```
```

#### Step 0.3: 确认数据流向

**标准 ML 数据流**：
```
输入数据 → 预处理 → 模型构建 → 训练 → 分析 → 输出结果
```

**确认关键问题**：
1. 数据从哪里进入？
2. 经过哪些处理？
3. 输出到哪里？

---

### Phase 1: 设计方案（必须先确认！）

**目标**：输出完整设计方案，等待用户确认后再执行

#### Step 1.1: 设计方案表格

```markdown
## 设计方案：[创新点名称]

### 背景
[描述原始代码的问题或改进空间]

### 目标
[描述改进目标]

### 修改计划

| 阶段 | 文件 | 修改内容 | 新增/修改 | 优先级 |
|------|------|----------|-----------|--------|
| 预处理 | `preprocessing/xxx.py` | 新增函数 | 新增 | High |
| 模型 | `model/model.py` | 修改架构 | 修改 | High |
| 训练 | `training/train.py` | 新增训练函数 | 新增 | High |
| 分析 | `analysis/analyze.py` | 新增分析函数 | 新增 | Medium |
| 教程 | `tutorial/xxx.ipynb` | 更新流程 | 修改 | Medium |
| 测试 | `tests/test_xxx.py` | 新增测试 | 新增 | Medium |

### 数据流（修改后）
输入 → [新预处理] → [新模型] → [新训练] → [新分析] → 输出

### 三审稿人反馈处理（如有）
| 反馈来源 | 问题 | 响应方式 | 修改位置 |
|----------|------|----------|----------|
| Editor | 创新性不足 | 强调方法创新 | model.py |
| 计算审稿人 | 缺少 Baseline | 新增对比实验 | training.py |
| 生物审稿人 | 缺少生物学验证 | 新增 marker 分析 | analysis.py |

### 风险评估
| 风险 | 影响 | 缓解措施 |
|------|------|---------|
| 接口不兼容 | 高 | 保持向后兼容 |
| 性能下降 | 中 | 添加性能测试 |

### 是否继续？
请确认设计方案后，我将继续执行修改。
```

#### Step 1.2: 等待用户确认

**⚠️ 重要**：必须等用户确认后再进入 Phase 2！

---

### Phase 2: 核心模块修改（按数据流向顺序）

**目标**：按照预处理 → 模型 → 训练 → 分析的顺序修改

#### Step 2.1: 预处理模块

**Checklist**：
- [ ] 新增预处理函数
- [ ] 更新 `__init__.py` 导出
- [ ] 确保与原始流程兼容
- [ ] 测试输出格式正确

**示例**：
```python
# preprocessing/pp_multimodal.py（新增）

def pp_multimodal(adata, modalities=['RNA', 'ATAC', 'Protein']):
    """
    多模态数据预处理
    
    输入：
        adata: AnnData 对象
        modalities: 模态列表
    
    输出：
        adata.obsm['X_gene_input']: RNA 输入
        adata.obsm['X_atac_input']: ATAC 输入
        adata.obsm['X_protein_input']: Protein 输入
    """
    # 预处理各模态
    for mod in modalities:
        # ... 预处理逻辑
        adata.obsm[f'X_{mod.lower()}_input'] = processed_data
    
    return adata
```

```python
# preprocessing/__init__.py（修改）

from .pp_multimodal import pp_multimodal

__all__ = ['pp', 'pp_multimodal', ...]
```

#### Step 2.2: 模型构建

**Checklist**：
- [ ] 新增/修改模型类
- [ ] 更新 `__init__.py` 导出
- [ ] 保持接口兼容性
- [ ] 添加参数文档

**示例**：
```python
# model/multimodal_model.py（新增）

class MultiModalEncoder(nn.Module):
    """多模态编码器，使用 Cross-Attention"""
    
    def __init__(self, dim_rna, dim_atac, dim_protein, latent_dim):
        super().__init__()
        
        # 各模态独立编码器
        self.rna_encoder = Encoder(dim_rna, latent_dim)
        self.atac_encoder = Encoder(dim_atac, latent_dim)
        self.protein_encoder = Encoder(dim_protein, latent_dim)
        
        # Cross-Attention 融合
        self.cross_attention = CrossAttention(latent_dim, num_heads=4)
    
    def forward(self, rna, atac, protein):
        # 独立编码
        z_rna = self.rna_encoder(rna)
        z_atac = self.atac_encoder(atac)
        z_protein = self.protein_encoder(protein)
        
        # Cross-Attention 融合
        z_fused = self.cross_attention(z_rna, z_atac, z_protein)
        
        return z_fused
```

#### Step 2.3: 训练函数

**Checklist**：
- [ ] 新增训练函数（包含模型创建 + 训练）
- [ ] 更新 `__init__.py` 导出
- [ ] 添加便捷调用函数
- [ ] 支持模型保存/加载

**示例**：
```python
# training/train_multimodal.py（新增）

def train_multimodal_stvcr(
    adata,
    modalities=['RNA', 'ATAC', 'Protein'],
    latent_dim=128,
    epochs=200,
    lr=1e-3,
    device='cuda',
    save_path='model.pt'
):
    """
    多模态训练函数（一键调用）
    
    参数：
        adata: 预处理后的 AnnData
        modalities: 模态列表
        latent_dim: latent 维度
        epochs: 训练轮数
        lr: 学习率
        device: 设备
        save_path: 模型保存路径
    
    返回：
        model: 训练好的模型
        losses: 损失历史
    """
    # 1. 自动创建模型
    model = MultiModalSTVCR(
        dim_rna=adata.obsm['X_gene_input'].shape[1],
        dim_atac=adata.obsm['X_atac_input'].shape[1],
        dim_protein=adata.obsm['X_protein_input'].shape[1],
        latent_dim=latent_dim
    ).to(device)
    
    # 2. 训练
    optimizer = torch.optim.Adam(model.parameters(), lr=lr)
    losses = []
    
    for epoch in range(epochs):
        # ... 训练逻辑
        loss = train_step(model, batch, optimizer)
        losses.append(loss)
    
    # 3. 保存模型
    torch.save(model.state_dict(), save_path)
    
    return model, losses
```

#### Step 2.4: 分析模块

**Checklist**：
- [ ] 新增分析函数
- [ ] 更新 `__init__.py` 导出
- [ ] 支持可视化
- [ ] 与 Demo 验证联动

**示例**：
```python
# analysis/analyze_multimodal.py（新增）

def get_multimodal_velocities(model, adata, device='cuda'):
    """
    获取多模态速度
    
    参数：
        model: 训练好的模型
        adata: 数据
    
    输出：
        adata.obsm['velocity_rna']: RNA 速度
        adata.obsm['velocity_atac']: ATAC 速度
        adata.obsm['velocity_protein']: Protein 速度
    """
    # ... 分析逻辑
    
    return adata

def plot_multimodal_results(adata):
    """可视化多模态结果"""
    import scanpy as sc
    
    # UMAP
    sc.tl.umap(adata)
    sc.pl.umap(adata, color='cluster')
    
    # 空间分布
    sc.pl.spatial(adata, color='cluster', palette='tab20')
```

---

### Phase 3: 教程更新（自动完成！）

**⚠️ 重要**：不要等用户提醒才改教程！

#### Step 3.1: 检查现有教程结构

```bash
ls tutorial/*.ipynb
```

#### Step 3.2: 更新教程内容

**Checklist**：
- [ ] 更新导入语句
- [ ] 更新预处理步骤
- [ ] 更新模型构建/训练步骤
- [ ] 更新分析步骤
- [ ] 确保代码可直接运行

**示例教程**：
```markdown
# 多模态 STVCR 教程

## 快速开始

### 1. 数据加载与预处理
\`\`\`python
import scanpy as sc
from stvcr.preprocessing import pp_multimodal

# 加载数据
adata = sc.read_h5ad('data.h5ad')

# 预处理（新增）
adata = pp_multimodal(adata, modalities=['RNA', 'ATAC', 'Protein'])
\`\`\`

### 2. 训练
\`\`\`python
from stvcr.training import train_multimodal_stvcr

# 一键训练（新增）
model, losses = train_multimodal_stvcr(
    adata,
    modalities=['RNA', 'ATAC', 'Protein'],
    epochs=200
)
\`\`\`

### 3. 分析
\`\`\`python
from stvcr.analysis import get_multimodal_velocities, plot_multimodal_results

# 获取多模态速度（新增）
adata = get_multimodal_velocities(model, adata)

# 可视化
plot_multimodal_results(adata)
\`\`\`

## 详细说明

### 数据要求
- RNA: `adata.X`
- ATAC: `adata.obsm['ATAC']`
- Protein: `adata.obsm['Protein']`

### 参数说明
...
```

#### Step 3.3: 创建快速开始指南

```markdown
# 快速开始

## 三步使用

### 1. 预处理
\`\`\`python
from xxx import pp_multimodal
adata = pp_multimodal(adata)
\`\`\`

### 2. 训练
\`\`\`python
from xxx import train_multimodal_stvcr
model, losses = train_multimodal_stvcr(adata)
\`\`\`

### 3. 分析
\`\`\`python
from xxx import get_multimodal_velocities
adata = get_multimodal_velocities(model, adata)
\`\`\`
```

---

### Phase 4: 测试验证

#### Step 4.1: 单元测试

```python
# tests/test_multimodal.py（新增）

def test_pp_multimodal():
    """测试多模态预处理"""
    adata = create_test_adata()
    adata = pp_multimodal(adata)
    
    assert 'X_gene_input' in adata.obsm
    assert 'X_atac_input' in adata.obsm
    assert 'X_protein_input' in adata.obsm

def test_multimodal_model():
    """测试多模态模型"""
    model = MultiModalSTVCR(dim_rna=100, dim_atac=50, dim_protein=20)
    
    # 测试前向传播
    output = model(rna, atac, protein)
    assert output.shape == (batch_size, latent_dim)
```

#### Step 4.2: 集成测试

```python
# 测试完整流程
adata = load_test_data()
adata = pp_multimodal(adata)
model, losses = train_multimodal_stvcr(adata, epochs=10)
adata = get_multimodal_velocities(model, adata)

# 检查输出
assert 'velocity_rna' in adata.obsm
```

---

### Phase 5: 修改总结与 Demo 验证联动

#### Step 5.1: 输出修改总结

```markdown
## ✅ 修改完成

### 文件清单
| 文件 | 行数 | 说明 |
|------|------|------|
| `preprocessing/pp_multimodal.py` | 新增 | 多模态预处理 |
| `model/multimodal_model.py` | 新增 | 多模态模型 |
| `training/train_multimodal.py` | 新增 | 多模态训练 |
| `analysis/analyze_multimodal.py` | 新增 | 多模态分析 |
| `tutorial/multimodal.ipynb` | 新增 | 多模态教程 |
| `preprocessing/__init__.py` | 修改 | 导出新函数 |
| `model/__init__.py` | 修改 | 导出新模型 |
| `training/__init__.py` | 修改 | 导出新训练函数 |
| `analysis/__init__.py` | 修改 | 导出新分析函数 |

### 使用流程
\`\`\`python
# 1. 预处理
from stvcr.preprocessing import pp_multimodal
adata = pp_multimodal(adata)

# 2. 训练
from stvcr.training import train_multimodal_stvcr
model, losses = train_multimodal_stvcr(adata)

# 3. 分析
from stvcr.analysis import get_multimodal_velocities
adata = get_multimodal_velocities(model, adata)
\`\`\`

### 教程位置
- `tutorial/multimodal.ipynb`

### Demo 验证
建议运行以下命令验证：
\`\`\`bash
python demo_multimodal.py --data demo_data.h5ad --epochs 10
\`\`\`

### 下一步
修改完成，可进入 Step 9（Demo 验证）确认可行性。
```

#### Step 5.2: Demo 验证联动

**联动 Step 9**：
- 提供 Demo 运行命令
- 提供 Demo 数据要求
- 提供 Demo 验证参数

---

## 🚨 常见问题 Checklist

### 必须检查的问题：

1. **预处理是否生成正确格式？**
   - 输出 `adata.obsm['X_gene_input']` 是否正确？
   - 输出 `adata.obsm['X_atac_input']` 是否正确？
   - 输出 `adata.obsm['X_protein_input']` 是否正确？

2. **模型是否能正确创建？**
   - 参数是否传递正确？
   - 是否自动创建模型？

3. **训练函数是否完整？**
   - 是否包含模型创建？
   - 是否包含训练循环？
   - 是否保存模型？

4. **分析函数是否对应？**
   - 是否支持新的输出格式？
   - 是否有可视化？

5. **教程是否同步？**
   - 导入语句是否正确？
   - 调用方式是否正确？
   - 能否直接运行？

6. **测试是否覆盖？**
   - 单元测试是否通过？
   - 集成测试是否通过？

---

## 📝 输出格式

### 输出文件

```
08_CODE_MODIFICATION_PLAN.md

包含：
├─ Phase 0: 代码结构分析
├─ Phase 1: 设计方案（等待确认）
├─ Phase 2: 核心模块修改方案
│   ├─ 预处理模块
│   ├─ 模型构建
│   ├─ 训练函数
│   └─ 分析模块
├─ Phase 3: 教程更新方案
├─ Phase 4: 测试方案
└─ Phase 5: 修改总结
```

---

## 使用方式

```bash
/bio-code-modification "base_work: https://github.com/xxx | innovation: 增加 Cross-Attention | refine_feedback: [三审稿人反馈]"
```

---

## ⚠️ 重要提醒

1. **不要等用户提醒才改教程！**
2. **不要等用户提醒才改上下游！**
3. **遵循机器学习标准流程：预处理 → 模型 → 训练 → 分析**
4. **每改一个核心模块，立即检查其上下游是否需要修改**
5. **设计方案必须先确认，再执行！**
6. **一次 idea → 完整修改！**

---

*Based on: ai-method-code-modify + bio-code-modification*
*Updated: 2026-04-01 02:40（合并两版精华）*