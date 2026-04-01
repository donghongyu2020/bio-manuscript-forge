# bio-innovation-check

**Step 1: 创新性检测**

通过 Topic 同义变换搜索，判断研究想法是否达到子刊级别。

## 功能

1. 生成 Topic 的多个同义变体
2. 搜索 PubMed + bioRxiv + arXiv q-bio
3. 统计相似文章数量
4. 判断创新性级别
5. 提供修改建议（如果需要）

## 输入格式

```
topic: [研究主题]
```

## 执行流程

### Step 1.1: Topic 同义变换

**策略**：

```
1. 核心词替换：
   - 中文 → 英文
   - 专业术语 → 通俗表达
   - 例如："空间多组学整合" → "spatial multi-omics integration"

2. 词序调整：
   - "spatial transcriptomics proteomics integration"
   - "integration of spatial transcriptomics and proteomics"

3. 上下位词扩展：
   - "空间组学" → "空间转录组"、"空间蛋白质组"
   - "整合" → "融合"、"对齐"、"映射"

4. 相邻领域词汇：
   - 上位："多模态整合"
   - 下位："空间转录组-蛋白质组整合"

5. 方法词扩展：
   - 加上方法关键词："deep learning"、"neural network"、"attention"
```

**生成数量**：默认 15-20 个变体

### Step 1.2: 文献搜索

**搜索平台**：
- **PubMed**：已发表论文
- **bioRxiv**：生物学预印本
- **arXiv q-bio**：计算生物学预印本

**搜索方式**：
```
调用 searxng skill 或 web_search 工具

for variant in topic_variants:
    results = search(variant, platforms=[PubMed, bioRxiv, arXiv])
    all_papers.extend(results)

# 去重（按标题相似度）
unique_papers = deduplicate(all_papers, threshold=0.8)
```

### Step 1.3: 统计与判断

**创新性阈值**：

```
if paper_count <= 2:
    level = "子刊级别"
    feedback = "该想法具有较高创新性，可作为子刊投稿方向"
    
elif paper_count <= 5:
    level = "需要完善"
    feedback = "已有 {count} 篇相关工作，建议调整方向"
    
else:
    level = "需重新定位"
    feedback = "已有大量相关工作，需要重新定位研究方向"
```

### Step 1.4: 修改建议

如果需要完善，提供具体修改建议：

```
修改角度：
1. 方法角度：改进算法细节
2. 任务角度：拓展到新任务场景
3. 数据角度：使用新数据集验证
4. 分析角度：增加新的分析维度
```

## 输出格式

```markdown
# 创新性评估报告

## 搜索策略
- 同义变换数量：15 个
- 搜索平台：PubMed, bioRxiv, arXiv q-bio
- 搜索时间：YYYY-MM-DD

## 同义变换列表
| 序号 | 变体 |
|-----|------|
| 1 | spatial multi-omics integration |
| 2 | spatial transcriptomics proteomics integration |
| 3 | spatial multi-modal data fusion |
| ... | ... |

## 搜索结果汇总
| 变体 | PubMed | bioRxiv | arXiv | 合计 |
|------|--------|---------|-------|------|
| spatial multi-omics integration | 3 | 1 | 0 | 4 |
| ... | ... | ... | ... | ... |

## 去重后统计
- 总相关工作数量：**X 篇**
- 已发表论文：X 篇
- 预印本：X 篇

## 创新性判断
- **级别**：[子刊级别/需要完善/需重新定位]
- **原因**：[具体说明]

## 相关工作列表
1. [论文标题]
   - 来源：[期刊/bioRxiv/arXiv]
   - 发表时间：YYYY
   - 主要方法：[摘要]
   - 与用户想法重叠度：[高/中/低]
   
2. ...

## 修改建议（如果需要）
如果坚持投子刊，建议从以下角度调整：
1. **方法角度**：...
2. **任务角度**：...
3. **数据角度**：...

## 下一步建议
- 如果子刊级别：继续 Step 2
- 如果需要完善：与用户确认修改方向后继续
- 如果需重新定位：建议重新设计研究方向
```

## 使用方式

```bash
/bio-innovation-check "空间多组学整合"
```

## 注意事项

1. **搜索时间限制**：每个平台搜索耗时不同，建议设置 timeout
2. **去重重要性**：避免重复计数影响判断
3. **重叠度评估**：需要人工判断论文与用户想法的相似程度
4. **阈值可调整**：不同子刊的创新性要求可能不同