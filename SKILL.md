---
name: smart-citation-finder
description: 智能引文检索与论证助手 — 分析文章内容，自动在中英文权威数据库中检索文献为每个论点提供支撑，生成GB/T 7714-2015格式引文列表。触发词：找文献、检索引文、文献论证、论点支撑。
trigger: "找文献,检索引文,文献论证,论点支撑,根据文章内容检索,claim support"
author: OpenClaw Community
created: 2026-05-25
tags: [research, citation, literature-search, nsfc, academic]
---

# Smart Citation Finder — 智能引文检索技能

## 核心功能
分析给定文章或研究内容，自动从多个权威数据库检索中英文文献，为每个关键论点提供权威支撑，并生成符合GB/T 7714-2015标准的引文列表。

## 支持的数据库
- **中文**：CNKI（中国知网）、万方数据、WanFang
- **英文**：PubMed、Web of Science、Google Scholar、OpenAlex、arXiv
- **交叉**：同步检索避免重复、交叉验证引用真实性

## 工作流程

### 第一步：分析内容，提取论点
对输入内容进行结构化分析：
1. 识别**研究背景/问题**（前1-2段）
2. 识别**核心论点/假设**（每段首句）
3. 识别**方法/技术描述**（含专业术语的句子）
4. 识别**数据/结论引用**（含统计数字的句子）
5. 识别**政策/标准引用**（法规、指南、规范）

### 第二步：生成检索词
对每个论点生成多语言检索词：
- 中文检索词（同义词扩展）
- 英文检索词（MeSH terms + 同义词）
- 检索式组合（AND/OR/NOT）

### 第三步：多库并行检索
使用以下技能并行检索：
- `academic-search-zh`：中文文献综合检索
- `pubmed-search`：PubMed英文文献
- `openalex-database`：跨学科英文文献
- `wanfang-research`：万方数据（中文）
- `cnki-advanced-search`：知网高级检索

### 第四步：文献质量评估
每个候选文献评估：
- **相关性评分**（0-10）：基于关键词命中、摘要语义相似度
- **权威性评分**：期刊影响因子、作者H指数、被引次数
- **时效性评分**：近5年文献优先
- **类型权重**：论著 > 综述 > 病例报告 > 会议摘要

### 第五步：构建论证矩阵
输出格式：

```
## 论点1：[核心主张一句话]
- 支撑文献：[#文献编号] 作者等, 年份. 标题. 期刊, 卷(期): 页码.
  - 证据强度：强/中/弱（说明）
  - 检索来源：PubMed/CNKI/万方

## 论点2：...
```

### 第六步：生成引文列表
按GB/T 7714-2015（顺序编码制）输出完整参考文献列表。

## 输入格式

**格式A（文件路径）**：
```
skill:smart-citation-finder
input: /path/to/manuscript.docx
style: gb7714
min_refs_per_claim: 2
```

**格式B（直接文本）**：
```
skill:smart-citation-finder
input: |
  [粘贴文章正文内容]
style: gb7714
focus_topics: ["医院数据治理", "抗肿瘤药物监测"]
```

**格式C（主题检索）**：
```
skill:smart-citation-finder
input: 研究方向：医院处方点评系统设计与应用
style: gb7714
claim_mode: true
```

## 配置参数
| 参数 | 默认值 | 说明 |
|------|--------|------|
| `style` | gb7714 | 引文格式（gb7714/apa/ieee） |
| `min_refs_per_claim` | 2 | 每个论点最少支撑文献数 |
| `max_total_refs` | 50 | 最大总引文数 |
| `lang_preference` | auto | 语言偏好（zh/en/auto） |
| `year_range` | 2019-2026 | 检索时间范围 |
| `focus_topics` | [] | 重点关注领域关键词 |
| `claim_mode` | false | true=输入是研究方向，生成通用论证 |

## 输出结构
1. **论证矩阵**（Markdown表格）
2. **完整参考文献列表**（GB/T 7714-2015格式）
3. **文献检索报告**（包含数据库、检索式、命中数）

## 错误处理
- 数据库连接失败：自动切换到备用数据库
- 检索结果为空：扩大检索词或降低相关性阈值
- 文献无法获取：标记为"需手动确认"并提供DOI/URL

## 使用示例

**示例1：国自然申请书引文补充**
```
skill:smart-citation-finder
input: /Volumes/NAS/申报材料/申请书v36.md
style: gb7714
focus_topics: ["数据治理", "医院数据资产", "抗肿瘤药物管理"]
```

**示例2：直接输入研究内容**
```
skill:smart-citation-finder
input: |
  本研究拟构建医院抗肿瘤药物临床应用监测系统，
  通过处方前置审核实现药物合理使用。已有研究表明，
  临床决策支持系统可显著降低用药错误率...
style: gb7714
claim_mode: false
```

**示例3：研究方向通用检索**
```
skill:smart-citation-finder
input: 人工智能辅助临床诊断在消化系统疾病中的应用
style: gb7714
claim_mode: true
```

## 支持文件
- `references/citation-search-prompts.md` — 论点提取、文献评估、GB/T 7714格式化 的标准Prompt模板，供agent在多轮检索中直接调用。