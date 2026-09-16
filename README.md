# Storybook Generator Skill

一套把“故事想法”推进到“可连续出图、可排版、可上架验证”的绘本生产 skill。标准 Agent Skills 格式，不绑定任何 agent 工具，可安装进 Codex、Claude Code、ZCode 或任何支持 skills 目录约定的 agent。

A portable Agent Skill that turns a rough story idea into a structured picture-book MVP: story beats, page plan, character/style bible, page-level image prompts, layout rules, QA checks, and publishing notes. Works in any agent that supports the skills directory convention.

## 为什么它不只是一个 prompt

大多数绘本生成失败，不是因为一句 prompt 写得不够华丽，而是因为生产链断了：

- 故事没有页间因果，只是一组漂亮插画。
- 主角跨页漂移，衣服、体型、表情和道具都不稳定。
- 正文说到的东西画面里没有，画面里出现的东西正文又没交代。
- 图片模型直接写正文，导致错字、乱码、排版失控。
- 封面、内页、三语文字、KDP 上架描述和 QA 没有连成一套流程。

`storybook-generator` 解决的是整条链路。它要求 agent 先建立故事节奏、角色连续性、视觉锚点和图文契约，再逐页生成提示词，最后用确定性排版和 QA 清单把书做成可交付的样书。

## What Makes It Different

Most AI storybook workflows fail at the production layer, not the imagination layer.

`storybook-generator` gives any AI agent a full editorial and production workflow:

- story architecture before image generation
- page-by-page narrative causality and page-turn hooks
- character continuity rules for multi-page illustration
- prompt templates that preserve visible evidence and avoid visual drift
- layout rules for Chinese, pinyin, English, and mixed-language pages
- a QA checklist for story coherence, child safety, image/text alignment, and publishing readiness
- optional commercial workflow notes for KDP-style MVP validation

It is designed for people who want a real picture-book pipeline, not a pile of disconnected image prompts.

## Capabilities

- Turn a topic, lesson, short text, or character idea into a picture-book MVP.
- Create a page plan with narrative function, page-turn hook, visual evidence, and text draft.
- Build a character and style bible before image generation.
- Write stable page-level prompts for whatever image model you use.
- Keep visible objects, actions, hands, props, and scene anchors consistent across pages.
- Add layout guidance for no-text illustrations, cover text, pinyin, Chinese, and English.
- Check finished pages for story logic, illustration defects, text errors, and child-appropriate content.
- Extend a book concept into publishing notes, product positioning, and series expansion.

## 适合什么场景

- “帮我做一本儿童绘本”
- “把这个故事拆成 12 页绘本”
- “给我整本绘本的角色圣经和分镜 prompt”
- “保持同一个主角连续出图”
- “做中文版/拼音版/中英双语版绘本页面”
- “做一本可用于 KDP 测试的绘本 MVP”
- “检查这本绘本哪里不连贯、哪里要重出图”

## 安装到任意 agent 工具

本 skill 采用标准 Agent Skills 结构（`SKILL.md` + `references/`），frontmatter 只有通用的 `name` 和 `description` 字段，不依赖任何厂商专属配置。克隆到对应工具的 skills 目录即可：

| Agent 工具 | 安装路径 |
|---|---|
| OpenAI Codex | `~/.codex/skills/storybook-generator/` |
| Claude Code | `~/.claude/skills/storybook-generator/`（全局）或项目内 `.claude/skills/` |
| ZCode | `~/.zcode/skills/storybook-generator/` |
| 其他支持 skills 的 agent | `~/.agents/skills/storybook-generator/`（跨工具通用目录） |

示例：

```bash
# 装进 Claude Code
git clone https://github.com/weaiw/storybook-generator-skill.git ~/.claude/skills/storybook-generator

# 装进 Codex
git clone https://github.com/weaiw/storybook-generator-skill.git ~/.codex/skills/storybook-generator

# 装进通用目录，供多个 agent 共享
git clone https://github.com/weaiw/storybook-generator-skill.git ~/.agents/skills/storybook-generator
```

安装后重启 agent 或开新会话即可生效。也可以只装到某个项目里（把目录放进项目的 `.claude/skills/`、`.codex/skills/` 等），随仓库分发给团队。

`agents/openai.yaml` 是 Codex 专属的界面元数据（显示名和默认提示语），其他工具可以忽略，不需要删除。

## 环境要求与降级策略

- 硬依赖只有文件读写。任何能读写 workspace 的 agent 都能跑完整的故事规划、提示词、排版和 QA 流程。
- 图像生成是可选能力：环境中有生图工具（`image_gen`、MCP 生图工具、CLI 生图命令等）时逐页直接出图；没有时自动切换“仅提示词模式”——产出全部逐页 prompt 和排版文件，你用任意图像模型（GPT-4o、即梦、Midjourney 等）出图后按命名规则放进 `images/`，再回到 agent 继续 QA 和排版。
- 排版交付默认是 `sample-book.html`（纯 HTML/CSS，任何环境可打开）；环境支持时才导出 PDF。

## 使用方式

安装后不需要特定语法，直接用自然语言描述需求，agent 会按 skill 描述自动匹配；支持斜杠命令的工具也可以显式调用 `/storybook-generator`：

```text
用 storybook-generator 把“一个怕黑的小朋友学会检查影子”的故事做成 12 页绘本。
```

英文同样有效：

```text
Use storybook-generator to turn my story idea into a 12-page picture-book MVP.
```

这个 skill 会默认先输出故事骨架、角色/风格圣经、逐页计划和提示词。需要出图时，再进入逐页图片生成和排版 QA。

## Repository Structure

```text
storybook-generator/
  SKILL.md            # 入口：工作流、环境适配、交付规则
  agents/
    openai.yaml       # Codex 专属界面元数据，其他工具可忽略
  references/
    story-structure.md
    character-continuity.md
    visual-styles.md
    prompt-workflow.md
    layout-and-pinyin.md
    reference-corpus-lessons.md
    story-text-structure-lessons.md
    commercial-publishing-workflow.md
    qa-checklist.md
```

## Design Philosophy

The core idea is simple: picture books are systems.

A good page needs a job. A good spread needs a visual contract. A good character needs repeated anchors. A good book needs rhythm, restraint, escalation, and a final emotional turn.

This skill teaches the agent to treat storybook creation as a repeatable creative pipeline: editorial structure first, generation second, QA always.

## License

MIT. Use it, fork it, adapt it, and build better storybook agents with it.
