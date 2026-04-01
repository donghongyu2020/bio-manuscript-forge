# bio-dataset-search

**Step 3: 数据集搜索与匹配**

搜索适合各任务的数据集，并进行数据集-任务匹配。

## 功能

1. 从已有工作提取数据集
2. 或自行搜索数据集
3. 数据集与任务匹配
4. 确保每个任务有对应数据集

## 输入格式

```
topic: [研究主题]
task_system: [任务体系，从 Step 2 获得]
paper_count: [已有工作数量]
existing_papers: [相关工作列表，可选]
```

## 执行流程

### Step 3.1: 从已有工作提取数据集

**条件**：paper_count >= 5

**策略**：
```
for paper in top_papers:
    # 下载论文 PDF（如果可获取）
    pdf = download_paper(paper.url)
    
    # 阅读提取数据集信息
    # 重点阅读 Methods / Data Availability 部分
    datasets = extract_datasets_from_paper(pdf)
    
    # 提取内容：
    # - 数据集名称
    # - 数据来源（GEO、ArrayExpress、公开数据）
    # - 平台（Visium、Xenium、Stereo-seq）
    # - 维度（样本数、特征数）
    # - 模态（RNA、Protein、ATAC）
    # - 下载链接

# 汇总数据集
dataset_catalog = aggregate_datasets(datasets)
```

### Step 3.2: 自行搜索数据集

**条件**：paper_count < 5

**策略 1：关键词搜索**：
```
keywords = [
    f"{topic} dataset",
    f"{topic} data GEO",
    f"{topic} benchmark data",
    f"{modality} spatial data",  # 如 "RNA spatial data"
    ...
]

for keyword in keywords:
    # 搜索 GEO、ArrayExpress
    geo_results = search_geo(keyword)
    arrayexpress_results = search_arrayexpress(keyword)
    
    # 提取数据集信息
    datasets.extend(process_search_results(geo_results))
```

**策略 2：向上级领域借用**：
```
parent_domain = find_parent_domain(topic)
parent_datasets = search_datasets_in_domain(parent_domain)

# 适配到当前领域
# 例如：单细胞多组学数据集 → 空间多组学（需要空间信息）
adapted_datasets = adapt_datasets(parent_datasets, topic)
```

### Step 3.3: 数据集信息标准化

每个数据集需要标准化信息：

```
数据集信息模板：

### Dataset X: [数据集名称]

**基本信息**：
- 来源：[GEO: GSEXXXXX / 公开数据 / 已发表论文]
- 平台：[Visium / Xenium / Stereo-seq / MISAR-seq / ...]
- 物种：[Human / Mouse / ...]
- 组织：[Brain / Breast / Liver / ...]

**数据维度**：
- 样本数：[X cells / X spots]
- 特征数：[X genes / X proteins]
- 切片数：[X slices]
- 时间点：[E13/E15/E18（如果有）]

**模态信息**：
- 模态1：[RNA] - [测量方式]
- 模态2：[Protein] - [标记数量]
- 模态3：[ATAC] - [如果有]

**数据质量**：
- QC 信息：[是否有 QC]
- 注释信息：[是否有 cell type annotation]

**下载信息**：
- 下载链接：[URL]
- 数据格式：[h5ad / FASTQ / ...]
- 是否需要预处理：[是/否]

**适用任务**：
- 推荐任务：[Task X]
- 适用原因：[原因描述]

**已有应用**（如果有）：
- 已在论文 X 中使用
- 已有 baseline 结果：[结果描述]
```

### Step 3.4: 数据集-任务匹配

**匹配原则**：
```
匹配原则：
1. 每个任务至少有 1-2 个对应数据集
2. 数据集要覆盖任务的关键特征
   - Task 1（垂直整合）：需要同一切片的多模态数据
   - Task 2（水平整合）：需要多切片数据
   - Task 3（马赛克整合）：需要部分测量数据
   - Task 4（对角整合）：需要跨平台、跨分辨率数据
3. 数据集要有公开下载链接
4. 数据集要有足够的质量信息
```

**匹配过程**：
```
for task in task_system:
    # 找适合该任务的数据集
    suitable_datasets = find_datasets_for_task(task, dataset_catalog)
    
    # 选择最佳数据集
    best_dataset = select_best_dataset(suitable_datasets)
    
    # 记录匹配关系
    task_dataset_mapping[task] = best_dataset
```

## 输出格式

```markdown
# 数据集目录

## 数据来源
- 已有工作提取：[X篇] 论文中的数据集
- 自行搜索：[X] 个数据集
- 上级领域借用：[X] 个数据集

## 数据集列表

### Dataset 1: [名称]

**基本信息**：
- 来源：GEO: GSEXXXXX
- 平台：Xenium
- 物种：Human
- 组织：Breast Cancer

**数据维度**：
- 样本数：50,000 cells
- 特征数：300 genes + 50 proteins

**模态信息**：
- 模态1：RNA - 全转录组
- 模态2：Protein - 50 markers

**下载信息**：
- 下载链接：https://...
- 数据格式：h5ad

**适用任务**：
- 推荐任务：Task 1（垂直整合）
- 适用原因：同一切片 RNA + Protein，完美垂直整合示例

### Dataset 2: [名称]
...

### Dataset 3: [名称]
...

### Dataset 4: [名称]
...

## 数据集-任务匹配表

| 任务 | 推荐数据集 | 适用原因 | 备注 |
|------|-----------|---------|------|
| Task 1 | Dataset 2 | 同切片多模态 | 完美示例 |
| Task 2 | Dataset 1 | 多切片数据 | 需配准 |
| Task 3 | Dataset 3 | 部分测量 | 推断场景 |
| Task 4 | Dataset 4 + Dataset X | 跨平台跨分辨率 | 创新任务 |

## 数据集获取说明

### GEO 数据下载
```bash
# 使用 GEOquery (R) 或 wget
wget https://ftp.ncbi.nlm.nih.gov/geo/series/GSEXXX/GSEXXXXX/
```

### 公开平台数据下载
```bash
# Xenium 数据
wget https://...

# Stereo-seq 数据
wget https://...
```

## 数据预处理建议

根据数据格式和任务需求，建议预处理流程：

| 数据集 | 预处理需求 | 建议 Skill |
|--------|-----------|------------|
| Dataset 1 | QC + Normalization | OmicsClaw: spatial-preprocess |
| Dataset 2 | 配准预处理 | PASTE / STalign |
| Dataset 3 | 缺失模态标记 | 自定义处理 |
| Dataset 4 | 分辨率统一 | 自定义处理 |

## 下一步
- 根据数据集和任务，构建评价指标体系（Step 4）
```

## 使用方式

```bash
/bio-dataset-search "空间多组学整合 | paper_count: 5 | task_system: [从Step2获得的任务体系]"
```

## 注意事项

1. **优先从已有工作提取**：这样数据集有文献支撑
2. **确保下载链接有效**：避免无法获取的数据集
3. **数据质量检查**：记录 QC 信息和注释信息
4. **匹配合理性**：数据集特征要符合任务需求
5. **备用数据集**：每个任务最好有 2 个候选数据集