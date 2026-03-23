# AI Daily Report — Claude Code 项目配置

## 项目简介

这是一个基于 Claude Code 的每日 AI 研究进展追踪系统。通过 `/daily-ai-report` 命令自动获取论文、新闻和前沿观点，生成结构化的中英混合日报，并以 Markdown 文件形式归档管理。本项目同时作为 Obsidian vault 使用，支持双向链接和知识图谱。

## 目录结构

```
cc_daily_ai_report/
├── CLAUDE.md              # 本文件 — Claude Code 项目级上下文
├── CHANGELOG.md           # 变更历史（What/Why/Approach/Files）
├── reports/               # 日报输出目录
│   └── {YYYY}/
│       └── {MM}/
│           └── {YYYY-MM-DD}.md   # 每日报告
└── .claude/
    ├── commands/
    │   └── daily-ai-report.md    # 命令定义（核心）
    └── settings.local.json       # 权限配置
```

## 用户研究背景

**主要方向**: VLM / Omni Model / 理解生成统一模型 / 世界模型

**关注领域（按优先级）**:

1. **VLM / 多模态** — Vision Language Model, Multimodal Reasoning, Visual Understanding
2. **Omni Model / 统一模型** — Unified Understanding & Generation, Any-to-Any, Multimodal Generation
3. **世界模型** — World Model, Simulation, Physical Understanding, Video Prediction
4. **图像生成** — Diffusion, Flow Matching, DiT, Consistency Model, Autoregressive
5. **视频生成** — Text-to-Video, Temporal Modeling, Video Understanding
6. **LLM** — Large Language Model, Reasoning, Long Context
7. **强化学习** — RL / RLHF / DPO / GRPO, Reward Model（尤其 RL 优化生成质量）
8. **具身智能** — Embodied AI, Robotics, Manipulation, Humanoid
9. **Agent** — AI Agent, Tool Use, Planning

## 关键词体系

### 核心关键词（必须优先展示）

vision language model, VLM, multimodal understanding, omni model, unified model, any-to-any, multimodal generation, unified understanding and generation, world model, world simulator, visual reasoning, image understanding

### 扩展关键词（高优先级）

diffusion, text-to-image, image generation, image synthesis, video generation, text-to-video, flow matching, rectified flow, consistency model, DiT, diffusion transformer, autoregressive image, visual tokenizer, RLHF, reinforcement learning, reward model, DPO, GRPO, PPO, LLM, large language model, reasoning, chain-of-thought, agent, tool use, embodied, robotics, manipulation, humanoid

### 探索关键词（补充）

image editing, inpainting, controlnet, 3D generation, multi-view, visual tokenizer, autoregressive image, video understanding, temporal consistency, efficient training, data curation, distillation, scaling law, photorealism, GAN, VAE, tokenizer

## 大厂模型发布追踪

报告中设有独立的「大厂模型发布」专栏，优先于论文推荐展示。

**追踪对象**:
- 国际: OpenAI, Google/DeepMind, Anthropic, Meta/FAIR, xAI, NVIDIA, Microsoft, Apple, Mistral, Stability AI
- 国内: 字节跳动/豆包, 阿里/通义, 百度/文心, 腾讯/混元, MiniMax, 智谱AI, DeepSeek, 月之暗面/Kimi, 零一万物

**每个模型需展示**: 模型名称、发布方、类型、参数量、关键能力与 benchmark 表格、是否开源、Tech Report 链接（arXiv 或 Blog）、与前代/竞品对比亮点、Tech Report 关键发现。当日无新发布时回溯近 3 天。

## 数据源

| 数据源 | 用途 | 获取方式 |
|--------|------|----------|
| HuggingFace Daily Papers | 每日热门论文 | `curl -s "https://huggingface.co/api/daily_papers"` |
| arXiv API | 分类检索最新论文 | `curl -s "https://export.arxiv.org/api/query?search_query=cat:cs.CV+OR+cat:cs.AI+OR+cat:cs.LG+OR+cat:cs.CL+OR+cat:cs.RO&sortBy=submittedDate&sortOrder=descending&max_results=50"` |
| arXiv HTML | 论文 HTML 版（提取架构图，优先） | WebFetch `https://arxiv.org/html/{arxiv_id}v1`，图片 URL: `https://arxiv.org/html/{arxiv_id}v1/xN.png` |
| ar5iv | 论文 HTML 版（备用） | WebFetch `https://ar5iv.labs.arxiv.org/html/{arxiv_id}` |
| WebSearch | AI 新闻、模型发布、播客 | 使用 WebSearch 工具按日期搜索 |

## 论文重要性评级

- **⭐⭐⭐⭐⭐**: 架构级突破、范式转换、重大 SOTA、顶级团队重量级工作
- **⭐⭐⭐⭐**: 显著方法改进、新颖跨领域迁移、重要开源发布
- **⭐⭐⭐**: 扎实增量改进、有用工程贡献、有趣实验发现
- 与用户核心兴趣（VLM、Omni Model、统一模型、世界模型）直接相关的论文额外 +1 星

## 论文详细度分级

- **⭐⭐⭐⭐⭐（完整深度分析）**: TL;DR(3-5句) → 架构图 → 研究动机 → 核心方法(3-4段+公式) → 实验表格 → 关键Insight(3条) → 亮点与不足 → 对我研究的启发
- **⭐⭐⭐⭐（中等详细）**: TL;DR(2-3句) → 架构图(如有) → 核心方法(2段) → 关键结果 → Insight/启发(2-3条)
- **⭐⭐⭐（简要但信息完整）**: TL;DR(1-2句) → 方法要点(1段+关键数字) → 值得关注(1句)

## 关联度标签

- `#directly-relevant`: 与用户核心方向（VLM、Omni Model、统一模型、世界模型）直接相关
- `#technique-borrow`: 方法/技巧可迁移借鉴到用户方向
- `#inspiration`: 提供思路启发但非直接相关

## 报告规范

- **语言**: 中英混合 — 论文标题、术语、作者用英文；分析和描述用中文
- **详细度**: 按星级分层（见上方论文详细度分级）
- **板块顺序**: 大厂模型发布 → 论文推荐 → AI 新闻 → 前沿观点 → 今日趋势洞察 → 快速索引
- **格式**: Markdown with YAML frontmatter，包含快速索引表（含关联度和一句话总结）
- **输出路径**: `reports/{YYYY}/{MM}/{YYYY-MM-DD}.md`

## 标签体系

- **领域**: `#VLM` `#OmniModel` `#UnifiedModel` `#WorldModel` `#Multimodal` `#ImageGen` `#VideoGen` `#Diffusion` `#FlowMatching` `#DiT` `#RL` `#RLHF` `#3DGen` `#ImageEdit` `#LLM` `#Agent` `#Embodied`
- **类型**: `#survey` `#method` `#benchmark` `#application` `#theory` `#system`
- **关联度**: `#directly-relevant` `#technique-borrow` `#inspiration`

## 编写代码和操作文件的约定

- 报告文件统一使用 UTF-8 编码
- 日期格式统一使用 `YYYY-MM-DD`
- arXiv 链接统一使用 `https://arxiv.org/abs/{id}` 格式
- 图片嵌入使用 Markdown 标准语法 `![desc](url)`
- 论文去重以 arXiv ID 为准，合并 HuggingFace 和 arXiv 两个来源

## 变更记录规范

每次修改项目文件（CLAUDE.md、命令文件、设置、报告模板等）后，**必须**：

1. **更新 CHANGELOG.md** — 在最新日期下追加结构化条目：
   - **What**: 一句话描述变更
   - **Why**: 为什么要做这个改动
   - **Approach**: 选择了什么方案（及排除其他方案的理由）
   - **Files**: 修改的文件列表

2. **更新 Memory**（如变更涉及设计决策）：
   - 在 `~/.claude/projects/-Users-bytedance-Desktop-code-cc_daily_ai_report/memory/MEMORY.md` 的 Active Decisions 中新增/修改/删除过时条目
   - 保持 MEMORY.md 在 150 行以内

3. **不记录**：`/daily-ai-report` 日常运行。只记录系统本身的变更。

新会话修改项目前，先读 CHANGELOG.md 了解近期变更。
