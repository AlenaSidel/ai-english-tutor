# English Tutor

[English](README.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (BR)](README.pt-BR.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · **한국어**

AI 에이전트([Claude Code](https://claude.com/claude-code), Codex 등) 안에서 동작하는 개인 영어 튜터입니다. 수업을 만들고, 연습 문제를 파일로 내고, 답을 첨삭하고, 실수를 기록하고, 복습할 때를 알려 줍니다.

## 사용 방법

1. 에이전트에게 요청을 보냅니다. 예: "면접에서 쓸 표현을 배우고 싶어요", "첨삭해 줘: …", "예전 단어 복습하자". 모국어로 써도 됩니다.
2. 에이전트가 내 폴더 `users/<이름>/` 안에 수업은 `lessons/`에, 연습 문제는 `exercises/`에 만듭니다.
3. 연습 문제 파일의 화살표 `→` 뒤에 바로 답을 씁니다.
4. 첨삭 명령어(프로필에서 설정)를 보내면 에이전트가 같은 파일에 해설을 쓰고 학습 기록을 업데이트합니다.

이 폴더에서 에이전트는 영어만 가르칩니다. 영어와 관계없는 요청을 받으면, 잠깐 쉬었다가 나중에 다시 공부하거나 같은 주제로 영어 대화를 해 보자고 부드럽게 제안합니다. 규칙은 [`AGENTS.md`](AGENTS.md)의 "English only" 섹션에 있습니다.

모드는 두 가지입니다: **일반 영어**(일상 주제)와 **비즈니스 영어**(취업 면접, 고객 미팅, 영어로 일하기).

## 구조

```
.
├── AGENTS.md                         # 이 폴더에서 동작하는 모든 에이전트용 규칙
├── README.*.md                       # 번역본: ru, es, pt-BR, zh-CN, ja, ko
├── CLAUDE.md                         # Claude Code용으로 AGENTS.md와 README.md를 불러옴
├── GEMINI.md                         # Gemini CLI용으로 같은 역할
├── .claude/skills/
│   ├── english-tutor/SKILL.md        # 튜터의 교수법(개인 데이터 없음)
│   └── setup/SKILL.md                # 첫 실행: 학습자 폴더 생성
├── .agents/skills                    # .claude/skills로 가는 링크(Codex, Gemini CLI, Antigravity CLI용)
├── templates/                        # 개인 파일, 수업, 연습 문제용 빈 템플릿
├── prompts/
│   ├── en.md                         # 튜터와 잘 맞는 요청 예시
│   └── <언어코드>.md                 # 학습자 언어 버전, setup이 생성(git에 포함되지 않음)
├── LICENSE                           # MIT 라이선스
└── users/                            # 학습자 개인 폴더(git에 포함되지 않음)
    └── <이름>/
        ├── profile.md                # 프로필: 언어, 수준, 목표, 직업
        ├── english-progress.md       # 학습 기록: 실수, 주제, 복습
        ├── useful-phrases.md         # 공부하면서 만난 유용한 표현
        ├── lessons/                  # 프로필에 맞춘 이론, 수업당 파일 하나
        └── exercises/                # 수업별 연습 문제, 답, 첨삭
```

## 파일 형식

모든 파일의 견본은 `templates/`에 있습니다. 템플릿은 영어로 되어 있지만, 에이전트는 학습자 파일, 수업, 연습 문제를 만들 때 제목, 표 머리글, 안내 문구 같은 표시를 설명 언어로 씁니다.

**수업과 연습 문제.** 수업 파일과 연습 문제 파일은 이름이 같습니다: `YYYY-MM-DD-<주제>.md`. 예: `users/<이름>/lessons/2026-11-18-interview-templates.md`와 `users/<이름>/exercises/2026-11-18-interview-templates.md`. 서로 링크되어 있습니다.

- 수업(`templates/lesson.md`): 설명, 패턴, 예문, 자주 하는 실수로 이루어진 이론. 수업은 프로필을 바탕으로, 내 직업과 경험에 맞는 예문으로 만들어집니다.
- 연습 문제(`templates/exercise.md`): 수업 링크가 달린 문제. 문제마다 답을 쓰는 `→` 줄이 있습니다. 끝에는 첨삭 섹션(제목은 프로필에서 설정)이 있고, 첨삭 후에 채워집니다.
- 새 이론 없는 복습: 주제가 `revision`인 연습 문제 파일만 만듭니다.

**`english-progress.md`**: 현재 우선순위, 단어와 표현, 문법, 반복되는 실수, 복습할 것, 이미 잘하는 것 섹션으로 구성됩니다. 수업과 연습 문제 링크도 여기에 있습니다.

**`useful-phrases.md`**: 주제별 표로 표현, 번역, 쓰는 상황을 정리합니다.

**`prompts/en.md`**: 요청 예시. 섹션별 표로 요청, 하는 일, 쓰는 상황을 정리했습니다. 저장소에는 영어 버전만 있습니다. 설정할 때 에이전트가 학습자 언어로 번역해 `prompts/<언어코드>.md`(ISO 639-1 코드, 예: `ko.md`)로 저장하며, 이 파일은 내 컴퓨터에만 남습니다. 저장한 요청은 모든 언어 버전에 추가됩니다.

## 시작하기

1. 저장소를 클론합니다.
2. 프로젝트 루트에서 에이전트를 실행합니다(예: `claude`).
3. 첫 실행 명령어([에이전트 표](#지원-에이전트) 참고)를 입력하거나 그냥 "시작하자"라고 씁니다. 에이전트가 몇 가지(언어, 수준, 목표, 직업)를 물어본 뒤 템플릿으로 내 폴더 `users/<이름>/`을 만듭니다.
4. 공부를 시작하세요. 요청 예시는 [`prompts/en.md`](prompts/en.md)에, 설정 후에는 내 언어로 된 `prompts/<언어코드>.md`에도 있습니다.

학습자 폴더가 없으면 첫 요청 때 에이전트가 설정을 제안합니다.

**에이전트 없이 직접:** `users/<이름>/` 폴더를 만들고 `templates/profile.md`를 복사한 다음, `<!-- … -->` 주석 자리에 내용을 채웁니다. 나머지 파일은 첫 수업에서 에이전트가 만듭니다.

```bash
mkdir -p users/anna && cp templates/profile.md users/anna/
```

### 지원 에이전트

| 에이전트 | 지침 파일 | Skills | 첫 실행 |
|---|---|---|---|
| [Claude Code](https://claude.com/claude-code) | `CLAUDE.md` → `AGENTS.md`, `README.md` | `.claude/skills/` | `/setup` |
| [Codex](https://developers.openai.com/codex) | `AGENTS.md` | `.agents/skills/`(링크) | `$setup` |
| [Cursor](https://cursor.com) | `AGENTS.md` | `.claude/skills/`와 `.agents/skills/` | Agent 채팅에서 `/setup` |
| [OpenCode](https://opencode.ai) | `AGENTS.md` | `.claude/skills/`와 `.agents/skills/` | `@setup` |
| [Antigravity CLI](https://antigravity.google/docs/getting-started)(`agy`) | `AGENTS.md`와 `GEMINI.md` | `.agents/skills/`(링크) | `/setup` |
| [Gemini CLI](https://geminicli.com) | `GEMINI.md` → `AGENTS.md`, `README.md` | `.agents/skills/`(링크) | 명령어 없음 |

Claude Code와 Codex에서 테스트했습니다. 나머지 에이전트는 공식 문서를 바탕으로 정리했습니다.

어느 에이전트에서든 첫 실행 때 "시작하자"라고만 써도 됩니다. `.agents/skills/`는 `.claude/skills/`로 가는 링크라서 skill 파일은 같습니다. 에이전트가 skills를 스스로 찾지 못하면 `AGENTS.md`에 적힌 경로로 엽니다.

- **Claude Code:** `AGENTS.md`와 `.agents/skills/`를 스스로 읽지 않기 때문에 `CLAUDE.md`가 필요합니다. `CLAUDE.md`는 `@`로 `AGENTS.md`와 `README.md`를 불러옵니다. Skills는 `.claude/skills/`에서 자동으로 불러오며, 에이전트가 설명을 보고 사용합니다. 각 skill은 슬래시 명령어로도 쓸 수 있습니다(`/setup`, `/english-tutor`).
- **Cursor:** Agent 채팅에서 공부하세요. 다른 모드에서는 규칙과 skills가 보장되지 않습니다. Cursor는 각 skill을 두 번 인식합니다. 이 때문에 문제가 생기면(`/` 목록에 skill이 중복으로 보이거나 엉뚱한 skill이 동작하면) `.agents/skills` 링크를 삭제하세요. Codex, Gemini CLI, Antigravity CLI는 `AGENTS.md`의 경로로 skills를 찾습니다. `.claude/skills/`는 삭제하지 마세요. 실제 skill 파일이 여기에 있습니다.
- **Gemini CLI:** 2026년 6월 18일부터 무료 요금제와 Google AI Pro / Ultra 구독에서는 Antigravity CLI로 대체되었습니다. 유료 API 키나 엔터프라이즈 라이선스로는 계속 쓸 수 있습니다. skill을 불러오기 전에 허락을 구합니다.

## 개인 설정

개인 데이터는 모두 `users/<이름>/` 폴더, 주로 `profile.md`에 있습니다. 수준, 목표, 설명 언어를 바꾸고 싶으면 프로필을 고치거나 에이전트에게 부탁하세요. skills는 고칠 필요가 없습니다.

`users/` 폴더는 `.gitignore`에 들어 있어서, 수업, 답, 학습 기록이 저장소에 들어가지 않습니다.

## 라이선스

[MIT](LICENSE)
