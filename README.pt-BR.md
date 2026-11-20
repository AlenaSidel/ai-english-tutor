# English Tutor

[English](README.md) · [Русский](README.ru.md) · [Español](README.es.md) · **Português (BR)** · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [한국어](README.ko.md)

Um tutor pessoal de inglês que funciona dentro de um agente de IA ([Claude Code](https://claude.com/claude-code), Codex e outros). O agente dá aulas, passa exercícios em arquivos, corrige suas respostas, acompanha seus erros e lembra você de revisar.

## Como funciona

1. Você escreve para o agente: "quero aprender frases para entrevistas de emprego", "corrija: …", "vamos revisar as palavras antigas". Pode escrever no seu idioma.
2. O agente cria uma aula em `lessons/` e exercícios em `exercises/` dentro da sua pasta `users/<nome>/`.
3. Você escreve as respostas direto no arquivo de exercícios, depois das setas `→`.
4. Você envia o comando de correção (definido no seu perfil): o agente adiciona a correção no mesmo arquivo e atualiza seu registro de progresso.

Nesta pasta o agente cuida só do inglês. Se você pedir outra coisa, ele sugere com gentileza fazer uma pausa e voltar a estudar mais tarde, ou conversar sobre o mesmo assunto em inglês. As regras estão na seção "English only" do [`AGENTS.md`](AGENTS.md).

Há dois modos: **inglês geral** (assuntos do dia a dia) e **inglês profissional** (entrevistas de emprego, reuniões com clientes, trabalhar em inglês).

## Estrutura

```
.
├── AGENTS.md                         # regras para qualquer agente nesta pasta
├── README.*.md                       # traduções: ru, es, pt-BR, zh-CN, ja, ko
├── CLAUDE.md                         # carrega AGENTS.md e README.md para o Claude Code
├── GEMINI.md                         # o mesmo para o Gemini CLI
├── .claude/skills/
│   ├── english-tutor/SKILL.md        # o método do tutor (sem dados pessoais)
│   └── setup/SKILL.md                # primeira execução: cria a pasta do aluno
├── .agents/skills                    # link para .claude/skills (para Codex, Gemini CLI, Antigravity CLI)
├── templates/                        # modelos vazios para arquivos pessoais, aulas e exercícios
├── prompts/
│   ├── en.md                         # pedidos que funcionam bem com o tutor
│   └── <código>.md                   # o mesmo no idioma do aluno, criado pelo setup (fora do git)
├── LICENSE                           # licença MIT
└── users/                            # pastas pessoais dos alunos (fora do git)
    └── <nome>/
        ├── profile.md                # perfil: idioma, nível, objetivos, profissão
        ├── english-progress.md       # registro de progresso: erros, temas, revisão
        ├── useful-phrases.md         # frases úteis que aparecem pelo caminho
        ├── lessons/                  # teoria baseada no perfil, um arquivo por aula
        └── exercises/                # exercícios das aulas, respostas e correções
```

## Formato dos arquivos

Há exemplos de todos os arquivos em `templates/`. Os modelos estão em inglês; ao criar os arquivos do aluno, as aulas e os exercícios, o agente escreve os rótulos (títulos, cabeçalhos de tabelas, instruções) no idioma das explicações.

**Aulas e exercícios.** Uma aula e seus exercícios têm o mesmo nome de arquivo: `YYYY-MM-DD-<tema>.md`, por exemplo `users/<nome>/lessons/2026-11-18-interview-templates.md` e `users/<nome>/exercises/2026-11-18-interview-templates.md`. Um aponta para o outro.

- Aula (`templates/lesson.md`): teoria com explicação, padrões, exemplos e erros típicos. As aulas se baseiam no seu perfil, com exemplos da sua profissão e experiência.
- Exercícios (`templates/exercise.md`): tarefas com link para a aula. Depois de cada item há uma linha `→` para sua resposta. No final há uma seção de correção (o título está no perfil), preenchida na correção.
- Revisão sem teoria nova: só um arquivo de exercícios com o tema `revision`.

**`english-progress.md`**: seções de prioridades atuais, vocabulário e frases, gramática, erros recorrentes, o que revisar e o que você já acerta. Também traz links para as aulas e os exercícios.

**`useful-phrases.md`**: tabelas por tema com a frase, a tradução e quando usar.

**`prompts/en.md`**: exemplos de pedidos em tabelas por seção, com o pedido, o que ele faz e quando usar. No repositório fica só a versão em inglês. Na configuração o agente a traduz para o idioma do aluno como `prompts/<código>.md` (código ISO 639-1, por exemplo `pt.md`); esses arquivos ficam no seu computador. Os pedidos que você salvar são adicionados a todas as versões.

## Primeiros passos

1. Clone o repositório.
2. Inicie o agente na raiz do projeto (por exemplo, `claude`).
3. Digite o comando de primeira execução (veja a [tabela de agentes](#agentes-compatíveis)) ou simplesmente "Vamos começar". O agente fará algumas perguntas (idioma, nível, objetivos, profissão) e criará sua pasta `users/<nome>/` a partir dos modelos.
4. Comece a estudar. Há exemplos de pedidos em [`prompts/en.md`](prompts/en.md) e, depois da configuração, no seu idioma em `prompts/<código>.md`.

Se não houver pasta de aluno, o agente vai propor a configuração no seu primeiro pedido.

**Manualmente, sem o agente:** crie uma pasta `users/<nome>/`, copie `templates/profile.md` para ela e preencha os campos no lugar dos comentários `<!-- … -->`. O agente cria os outros arquivos na primeira sessão.

```bash
mkdir -p users/ana && cp templates/profile.md users/ana/
```

### Agentes compatíveis

| Agente | Instruções | Skills | Primeira execução |
|---|---|---|---|
| [Claude Code](https://claude.com/claude-code) | `CLAUDE.md` → `AGENTS.md`, `README.md` | `.claude/skills/` | `/setup` |
| [Codex](https://developers.openai.com/codex) | `AGENTS.md` | `.agents/skills/` (link) | `$setup` |
| [Cursor](https://cursor.com) | `AGENTS.md` | `.claude/skills/` e `.agents/skills/` | `/setup` no chat Agent |
| [OpenCode](https://opencode.ai) | `AGENTS.md` | `.claude/skills/` e `.agents/skills/` | `@setup` |
| [Antigravity CLI](https://antigravity.google/docs/getting-started) (`agy`) | `AGENTS.md` e `GEMINI.md` | `.agents/skills/` (link) | `/setup` |
| [Gemini CLI](https://geminicli.com) | `GEMINI.md` → `AGENTS.md`, `README.md` | `.agents/skills/` (link) | sem comando |

Testado com Claude Code e Codex; os demais agentes estão descritos com base na documentação deles.

Em qualquer agente você pode simplesmente escrever "Vamos começar" na primeira execução. `.agents/skills/` é um link para `.claude/skills/`, então os arquivos dos skills são os mesmos. Se um agente não encontrar os skills sozinho, ele os abre pelos caminhos indicados no `AGENTS.md`.

- **Claude Code:** não lê `AGENTS.md` nem `.agents/skills/` por conta própria, por isso existe o `CLAUDE.md`, que carrega `AGENTS.md` e `README.md` com `@`. Os skills são carregados automaticamente de `.claude/skills/`: o agente os ativa pela descrição, e cada skill também funciona como comando com barra (`/setup`, `/english-tutor`).
- **Cursor:** estude no chat Agent; em outros modos as regras e os skills não são garantidos. O Cursor vê cada skill duas vezes. Se isso causar problemas (skills duplicados na lista `/`, o skill errado sendo ativado), apague o link `.agents/skills`: Codex, Gemini CLI e Antigravity CLI vão encontrar os skills pelos caminhos do `AGENTS.md`. Não apague `.claude/skills/`: é lá que ficam os arquivos reais dos skills.
- **Gemini CLI:** desde 18 de junho de 2026 foi substituído pelo Antigravity CLI no plano gratuito e nas assinaturas Google AI Pro / Ultra; continua funcionando com uma chave de API paga ou licença empresarial. Pede permissão antes de carregar um skill.

## Personalização

Todos os seus dados pessoais ficam na sua pasta `users/<nome>/`, principalmente em `profile.md`. Para mudar o nível, os objetivos ou o idioma das explicações, edite o perfil ou peça ao agente. Não é preciso mexer nos skills.

A pasta `users/` está no `.gitignore`: suas aulas, respostas e progresso não vão parar no repositório.

## Licença

[MIT](LICENSE)
