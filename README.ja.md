# English Tutor

[English](README.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (BR)](README.pt-BR.md) · [简体中文](README.zh-CN.md) · **日本語** · [한국어](README.ko.md)

AI エージェント（[Claude Code](https://claude.com/claude-code)、Codex など）の中で動く、あなた専用の英語チューターです。レッスンを作り、練習問題をファイルで出し、答えを添削し、間違いを記録して、復習のタイミングを知らせます。

## しくみ

1. エージェントにリクエストを送ります。例：「面接で使えるフレーズを覚えたい」「添削して：…」「前に覚えた単語を復習しよう」。母語で書いてかまいません。
2. エージェントがあなたのフォルダ `users/<名前>/` の中に、レッスンを `lessons/`、練習問題を `exercises/` に作ります。
3. 答えは練習問題ファイルの矢印 `→` の後に直接書き込みます。
4. 添削コマンド（プロフィールで設定）を送ると、エージェントが同じファイルに解説を書き込み、学習記録を更新します。

このフォルダでは、エージェントは英語学習だけを担当します。関係のない依頼には、少し休んで後で学習に戻るか、同じ話題を英語で話してみるよう、やさしく提案します。ルールは [`AGENTS.md`](AGENTS.md) の「English only」セクションにあります。

モードは2つあります：**一般英語**（日常の話題）と**ビジネス英語**（就職面接、顧客との打ち合わせ、英語での仕事）。

## 構成

```
.
├── AGENTS.md                         # このフォルダで動くすべてのエージェント向けのルール
├── README.*.md                       # 翻訳版：ru、es、pt-BR、zh-CN、ja、ko
├── CLAUDE.md                         # Claude Code 用に AGENTS.md と README.md を読み込む
├── GEMINI.md                         # Gemini CLI 用に同じことをする
├── .claude/skills/
│   ├── english-tutor/SKILL.md        # チューターの教え方（個人データなし）
│   └── setup/SKILL.md                # 初回起動：学習者フォルダを作成
├── .agents/skills                    # .claude/skills へのリンク（Codex、Gemini CLI、Antigravity CLI 用）
├── templates/                        # 個人ファイル、レッスン、練習問題の空のテンプレート
├── prompts/
│   ├── en.md                         # チューターとうまく使えるリクエスト例
│   └── <言語コード>.md               # 学習者の言語版、setup が作成（git 管理外）
├── LICENSE                           # MIT ライセンス
└── users/                            # 学習者ごとの個人フォルダ（git 管理外）
    └── <名前>/
        ├── profile.md                # プロフィール：言語、レベル、目標、職業
        ├── english-progress.md       # 学習記録：間違い、トピック、復習
        ├── useful-phrases.md         # 学習中に出てきた便利なフレーズ
        ├── lessons/                  # プロフィールに合わせた解説、1レッスン1ファイル
        └── exercises/                # レッスンの練習問題、答え、添削
```

## ファイル形式

すべてのファイルの見本は `templates/` にあります。テンプレートは英語ですが、エージェントは学習者のファイル、レッスン、練習問題を作るとき、見出しや表の項目名、指示文などを解説言語で書きます。

**レッスンと練習問題。** レッスンとその練習問題は同じファイル名です：`YYYY-MM-DD-<トピック>.md`。例：`users/<名前>/lessons/2026-11-18-interview-templates.md` と `users/<名前>/exercises/2026-11-18-interview-templates.md`。互いにリンクしています。

- レッスン（`templates/lesson.md`）：解説、パターン、例文、よくある間違い。レッスンはプロフィールに基づき、あなたの職業や経験に合った例文で作られます。
- 練習問題（`templates/exercise.md`）：レッスンへのリンク付きの問題。各問題の後に答えを書く `→` の行があります。最後に添削セクション（見出しはプロフィールで設定）があり、添削後に埋まります。
- 新しい解説なしの復習：トピック名 `revision` の練習問題ファイルだけを作ります。

**`english-progress.md`**：現在の優先事項、単語とフレーズ、文法、よくある間違い、復習すること、できるようになったこと、の各セクション。レッスンと練習問題へのリンクもここにあります。

**`useful-phrases.md`**：トピック別の表で、フレーズ、訳、使う場面をまとめます。

**`prompts/en.md`**：リクエスト例。セクションごとの表で、リクエスト、何が起きるか、使う場面をまとめています。リポジトリには英語版だけが入っています。セットアップ時にエージェントが学習者の言語に翻訳し、`prompts/<言語コード>.md`（ISO 639-1 コード、例：`ja.md`）として保存します。このファイルはあなたのパソコンにだけ残ります。保存したリクエストはすべての言語版に追加されます。

## はじめかた

1. リポジトリをクローンします。
2. プロジェクトのルートでエージェントを起動します（例：`claude`）。
3. 初回起動コマンド（[エージェント一覧](#対応エージェント)を参照）を入力するか、「始めましょう」とだけ書きます。エージェントがいくつか質問（言語、レベル、目標、職業）をして、テンプレートからあなたのフォルダ `users/<名前>/` を作ります。
4. 学習を始めましょう。リクエスト例は [`prompts/en.md`](prompts/en.md) に、セットアップ後はあなたの言語の `prompts/<言語コード>.md` にもあります。

学習者フォルダがない場合、最初のリクエストでエージェントがセットアップを提案します。

**エージェントを使わず手動で：** フォルダ `users/<名前>/` を作り、`templates/profile.md` をコピーして、`<!-- … -->` のコメントの代わりに項目を記入します。ほかのファイルは最初のレッスンでエージェントが作ります。

```bash
mkdir -p users/anna && cp templates/profile.md users/anna/
```

### 対応エージェント

| エージェント | 指示ファイル | Skills | 初回起動 |
|---|---|---|---|
| [Claude Code](https://claude.com/claude-code) | `CLAUDE.md` → `AGENTS.md`、`README.md` | `.claude/skills/` | `/setup` |
| [Codex](https://developers.openai.com/codex) | `AGENTS.md` | `.agents/skills/`（リンク） | `$setup` |
| [Cursor](https://cursor.com) | `AGENTS.md` | `.claude/skills/` と `.agents/skills/` | Agent チャットで `/setup` |
| [OpenCode](https://opencode.ai) | `AGENTS.md` | `.claude/skills/` と `.agents/skills/` | `@setup` |
| [Antigravity CLI](https://antigravity.google/docs/getting-started)（`agy`） | `AGENTS.md` と `GEMINI.md` | `.agents/skills/`（リンク） | `/setup` |
| [Gemini CLI](https://geminicli.com) | `GEMINI.md` → `AGENTS.md`、`README.md` | `.agents/skills/`（リンク） | コマンドなし |

Claude Code と Codex で動作確認済みです。その他のエージェントは公式ドキュメントに基づいて記載しています。

どのエージェントでも、初回起動は「始めましょう」と書くだけで大丈夫です。`.agents/skills/` は `.claude/skills/` へのリンクなので、skill のファイルは同じものです。エージェントが skills を自動で見つけられない場合は、`AGENTS.md` に書かれたパスから開きます。

- **Claude Code：** `AGENTS.md` と `.agents/skills/` を自分では読まないため、`CLAUDE.md` が必要です。`CLAUDE.md` は `@` で `AGENTS.md` と `README.md` を読み込みます。Skills は `.claude/skills/` から自動で読み込まれ、説明文に応じて使われます。各 skill はスラッシュコマンドとしても使えます（`/setup`、`/english-tutor`）。
- **Cursor：** Agent チャットで学習してください。ほかのモードではルールや skills が効くとは限りません。Cursor は各 skill を2回認識します。これが原因で問題が起きたら（`/` の一覧に skill が重複して出る、違う skill が動く）、リンク `.agents/skills` を削除してください。Codex、Gemini CLI、Antigravity CLI は `AGENTS.md` のパスから skills を見つけます。`.claude/skills/` は削除しないでください。skill の実ファイルはそこにあります。
- **Gemini CLI：** 2026年6月18日から、無料プランと Google AI Pro / Ultra のサブスクリプションでは Antigravity CLI に置き換えられました。有料の API キーやエンタープライズライセンスでは引き続き使えます。skill を読み込む前に許可を求めます。

## カスタマイズ

個人データはすべて `users/<名前>/` フォルダ、主に `profile.md` にあります。レベル、目標、解説言語を変えたいときは、プロフィールを編集するか、エージェントに頼んでください。skills を変更する必要はありません。

`users/` フォルダは `.gitignore` に入っているので、レッスン、答え、学習記録がリポジトリに入ることはありません。

## ライセンス

[MIT](LICENSE)
