# Smart Citation Finder — 智能引文检索技能

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-green.svg)

**智能引文检索与论证助手** — 分析文章内容，自动在中英文权威数据库中检索文献，为每个关键论点提供权威支撑，并生成符合 GB/T 7714-2015 标准的引文列表。

---

## 🎯 核心功能

| 功能 | 说明 |
|------|------|
| **论点自动提取** | 扫描文章内容，识别研究背景、核心假设、方法描述、数据结论 |
| **多库并行检索** | CNKI + 万方 + PubMed + OpenAlex + arXiv 同步检索 |
| **中英文双检** | 每个论点同时生成中英文检索词，保证外文引用比例 ≥60% |
| **权威性评估** | 基于影响因子、被引次数、期刊级别自动评分排序 |
| **GB/T 7714-2015** | 输出符合国自然/SCI期刊标准的顺序编码制参考文献列表 |

---

## 🚀 在 OpenClaw 中使用

```
skill:smart-citation-finder
input: /path/to/manuscript.docx
style: gb7714
focus_topics: ["医院数据治理", "抗肿瘤药物"]
```

---

## 📋 支持的输入模式

### 模式A：文件路径
```
input: /path/to/manuscript.docx
```

### 模式B：直接粘贴正文
```
input: |
  本研究拟构建医院抗肿瘤药物临床应用监测系统...
style: gb7714
```

### 模式C：研究方向（通用论证）
```
input: 人工智能辅助临床诊断在消化系统疾病中的应用
style: gb7714
claim_mode: true
```

---

## ⚙️ 配置参数

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `style` | gb7714 | 引文格式（gb7714/apa/ieee） |
| `min_refs_per_claim` | 2 | 每个论点最少支撑文献数 |
| `max_total_refs` | 50 | 最大总引文数 |
| `lang_preference` | auto | 语言偏好（zh/en/auto） |
| `year_range` | 2019-2026 | 检索时间范围 |
| `focus_topics` | [] | 重点关注领域关键词 |
| `claim_mode` | false | true=输入是研究方向，生成通用论证 |

---

## 🔍 工作流程

1. **分析内容** → 提取研究背景、核心论点、方法描述
2. **生成检索词** → 中英文 MeSH terms + 同义词扩展
3. **多库并行检索** → PubMed / CNKI / 万方 / OpenAlex / arXiv
4. **文献质量评估** → 相关性 + 权威性 + 时效性三维评分
5. **构建论证矩阵** → 每论点附支撑文献及证据强度
6. **输出引文列表** → GB/T 7714-2015 格式完整参考文献

---

## 📦 输出结构

1. **论证矩阵**（Markdown表格）
2. **完整参考文献列表**（GB/T 7714-2015格式）
3. **文献检索报告**（数据库、检索式、命中数）

---

## ⚠️ 免责声明

- 本技能仅供学习和参考
- 引文真实性请通过 DOI/URL 人工核实
- 政策信息以官方最新指南为准

---

**创建时间**：2026-05-25
