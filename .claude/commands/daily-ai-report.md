---
description: 生成每日 AI 研究进展报告（论文 + 新闻 + 前沿观点）
argument-hint: "[YYYY-MM-DD] 可选日期参数，默认今天"
allowed-tools: Bash(curl:*), Bash(mkdir:*), Bash(date:*), Bash(ls:*), Read, Write, Edit, WebSearch, WebFetch, Agent, TodoWrite
---

# Daily AI Report Generator

你是一个专业的 AI 研究进展追踪助手。你的任务是生成一份结构化的每日 AI 研究报告。

## 用户研究兴趣

以下是用户关注的核心领域，按优先级排列：
1. VLM / 多模态（Vision Language Model, Multimodal Reasoning, Visual Understanding）
2. Omni Model / 统一模型（Unified Understanding & Generation, Any-to-Any, Multimodal Generation）
3. 世界模型（World Model, Simulation, Physical Understanding, Video Prediction）
4. 图像生成（Diffusion Model, Flow Matching, DiT, Autoregressive Image Generation）
5. 视频生成（Text-to-Video, Temporal Modeling, Video Understanding）
6. LLM（Large Language Model, Reasoning, Long Context）
7. 强化学习（RL/RLHF/DPO/GRPO, Reward Model）
8. 具身智能（Embodied AI, Robotics, Manipulation）
9. Agent（AI Agent, Tool Use, Planning）

## 配置

- **报告输出目录**: `./reports/`
- **语言**: 中英混合 — 论文标题、术语、作者用英文；分析和描述用中文；新闻类内容用中文
- **详细度**: 按星级分层 — 5 星完整深度分析，4 星中等详细，3 星简要但信息完整
- **关联度标签**: `#directly-relevant`（直接相关）/ `#technique-borrow`（可借鉴技术）/ `#inspiration`（启发性）

## 执行步骤

### Step 1: 确定日期

- 如果用户提供了日期参数 `$ARGUMENTS`，使用该日期
- 否则用 `date +%Y-%m-%d` 获取今天日期
- 设定变量：REPORT_DATE, YEAR, MONTH

### Step 2: 创建目录

```bash
mkdir -p ./reports/{YEAR}/{MONTH}/
```

### Step 3: 并行获取数据

同时启动以下数据获取任务（使用 Agent 工具并行化）：

#### 任务 A: 获取论文

**数据源 1 — HuggingFace Daily Papers**:
```bash
curl -s "https://huggingface.co/api/daily_papers"
```
返回 JSON 数组，每个元素包含 `title`, `paper.summary`, `paper.id`(arXiv ID), `publishedAt` 等字段。

**数据源 2 — arXiv API**:
按用户关注的分类搜索最新论文：
```bash
curl -s "https://export.arxiv.org/api/query?search_query=cat:cs.CV+OR+cat:cs.AI+OR+cat:cs.LG+OR+cat:cs.CL+OR+cat:cs.RO&sortBy=submittedDate&sortOrder=descending&max_results=50"
```

对于特定日期回溯，使用：
```bash
curl -s "https://export.arxiv.org/api/query?search_query=(cat:cs.CV+OR+cat:cs.AI+OR+cat:cs.LG)+AND+submittedDate:[YYYYMMDD0000+TO+YYYYMMDD2359]&sortBy=submittedDate&sortOrder=descending&max_results=50"
```

#### 任务 B: 获取大厂模型发布（重点关注）

使用 WebSearch 专门搜索各大 AI 实验室/公司近期发布的重要模型：
- `"new AI model release {REPORT_DATE}"` — 模型发布
- `"OpenAI Google Anthropic Meta model launch {REPORT_DATE}"` — 大厂模型发布
- `"tech report arxiv {REPORT_DATE} model"` — 技术报告
- `"开源模型发布 {REPORT_DATE}"` — 中文圈模型发布

**重点追踪的公司/实验室**：
- 国际: OpenAI, Google/DeepMind, Anthropic, Meta/FAIR, xAI, NVIDIA, Microsoft, Apple, Mistral, Stability AI
- 国内: 字节跳动/豆包, 阿里/通义, 百度/文心, 腾讯/混元, MiniMax, 智谱AI, DeepSeek, 月之暗面/Kimi, 零一万物

**每个模型需收集**：
- 模型名称和版本
- 发布公司/实验室
- 模型类型（LLM / VLM / 图像生成 / 视频生成 / 代码 / 多模态等）
- 关键能力和 benchmark 表现
- Tech Report / 论文链接（如有 arXiv 链接务必标注）
- 是否开源（开源地址）
- 与前代或竞品的对比亮点

如果当日无重大模型发布，搜索近 3 天内的发布以补充。

#### 任务 C: 获取 AI 新闻

使用 WebSearch 搜索以下内容（使用报告日期）：
- `"AI news {REPORT_DATE}"` — AI 领域重要新闻
- `"AI product launch {REPORT_DATE}"` — 新产品发布
- `"AI open source release {REPORT_DATE}"` — 开源项目发布
- `"artificial intelligence breakthrough {REPORT_DATE}"` — 重大突破

#### 任务 D: 获取前沿观点与播客

使用 WebSearch 搜索：
- `"AI frontier opinion {REPORT_DATE}"` — 行业领袖观点
- `"AI podcast episode {REPORT_DATE}"` — AI 播客新剧集
- `"AI research blog post {REPORT_DATE}"` — 技术博客

### Step 4: 论文筛选与排名

**关键词匹配过滤**（三级优先）：

**核心关键词**（必须优先展示）：
vision language model, VLM, multimodal understanding, omni model, unified model, any-to-any, multimodal generation, unified understanding and generation, world model, world simulator, visual reasoning, image understanding

**扩展关键词**（高优先级）：
diffusion, text-to-image, image generation, image synthesis, video generation, text-to-video, flow matching, rectified flow, consistency model, DiT, diffusion transformer, autoregressive image, visual tokenizer, RLHF, reinforcement learning, reward model, DPO, GRPO, PPO, LLM, large language model, reasoning, chain-of-thought, agent, tool use, embodied, robotics, manipulation

**探索关键词**（补充）：
image editing, inpainting, controlnet, 3D generation, multi-view, visual tokenizer, autoregressive image, video understanding, temporal consistency, efficient training, data curation, distillation, scaling law

**重要性评级标准**：
- ⭐⭐⭐⭐⭐: 架构层面的突破创新、范式转换、重大 SOTA 刷新、顶级团队（OpenAI/Google/Meta/DeepMind）的重量级工作
- ⭐⭐⭐⭐: 显著的方法改进、新颖的跨领域方法迁移、重要的开源模型/数据集发布
- ⭐⭐⭐: 扎实的增量改进、有用的工程贡献、有趣的实验发现
- 与用户核心兴趣（VLM、Omni Model、统一模型、世界模型）直接相关的论文额外 +1 星

**关联度标签**（为每篇论文标注与用户研究方向的关系）：
- `#directly-relevant`: 与用户核心方向（VLM、Omni Model、统一模型、世界模型）直接相关
- `#technique-borrow`: 方法/技巧可迁移借鉴到用户方向
- `#inspiration`: 提供思路启发但非直接相关

**筛选规则**：
- 从所有获取的论文中，选出与关键词匹配的论文
- 合并 HuggingFace 和 arXiv 来源，按 arXiv ID 去重
- 按重要性排序，展示 Top 10-15 篇
- 剩余相关论文放入快速索引表

### Step 5: 论文深度信息获取

对于 ⭐⭐⭐⭐⭐ 论文（**必须**）和 ⭐⭐⭐⭐ 论文（**尽量**）：

**5.1 架构图获取**（⭐⭐⭐⭐⭐ **必须**，⭐⭐⭐⭐ 尽量）:

使用 WebFetch 访问 **arXiv HTML 版本**获取论文图片：

**优先源 — arXiv HTML**（推荐，覆盖率高）:
```
https://arxiv.org/html/{arxiv_id}v1
```
用 WebFetch 提取页面中 `<img>` 标签的 `src` 属性。图片完整 URL 拼接规则：
```
https://arxiv.org/html/{arxiv_id}v1/{img_src}
```
提取 prompt: `"Find all figure images (<img> tags). For each figure return: figure number, img src path, caption text. Focus on method overview / architecture / pipeline diagrams (usually Figure 1 or 2)."`

**备用源 — ar5iv**（部分旧论文）:
```
https://ar5iv.labs.arxiv.org/html/{arxiv_id}
```
仅当 arXiv HTML 返回 404 或无图片时尝试。

**选图策略**:
- 优先选择 caption 中含 "overview" / "framework" / "architecture" / "pipeline" / "method" 的图
- 通常为 Figure 1 或 Figure 2
- 如果 Figure 1 是效果对比图而非方法图，选 Figure 2 或 Figure 3

**嵌入格式**:
```markdown
![Figure X: Caption Text](https://arxiv.org/html/{arxiv_id}v1/xN.png)
```

**失败处理**: 如 HTML 版本不可用（404/重定向/无图片），跳过图片嵌入，不需要标注。

**5.2 实验数据提取**: 从论文 abstract/summary 中提取关键 benchmark 数字（FID, CLIP Score, accuracy, throughput 等），组织为对比表格。

**5.3 关键公式**（仅 5 星）: 从论文中提取 1-2 个最核心的公式，用 LaTeX 写出。

### Step 6: 生成报告

将所有内容整合，按以下模板写入报告文件。**注意**：论文详细度严格按星级分层，确保高星论文有足够技术深度，让读者不看原文也能理解核心内容。

**输出路径**: `./reports/{YEAR}/{MONTH}/{REPORT_DATE}.md`

```markdown
---
date: {REPORT_DATE}
total_papers: N
model_releases: N
tags: [ImageGen, VideoGen, LLM, RL, ...]
top_paper: "最重要的论文标题"
---

# AI 日报 | {REPORT_DATE}

> 本报告由 Claude Code `/daily-ai-report` 自动生成

## 今日概览

- 🔥 大厂模型发布: N 个
- 📄 精选论文: N 篇（⭐⭐⭐⭐⭐: X 篇, ⭐⭐⭐⭐: Y 篇, ⭐⭐⭐: Z 篇）
- 📰 AI 新闻: N 条
- 🎙️ 前沿观点/播客: N 条

---

## 🔥 大厂模型发布

> 各 AI 实验室/公司近期发布的重要模型及技术报告。如当日无新发布则展示近 3 日内的重要发布。

### 🔥 Model Name (e.g. GPT-5.3 Codex)
| 属性 | 信息 |
|------|------|
| **发布方** | OpenAI / Google / ... |
| **类型** | LLM / VLM / 图像生成 / ... |
| **参数量** | XXB / 未公开 |
| **关键能力** | 简述核心能力和主要 benchmark 表现 |
| **开源** | ✅ 开源 ([HuggingFace](url) / [GitHub](url)) / ❌ 闭源 |
| **Tech Report** | [arXiv](https://arxiv.org/abs/XXXX.XXXXX) 或 [Blog](url) |

**技术亮点**: 架构创新 / 训练方法 / 数据策略（2-3 段详细描述，含关键数字）

**Benchmark 表现**:
| Benchmark | Score | vs 前代 | vs 竞品最佳 |
|-----------|-------|---------|------------|
| ... | ... | +X% | ... |

**Tech Report 关键发现**（如有 tech report）:
1. 发现 1
2. 发现 2
3. 发现 3

---

（更多模型...）

---

## 📄 论文推荐

### 今日论文总览

> 按主题领域分类，快速了解今日推荐论文全貌。点击标题跳转详细分析。

| # | 论文标题 | 主题领域 | 推荐度 | 关联度 | 类型 | 一句话总结 |
|---|---------|---------|--------|--------|------|-----------|
| 1 | [Paper A](#paper-a-anchor) | 图像生成 | ⭐⭐⭐⭐⭐ | #directly-relevant | #method | 一句话 |
| 2 | [Paper B](#paper-b-anchor) | 视频生成 | ⭐⭐⭐⭐ | #technique-borrow | #method | 一句话 |
| ... | ... | ... | ... | ... | ... | ... |

**今日主题分布**:
- 🧠 VLM / 多模态理解: X 篇
- 🌐 Omni Model / 统一模型: X 篇
- 🌍 世界模型: X 篇
- 🎨 图像生成/编辑: X 篇
- 🎬 视频生成: X 篇
- 💬 LLM: X 篇
- 🤖 RL / Reward Model: X 篇
- 🦾 具身智能 / Agent: X 篇
- 🔧 其他（效率/蒸馏/Benchmark）: X 篇

---

（以下按推荐度降序排列。5 星完整深度分析，4 星中等详细，3 星简要信息完整）

### ⭐⭐⭐⭐⭐ Paper Title
**Authors** | Institution | [arXiv](url) | [Code](github_url)
**领域**: #ImageGen #Diffusion | **类型**: #method
**Upvotes**: N | **关联度**: #directly-relevant / #technique-borrow / #inspiration

> **TL;DR**: 3-5 句话概括：解决什么问题 → 用什么方法 → 取得什么结果。要具体，不要空泛。

**架构图**:
![Figure 1: Method Overview](ar5iv_image_url)

**研究动机**:
- 现有方法 X 的具体局限是什么（2-3 点）
- 为什么需要新方法，gap 在哪里

**核心方法**:
详细描述方法流程（3-4 段），包含：
- 整体框架设计思路
- 关键模块的具体做法
- 关键公式（用 LaTeX）：$\mathcal{L} = ...$
- 与架构图对应的步骤解释

**实验结果**:
| Method | FID↓ | CLIP Score↑ | ... |
|--------|------|-------------|-----|
| Baseline A | xx.x | xx.x | ... |
| Baseline B | xx.x | xx.x | ... |
| **Ours** | **xx.x** | **xx.x** | ... |

关键实验发现（2-3 点，带具体数字）

**关键 Insight**:
1. Insight 1（2-3 句话解释为什么重要）
2. Insight 2
3. Insight 3

**亮点与不足**:
- ✅ 亮点：...
- ❌ 不足/局限：...

**对我研究的启发**: 具体到可借鉴的模块、训练技巧、实验设计

---

### ⭐⭐⭐⭐ Paper Title
**Authors** | [arXiv](url) | [Code](github_url)
**领域**: #tags | **关联度**: #directly-relevant / #technique-borrow / #inspiration

> **TL;DR**: 2-3 句话概括

**架构图**: ![Fig](url) ← 如可获取

**核心方法**: 2 段详细描述，包含关键创新点和技术细节

**关键结果**: 列出主要 benchmark 数字和对比

**Insight / 启发**: 2-3 条

---

### ⭐⭐⭐ Paper Title
**Authors** | [arXiv](url)
**领域**: #tags | **关联度**: #directly-relevant / #technique-borrow / #inspiration

> **TL;DR**: 1-2 句话

**方法要点**: 1 段核心方法 + 关键数字

**值得关注**: 1 句话说明与用户方向的关联

---

## 📰 AI 新闻与产品发布

### 新闻标题
**来源**: Source | [链接](url) | **影响等级**: 🔴高 / 🟡中 / 🟢低

简要事实描述（2-3 句话）

**影响分析**: 对 AI 研究/产业的影响（1-2 句）

---

## 🎙️ 前沿观点与播客

### 标题
**作者/主讲**: Person | [链接](url)

核心观点摘要（中文，3-5 句话）

---

## 📊 今日趋势洞察

基于今日论文和新闻，总结 2-3 个值得关注的趋势或模式：

1. **趋势名称**: 分析描述（3-4 句话），引用今日相关论文/新闻
2. **趋势名称**: ...

---

## 📋 快速索引

| 论文 | 领域 | 重要性 | 关联度 | 一句话总结 | 链接 |
|------|------|--------|--------|-----------|------|
| Paper Title | #tag | ⭐⭐⭐⭐ | #directly-relevant | 一句话总结 | [arXiv](url) |

---

*Generated by Claude Code `/daily-ai-report` | {REPORT_DATE}*
```

### Step 7: 输出总结

报告生成完毕后，在终端输出简短总结：
- 报告保存路径
- 大厂模型发布（如有）
- 今日精选论文数量和最重要的 2-3 篇论文标题
- 是否有重大新闻
