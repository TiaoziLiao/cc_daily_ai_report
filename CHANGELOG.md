# Changelog

All notable changes to this project's configuration and commands are documented here.
Each entry records **What** changed, **Why**, the chosen **Approach**, and which **Files** were modified.

Entries are in reverse chronological order (newest first).
Daily report generation (`/daily-ai-report` runs) is NOT recorded here — only changes to the system itself.

---

## 2026-03-23

### 更新研究方向优先级
- **What**: 将 VLM / Omni Model / 统一模型 / 世界模型 提升为最高优先级，图像生成降为第 4 优先级
- **Why**: 用户研究方向转变，核心关注点从图像生成转向 VLM、Omni Model、理解生成统一模型和世界模型
- **Approach**: 同步更新 CLAUDE.md（研究背景、关键词体系、评级标准、关联度标签、标签体系）、项目命令文件和全局命令文件中的优先级排序和关键词分类
- **Files**: `CLAUDE.md`, `.claude/commands/daily-ai-report.md`, `~/.claude/commands/daily-ai-report.md`

### 删除 init.md
- **What**: 移除不再需要的 init.md 初始配置文件
- **Why**: 所有相关配置已整合到 CLAUDE.md 和命令文件中，init.md 已无用
- **Approach**: 直接 git rm 删除
- **Files**: `init.md` (deleted)

---

## 2026-03-23

### Change Tracking

- **What**: 建立三层变更追踪系统（CHANGELOG.md + Memory + CLAUDE.md 指令）
- **Why**: 项目经历多轮迭代但没有记录，新会话完全丢失决策上下文，可能导致重复讨论或方向回退
- **Approach**: 三层设计 — CHANGELOG.md（完整历史，git 管理）+ Memory MEMORY.md（关键决策摘要，每次会话自动加载）+ CLAUDE.md 中的自执行指令。排除了「全放 CLAUDE.md」（会话上下文膨胀）和「只用 Memory」（200 行限制不够存完整历史）的方案
- **Files**: `CHANGELOG.md`（新建）, `CLAUDE.md`（追加变更记录规范 + 更新目录结构）, `~/.claude/projects/.../memory/MEMORY.md`（新建）

---

## 2026-03-22

### GitHub Upload

- **What**: 将项目上传到 GitHub (TiaoziLiao/cc_daily_ai_report)
- **Why**: 版本管理 + 跨设备同步
- **Approach**: 安装 gh CLI（无 sudo，直接下载二进制到 ~/.local/bin），用 gh repo create 创建仓库。因 Obsidian remotely-save 插件含 OAuth secrets 被 GitHub Push Protection 拦截，改用 orphan 分支重建干净历史并添加 .gitignore 排除 .obsidian/plugins/
- **Files**: `.gitignore`（新建）, `README.md`（新建）, 所有项目文件

### Project Portability

- **What**: 项目级命令文件改用相对路径 `./reports/`
- **Why**: 全局命令用绝对路径绑定本机，项目级命令应可移植，clone 到任何电脑都能用
- **Approach**: 将 `.claude/commands/daily-ai-report.md` 中 3 处绝对路径替换为 `./reports/`，全局命令保持绝对路径不变
- **Files**: `.claude/commands/daily-ai-report.md`

### Permission Configuration

- **What**: 权限配置改为 `Bash(*)` 通配 + 新增 `ToolSearch`
- **Why**: 执行 `/daily-ai-report` 时频繁弹出权限确认，用户希望全程零确认
- **Approach**: 用 `Bash(*)` 替代逐个列举的 Bash 命令，补上缺失的 `ToolSearch`（加载 WebSearch/WebFetch 等延迟工具 schema 的前置步骤）。同时更新用户级 `~/.claude/settings.json` 和项目级 `.claude/settings.local.json`
- **Files**: `~/.claude/settings.json`, `.claude/settings.local.json`

---

## 2026-03-21

### Architecture Figure Extraction

- **What**: 论文架构图提取从 ar5iv 改为 arXiv HTML
- **Why**: ar5iv 对新论文会 307 重定向到 arxiv.org 导致不可用；arXiv 自带 HTML 版本覆盖率更高
- **Approach**: 优先源改为 `arxiv.org/html/{id}v1`，图片 URL 格式 `https://arxiv.org/html/{id}v1/xN.png`（已验证返回 HTTP 200）。ar5iv 保留为备用源。5 星论文必须嵌入架构图，4 星尽量
- **Files**: `~/.claude/commands/daily-ai-report.md`（Step 5.1）, `.claude/commands/daily-ai-report.md`, `CLAUDE.md`（数据源表）

### Paper Overview Table

- **What**: 论文推荐板块开头新增论文总览表格
- **Why**: 用户希望先有全局视图再看详细分析，一张表快速了解今日论文的主题/推荐度/关联度分布
- **Approach**: 在论文详情之前插入分类表格（#/标题/主题领域/推荐度/关联度/类型/一句话总结）+ 今日主题分布统计
- **Files**: `~/.claude/commands/daily-ai-report.md`（Step 6 模板）, `.claude/commands/daily-ai-report.md`

---

## 2026-03-20

### Enhanced Report Structure

- **What**: 大幅增强报告结构化和详细度，参考 init.md 规范
- **Why**: 初版报告过于简略，论文仅 1 段概述，缺少技术细节、公式、实验数据、架构图
- **Approach**: 论文按星级分三档详细度（5 星完整深度分析 / 4 星中等 / 3 星简要）。新增关联度标签（#directly-relevant / #technique-borrow / #inspiration）、今日趋势洞察板块、新闻影响等级。快速索引表增加关联度和一句话总结列
- **Files**: `~/.claude/commands/daily-ai-report.md`（Step 4-6）, `.claude/commands/daily-ai-report.md`, `CLAUDE.md`

### Model Release Section

- **What**: 新增独立的「大厂模型发布」专栏
- **Why**: 用户希望重点关注各大厂发布的模型及 Tech Report，不被淹没在普通新闻中
- **Approach**: 在报告最前面（论文之前）设独立板块。追踪 OpenAI/Google/Anthropic/Meta/xAI/NVIDIA/字节/阿里/百度/腾讯/MiniMax/DeepSeek 等。每个模型含属性表（类型/参数量/开源/Tech Report）+ Benchmark 对比表 + Tech Report 关键发现。当日无发布时回溯 3 天
- **Files**: `~/.claude/commands/daily-ai-report.md`（Step 3 任务 B + Step 6 模板）, `CLAUDE.md`

---

## 2026-03-19

### Initial Project Setup

- **What**: 创建 cc_daily_ai_report 项目和 `/daily-ai-report` 命令
- **Why**: 用户希望通过 Claude Code CLI 一键生成每日 AI 研究进展报告
- **Approach**: 全局命令放在 `~/.claude/commands/`（任意目录可用），项目级命令放在 `.claude/commands/`（随 git 同步）。数据源：HuggingFace Daily Papers API + arXiv API + WebSearch。关键词三级过滤（核心/扩展/探索），重要性 3-5 星评级。报告按日期归档到 `reports/YYYY/MM/YYYY-MM-DD.md`
- **Files**: `~/.claude/commands/daily-ai-report.md`（新建）, `.claude/settings.local.json`（新建）, `CLAUDE.md`（新建）, `init.md`（参考）
