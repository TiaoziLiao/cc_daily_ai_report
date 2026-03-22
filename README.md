# CC Daily AI Report

基于 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 的每日 AI 研究进展追踪系统。输入一条命令，自动获取论文、新闻和前沿观点，生成结构化日报。

## 效果预览

每日生成的报告包含以下板块：

```
AI 日报 | 2026-03-20
├── 大厂模型发布        # OpenAI / Google / Meta / 字节 等重磅模型，含 Tech Report 解读
├── 论文推荐            # 按 ⭐ 分级的论文深度分析，含架构图、公式、实验表格
│   ├── 今日论文总览    # 分类表格，一眼看清今日论文全貌
│   ├── ⭐⭐⭐⭐⭐ 完整深度分析
│   ├── ⭐⭐⭐⭐ 中等详细
│   └── ⭐⭐⭐ 简要信息
├── AI 新闻             # 重大新闻 + 影响分析
├── 前沿观点与播客
├── 今日趋势洞察        # 基于当日论文/新闻的趋势总结
└── 快速索引            # 全部论文速查表
```

## 快速开始

### 前置条件

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) 已安装并登录

### 使用方式

```bash
# 1. clone 项目
git clone https://github.com/TiaoziLiao/cc_daily_ai_report.git
cd cc_daily_ai_report

# 2. 启动 Claude Code
claude

# 3. 生成今日报告
/daily-ai-report

# 4. 指定日期生成
/daily-ai-report 2026-03-15
```

首次运行即可，无需任何额外配置。权限已在 `.claude/settings.local.json` 中预设，全程零确认。

## 数据源

| 来源 | 用途 | 说明 |
|------|------|------|
| [HuggingFace Daily Papers](https://huggingface.co/papers) | 社区热门论文 | 按 upvotes 排序 |
| [arXiv API](https://arxiv.org/) | 分类检索最新论文 | cs.CV / cs.AI / cs.LG / cs.CL / cs.RO |
| arXiv HTML | 论文架构图提取 | 自动嵌入方法 overview 图 |
| WebSearch | AI 新闻、模型发布、播客 | 按日期搜索 |

## 报告结构

### 大厂模型发布追踪

独立专栏，优先展示。追踪 OpenAI、Google/DeepMind、Anthropic、Meta、xAI、NVIDIA、字节跳动、阿里、百度、腾讯、MiniMax、DeepSeek 等。每个模型含：

- 属性表（类型 / 参数量 / 开源 / Tech Report 链接）
- Benchmark 对比表
- Tech Report 关键发现

### 论文推荐（按星级分层）

| 星级 | 详细度 |
|------|--------|
| ⭐⭐⭐⭐⭐ | TL;DR → 架构图 → 研究动机 → 核心方法(含公式) → 实验表格 → Insight → 亮点与不足 → 研究启发 |
| ⭐⭐⭐⭐ | TL;DR → 架构图(如有) → 核心方法(2段) → 关键结果 → Insight |
| ⭐⭐⭐ | TL;DR → 方法要点 → 值得关注 |

每篇论文标注关联度：`#directly-relevant` / `#technique-borrow` / `#inspiration`

### 评级标准

- ⭐⭐⭐⭐⭐ — 架构级突破、范式转换、重大 SOTA
- ⭐⭐⭐⭐ — 显著方法改进、跨领域迁移、重要开源
- ⭐⭐⭐ — 扎实增量改进、有用工程贡献

## 关注领域

本项目面向 AI 研究者，重点追踪以下方向（按优先级）：

1. **图像生成** — Diffusion, Flow Matching, DiT, Consistency Model
2. **视频生成** — Text-to-Video, Temporal Modeling
3. **世界模型** — World Model, Simulation
4. **VLM / 多模态** — Vision Language Model, Multimodal Reasoning
5. **LLM** — Reasoning, Long Context
6. **强化学习** — RL / RLHF / DPO / GRPO, Reward Model
7. **具身智能** — Embodied AI, Robotics, Humanoid
8. **Agent** — AI Agent, Tool Use, Planning

> 可通过修改 `.claude/commands/daily-ai-report.md` 中的关键词和优先级来自定义关注方向。

## 项目结构

```
cc_daily_ai_report/
├── .claude/
│   ├── commands/
│   │   └── daily-ai-report.md    # 命令定义（核心）
│   └── settings.local.json       # 权限配置（全程零确认）
├── CLAUDE.md                      # Claude Code 项目上下文
├── reports/                       # 日报输出
│   └── 2026/
│       └── 03/
│           ├── 2026-03-13.md
│           ├── 2026-03-16.md
│           └── ...
└── README.md
```

本项目同时是一个 [Obsidian](https://obsidian.md/) vault，可直接用 Obsidian 打开浏览报告。

## 自定义

### 修改关注方向

编辑 `.claude/commands/daily-ai-report.md`，修改「用户研究兴趣」和「关键词体系」部分。

### 修改报告格式

编辑同一文件的 Step 6 报告模板部分，可调整板块顺序、详细度、语言等。

### 添加全局命令

如果希望在任意目录运行 `/daily-ai-report`：

```bash
cp .claude/commands/daily-ai-report.md ~/.claude/commands/
```

然后将报告输出路径从 `./reports/` 改为绝对路径。

## License

MIT
