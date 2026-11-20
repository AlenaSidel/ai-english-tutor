# English Tutor

[English](README.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (BR)](README.pt-BR.md) · **简体中文** · [日本語](README.ja.md) · [한국어](README.ko.md)

一个运行在 AI 智能体（[Claude Code](https://claude.com/claude-code)、Codex 等）里的私人英语老师。它会给你上课、把练习写进文件、批改答案、记录你的错误，并提醒你复习。

## 使用方式

1. 你给智能体发一条请求，例如“我想学几个面试常用句型”“批改：……”“复习一下之前的单词”。可以用你的母语写。
2. 智能体在你的文件夹 `users/<名字>/` 里，把课程写进 `lessons/`，把练习写进 `exercises/`。
3. 你直接在练习文件里作答，写在箭头 `→` 后面。
4. 发送批改指令（在个人资料里设定），智能体会把讲评写进同一个文件，并更新你的学习进度记录。

在这个文件夹里，智能体只教英语。遇到与英语无关的请求，它会委婉地建议你先休息一下、稍后再学，或者用英语聊聊同一个话题。规则见 [`AGENTS.md`](AGENTS.md) 中的 “English only” 一节。

有两种模式：**通用英语**（日常话题）和**职场英语**（求职面试、客户会议、用英语工作）。

## 项目结构

```
.
├── AGENTS.md                         # 适用于此文件夹中任何智能体的规则
├── README.*.md                       # 翻译版本：ru、es、pt-BR、zh-CN、ja、ko
├── CLAUDE.md                         # 为 Claude Code 加载 AGENTS.md 和 README.md
├── GEMINI.md                         # 为 Gemini CLI 做同样的事
├── .claude/skills/
│   ├── english-tutor/SKILL.md        # 老师的教学方法（不含个人数据）
│   └── setup/SKILL.md                # 首次运行：创建学员文件夹
├── .agents/skills                    # 指向 .claude/skills 的链接（供 Codex、Gemini CLI、Antigravity CLI 使用）
├── templates/                        # 个人文件、课程和练习的空白模板
├── prompts/
│   ├── en.md                         # 与老师配合效果好的请求示例
│   └── <语言代码>.md                 # 学员母语版本，由 setup 生成（不纳入 git）
├── LICENSE                           # MIT 许可证
└── users/                            # 学员的个人文件夹（不纳入 git）
    └── <名字>/
        ├── profile.md                # 个人资料：语言、水平、目标、职业
        ├── english-progress.md       # 学习进度记录：错误、主题、复习
        ├── useful-phrases.md         # 学习过程中遇到的实用表达
        ├── lessons/                  # 根据个人资料编写的理论，每节课一个文件
        └── exercises/                # 课程练习、答案和讲评
```

## 文件格式

所有文件的示例都在 `templates/` 里。模板是英文的；智能体在创建学员文件、课程和练习时，会用讲解语言书写各处标签（标题、表头、说明）。

**课程和练习。** 同一节课的课程文件和练习文件同名：`YYYY-MM-DD-<主题>.md`，例如 `users/<名字>/lessons/2026-11-18-interview-templates.md` 和 `users/<名字>/exercises/2026-11-18-interview-templates.md`。两者互相链接。

- 课程（`templates/lesson.md`）：理论部分，包括讲解、句型、例句和常见错误。课程依据你的个人资料编写，例句来自你的职业和经历。
- 练习（`templates/exercise.md`）：带课程链接的题目。每道题后面有一行 `→` 用于作答。末尾是讲评部分（标题在个人资料中设定），批改后填写。
- 不含新理论的复习：只有一个练习文件，主题为 `revision`。

**`english-progress.md`**：包含当前重点、词汇和表达、语法、反复出现的错误、待复习内容、已掌握内容等部分，也会链接到课程和练习。

**`useful-phrases.md`**：按主题分的表格，列出表达、翻译和使用场景。

**`prompts/en.md`**：请求示例，按部分分表，列出请求、作用和使用场景。仓库里只有英文版。设置时智能体会把它翻译成学员的语言，保存为 `prompts/<语言代码>.md`（ISO 639-1 代码，例如 `zh.md`），这些文件只保留在你的电脑上。你保存的请求会加到所有版本中。

## 快速开始

1. 克隆仓库。
2. 在项目根目录启动智能体（例如 `claude`）。
3. 输入首次运行指令（见[智能体列表](#支持的智能体)），或者直接说“我们开始吧”。智能体会问几个问题（语言、水平、目标、职业），然后根据模板创建你的文件夹 `users/<名字>/`。
4. 开始学习。请求示例见 [`prompts/en.md`](prompts/en.md)，设置完成后还有你母语版本的 `prompts/<语言代码>.md`。

如果还没有学员文件夹，智能体会在你第一次提出请求时建议先进行设置。

**不用智能体，手动设置：** 创建文件夹 `users/<名字>/`，把 `templates/profile.md` 复制进去，并把 `<!-- … -->` 注释替换成你的信息。其余文件智能体会在第一节课时创建。

```bash
mkdir -p users/anna && cp templates/profile.md users/anna/
```

### 支持的智能体

| 智能体 | 指令文件 | Skills | 首次运行 |
|---|---|---|---|
| [Claude Code](https://claude.com/claude-code) | `CLAUDE.md` → `AGENTS.md`、`README.md` | `.claude/skills/` | `/setup` |
| [Codex](https://developers.openai.com/codex) | `AGENTS.md` | `.agents/skills/`（链接） | `$setup` |
| [Cursor](https://cursor.com) | `AGENTS.md` | `.claude/skills/` 和 `.agents/skills/` | 在 Agent 聊天中输入 `/setup` |
| [OpenCode](https://opencode.ai) | `AGENTS.md` | `.claude/skills/` 和 `.agents/skills/` | `@setup` |
| [Antigravity CLI](https://antigravity.google/docs/getting-started)（`agy`） | `AGENTS.md` 和 `GEMINI.md` | `.agents/skills/`（链接） | `/setup` |
| [Gemini CLI](https://geminicli.com) | `GEMINI.md` → `AGENTS.md`、`README.md` | `.agents/skills/`（链接） | 无指令 |

已在 Claude Code 和 Codex 中测试；其他智能体根据其官方文档描述。

在任何智能体中，首次运行时都可以直接说“我们开始吧”。`.agents/skills/` 是指向 `.claude/skills/` 的链接，所以 skill 文件是同一份。如果某个智能体没有自动发现 skills，它会按照 `AGENTS.md` 中的路径打开它们。

- **Claude Code：** 不会自动读取 `AGENTS.md` 和 `.agents/skills/`，因此需要 `CLAUDE.md`，它通过 `@` 加载 `AGENTS.md` 和 `README.md`。Skills 会从 `.claude/skills/` 自动加载：智能体根据描述启用它们，每个 skill 也可以作为斜杠命令使用（`/setup`、`/english-tutor`）。
- **Cursor：** 请在 Agent 聊天中学习，其他模式下不保证规则和 skills 生效。Cursor 会把每个 skill 看到两次。如果因此出现问题（`/` 列表中出现重复的 skill，或启用了错误的 skill），请删除链接 `.agents/skills`：Codex、Gemini CLI 和 Antigravity CLI 会通过 `AGENTS.md` 中的路径找到 skills。不要删除 `.claude/skills/`，真正的 skill 文件在那里。
- **Gemini CLI：** 自 2026 年 6 月 18 日起，免费版和 Google AI Pro / Ultra 订阅用户改用 Antigravity CLI；使用付费 API 密钥或企业许可证仍可继续使用。加载 skill 前会请求许可。

## 个性化

你的所有个人数据都在 `users/<名字>/` 文件夹里，主要在 `profile.md`。想更改水平、目标或讲解语言，编辑个人资料或直接告诉智能体即可，无需修改 skills。

`users/` 文件夹已写入 `.gitignore`：你的课程、答案和进度不会进入仓库。

## 许可证

[MIT](LICENSE)
