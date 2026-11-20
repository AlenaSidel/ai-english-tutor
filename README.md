# English Tutor

**English** · [Русский](README.ru.md)

A personal English tutor built on an AI coding agent ([Claude Code](https://claude.com/claude-code), Codex and others). The agent gives lessons, sets exercises in files, checks your answers, tracks your mistakes and reminds you to revise.

## How it works

1. You send the agent a request: "I want to learn some phrases for job interviews", "check: …", "let's revise old words". You can write in your native language.
2. The agent creates a lesson in `lessons/` and exercises in `exercises/` inside your folder `users/<name>/`.
3. You type your answers right into the exercise file, after the `→` arrows.
4. You send the check command (set in your profile) — the agent adds a review to the same file and updates your progress log.

In this folder the agent works only on English. For off-topic requests it gently suggests taking a break and coming back to studying later, or discussing the same topic in English. The rules are in the "English only" section of [`AGENTS.md`](AGENTS.md).

There are two modes: **general English** (everyday topics) and **professional English** (job interviews, client meetings, working in English).

## Structure

```
.
├── AGENTS.md                         # rules for any agent in this folder
├── README.ru.md                      # this description in Russian
├── CLAUDE.md                         # loads AGENTS.md and README.md for Claude Code
├── GEMINI.md                         # the same for Gemini CLI
├── .claude/skills/
│   ├── english-tutor/SKILL.md        # the tutor's method (no personal data)
│   └── setup/SKILL.md                # first run: creating the learner's folder
├── .agents/skills                    # link to .claude/skills (for Codex, Gemini CLI, Antigravity CLI)
├── templates/                        # blank templates for personal files, lessons and exercises
├── prompts/
│   ├── en.md                         # requests that work well with the tutor
│   └── <code>.md                     # the same in the learner's language, created by setup (not tracked by git)
├── LICENSE                           # MIT license
└── users/                            # learners' personal folders (not tracked by git)
    └── <name>/
        ├── profile.md                # profile: language, level, goals, profession
        ├── english-progress.md       # progress log: mistakes, topics, revision
        ├── useful-phrases.md         # useful phrases picked up along the way
        ├── lessons/                  # theory built from the profile, one file per lesson
        └── exercises/                # exercises for the lessons, answers and reviews
```

## File formats

Samples of all files are in `templates/`. The templates are in English; when creating a learner's files, lessons and exercises, the agent writes the labels (headings, table headers, instructions) in the language of explanations.

**Lessons and exercises.** A lesson file and its exercise file share the same name: `YYYY-MM-DD-<topic>.md`, for example `users/<name>/lessons/2026-11-18-interview-templates.md` and `users/<name>/exercises/2026-11-18-interview-templates.md`. They link to each other.

- Lesson (`templates/lesson.md`) — theory: explanation, patterns, examples, typical mistakes. Lessons are built from your profile, with examples from your profession and experience.
- Exercises (`templates/exercise.md`) — tasks with a link to the lesson. Each item is followed by a `→` line for your answer. At the end there is a review section (its heading is set in the profile), filled in after checking.
- Revision without new theory — only an exercise file with the topic `revision`.

**`english-progress.md`** — sections: current priorities, vocabulary and phrases, grammar, recurring errors, what to revise, what you already get right. It also links to lessons and exercises.

**`useful-phrases.md`** — tables by topic: phrase, translation, when to use it.

**`prompts/en.md`** — example requests; tables by section: prompt, what it does, when to use it. Only the English version is in the repository. During setup the agent translates it into the learner's language as `prompts/<code>.md` (ISO 639-1 code, e.g. `ru.md`); these files stay local. Saved prompts are added to every version.

## Getting started

1. Clone the repository.
2. Start the agent in the project root (for example, `claude`).
3. Type the first-run command (see the [agents table](#supported-agents)) or just "Let's start". The agent will ask a few questions — language, level, goals, profession — and create your folder `users/<name>/` from the templates.
4. Start studying. Example requests are in [`prompts/en.md`](prompts/en.md) and, after setup, in your language in `prompts/<code>.md`.

If there is no learner folder, the agent will offer setup on your first request.

**Manually, without the agent:** create a folder `users/<name>/`, copy `templates/profile.md` into it and fill in the fields in place of the `<!-- … -->` comments. The agent will create the other files during the first session.

```bash
mkdir -p users/anna && cp templates/profile.md users/anna/
```

### Supported agents

| Agent | Instructions | Skills | First run |
|---|---|---|---|
| [Claude Code](https://claude.com/claude-code) | `CLAUDE.md` → `AGENTS.md`, `README.md` | `.claude/skills/` | `/setup` |
| [Codex](https://developers.openai.com/codex) | `AGENTS.md` | `.agents/skills/` (link) | `$setup` |
| [Cursor](https://cursor.com) | `AGENTS.md` | `.claude/skills/` and `.agents/skills/` | `/setup` in Agent chat |
| [OpenCode](https://opencode.ai) | `AGENTS.md` | `.claude/skills/` and `.agents/skills/` | `@setup` |
| [Antigravity CLI](https://antigravity.google/docs/getting-started) (`agy`) | `AGENTS.md` and `GEMINI.md` | `.agents/skills/` (link) | `/setup` |
| [Gemini CLI](https://geminicli.com) | `GEMINI.md` → `AGENTS.md`, `README.md` | `.agents/skills/` (link) | no command |

Tested with Claude Code and Codex; the other agents are described from their documentation.

In every agent you can simply write "Let's start" for the first run. `.agents/skills/` is a link to `.claude/skills/`, so the skill files are the same. If an agent does not discover the skills, it opens them via the paths in `AGENTS.md`.

- **Claude Code:** does not read `AGENTS.md` or `.agents/skills/` on its own, hence `CLAUDE.md`: it loads `AGENTS.md` and `README.md` via `@`. Skills are picked up from `.claude/skills/` automatically: the agent activates them by description, and each skill is also available as a slash command (`/setup`, `/english-tutor`).
- **Cursor:** study in Agent chat — in other modes rules and skills are not guaranteed. Cursor sees each skill twice. If this causes problems (skills appear twice in the `/` list, the wrong one fires), delete the `.agents/skills` link: Codex, Gemini CLI and Antigravity CLI will find the skills via the paths in `AGENTS.md`. Do not delete `.claude/skills/`: it holds the actual skill files.
- **Gemini CLI:** since 18 June 2026 replaced by Antigravity CLI for the free tier and Google AI Pro / Ultra subscriptions; still works with a paid API key or an enterprise license. Asks for permission before loading a skill.

## Personalisation

All personal data is in your folder `users/<name>/`, mainly in `profile.md`. To change your level, goals or the language of explanations, edit the profile or ask the agent. You do not need to change the skills.

The `users/` folder is listed in `.gitignore`: your lessons, answers and progress will not end up in the repository.

## License

[MIT](LICENSE)
