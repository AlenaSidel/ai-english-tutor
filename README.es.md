# English Tutor

[English](README.md) · [Русский](README.ru.md) · **Español** · [Português (BR)](README.pt-BR.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [한국어](README.ko.md)

Un tutor personal de inglés que funciona dentro de un agente de IA ([Claude Code](https://claude.com/claude-code), Codex y otros). El agente prepara lecciones, pone ejercicios en archivos, corrige tus respuestas, lleva la cuenta de tus errores y te recuerda repasar.

## Cómo funciona

1. Le escribes al agente: «quiero aprender frases para entrevistas de trabajo», «revisa: …», «repasemos las palabras antiguas». Puedes escribir en tu idioma.
2. El agente crea una lección en `lessons/` y ejercicios en `exercises/` dentro de tu carpeta `users/<nombre>/`.
3. Escribes las respuestas directamente en el archivo de ejercicios, después de las flechas `→`.
4. Envías el comando de corrección (definido en tu perfil): el agente añade la corrección al mismo archivo y actualiza tu registro de progreso.

En esta carpeta el agente solo se ocupa del inglés. Si le pides otra cosa, te sugiere con amabilidad tomar un descanso y volver a estudiar más tarde, o hablar del mismo tema en inglés. Las reglas están en la sección "English only" de [`AGENTS.md`](AGENTS.md).

Hay dos modos: **inglés general** (temas cotidianos) e **inglés profesional** (entrevistas de trabajo, reuniones con clientes, trabajar en inglés).

## Estructura

```
.
├── AGENTS.md                         # reglas para cualquier agente en esta carpeta
├── README.*.md                       # traducciones: ru, es, pt-BR, zh-CN, ja, ko
├── CLAUDE.md                         # carga AGENTS.md y README.md para Claude Code
├── GEMINI.md                         # lo mismo para Gemini CLI
├── .claude/skills/
│   ├── english-tutor/SKILL.md        # el método del tutor (sin datos personales)
│   └── setup/SKILL.md                # primer inicio: crea la carpeta del estudiante
├── .agents/skills                    # enlace a .claude/skills (para Codex, Gemini CLI, Antigravity CLI)
├── templates/                        # plantillas vacías para archivos personales, lecciones y ejercicios
├── prompts/
│   ├── en.md                         # peticiones que funcionan bien con el tutor
│   └── <código>.md                   # lo mismo en el idioma del estudiante, lo crea setup (no está en git)
├── LICENSE                           # licencia MIT
└── users/                            # carpetas personales de los estudiantes (no están en git)
    └── <nombre>/
        ├── profile.md                # perfil: idioma, nivel, objetivos, profesión
        ├── english-progress.md       # registro de progreso: errores, temas, repaso
        ├── useful-phrases.md         # frases útiles que van apareciendo
        ├── lessons/                  # teoría según el perfil, un archivo por lección
        └── exercises/                # ejercicios de las lecciones, respuestas y correcciones
```

## Formato de los archivos

Hay ejemplos de todos los archivos en `templates/`. Las plantillas están en inglés; al crear los archivos del estudiante, las lecciones y los ejercicios, el agente escribe los rótulos (títulos, encabezados de tablas, instrucciones) en el idioma de las explicaciones.

**Lecciones y ejercicios.** Una lección y sus ejercicios tienen el mismo nombre de archivo: `YYYY-MM-DD-<tema>.md`, por ejemplo `users/<nombre>/lessons/2026-11-18-interview-templates.md` y `users/<nombre>/exercises/2026-11-18-interview-templates.md`. Se enlazan entre sí.

- Lección (`templates/lesson.md`): teoría con explicación, patrones, ejemplos y errores típicos. Las lecciones se basan en tu perfil, con ejemplos de tu profesión y experiencia.
- Ejercicios (`templates/exercise.md`): tareas con un enlace a la lección. Después de cada punto hay una línea `→` para tu respuesta. Al final hay una sección de corrección (su título está en el perfil), que se completa al corregir.
- Repaso sin teoría nueva: solo un archivo de ejercicios con el tema `revision`.

**`english-progress.md`**: secciones de prioridades actuales, vocabulario y frases, gramática, errores recurrentes, qué repasar y qué ya te sale bien. También enlaza las lecciones y los ejercicios.

**`useful-phrases.md`**: tablas por tema con la frase, la traducción y cuándo usarla.

**`prompts/en.md`**: ejemplos de peticiones en tablas por sección, con la petición, qué hace y cuándo usarla. En el repositorio solo está la versión en inglés. Durante la configuración el agente la traduce al idioma del estudiante como `prompts/<código>.md` (código ISO 639-1, por ejemplo `es.md`); esos archivos se quedan en tu ordenador. Las peticiones que guardes se añaden a todas las versiones.

## Primeros pasos

1. Clona el repositorio.
2. Inicia el agente en la raíz del proyecto (por ejemplo, `claude`).
3. Escribe el comando de primer inicio (mira la [tabla de agentes](#agentes-compatibles)) o simplemente «Empecemos». El agente te hará unas preguntas (idioma, nivel, objetivos, profesión) y creará tu carpeta `users/<nombre>/` a partir de las plantillas.
4. Empieza a estudiar. Hay ejemplos de peticiones en [`prompts/en.md`](prompts/en.md) y, después de la configuración, en tu idioma en `prompts/<código>.md`.

Si no hay carpeta de estudiante, el agente te propondrá la configuración con tu primera petición.

**A mano, sin el agente:** crea una carpeta `users/<nombre>/`, copia en ella `templates/profile.md` y rellena los campos en lugar de los comentarios `<!-- … -->`. El agente creará los demás archivos en la primera sesión.

```bash
mkdir -p users/ana && cp templates/profile.md users/ana/
```

### Agentes compatibles

| Agente | Instrucciones | Skills | Primer inicio |
|---|---|---|---|
| [Claude Code](https://claude.com/claude-code) | `CLAUDE.md` → `AGENTS.md`, `README.md` | `.claude/skills/` | `/setup` |
| [Codex](https://developers.openai.com/codex) | `AGENTS.md` | `.agents/skills/` (enlace) | `$setup` |
| [Cursor](https://cursor.com) | `AGENTS.md` | `.claude/skills/` y `.agents/skills/` | `/setup` en el chat Agent |
| [OpenCode](https://opencode.ai) | `AGENTS.md` | `.claude/skills/` y `.agents/skills/` | `@setup` |
| [Antigravity CLI](https://antigravity.google/docs/getting-started) (`agy`) | `AGENTS.md` y `GEMINI.md` | `.agents/skills/` (enlace) | `/setup` |
| [Gemini CLI](https://geminicli.com) | `GEMINI.md` → `AGENTS.md`, `README.md` | `.agents/skills/` (enlace) | sin comando |

Probado con Claude Code y Codex; los demás agentes se describen a partir de su documentación.

En cualquier agente puedes escribir simplemente «Empecemos» para el primer inicio. `.agents/skills/` es un enlace a `.claude/skills/`, así que los archivos de los skills son los mismos. Si un agente no encuentra los skills por sí solo, los abre mediante las rutas de `AGENTS.md`.

- **Claude Code:** no lee `AGENTS.md` ni `.agents/skills/` por sí mismo, por eso existe `CLAUDE.md`, que carga `AGENTS.md` y `README.md` con `@`. Los skills se cargan automáticamente desde `.claude/skills/`: el agente los activa según su descripción, y cada skill también funciona como comando con barra (`/setup`, `/english-tutor`).
- **Cursor:** estudia en el chat Agent; en otros modos las reglas y los skills no están garantizados. Cursor ve cada skill dos veces. Si eso causa problemas (los skills aparecen duplicados en la lista `/` o se activa el que no es), borra el enlace `.agents/skills`: Codex, Gemini CLI y Antigravity CLI encontrarán los skills mediante las rutas de `AGENTS.md`. No borres `.claude/skills/`: ahí están los archivos reales de los skills.
- **Gemini CLI:** desde el 18 de junio de 2026 fue sustituido por Antigravity CLI en el plan gratuito y en las suscripciones Google AI Pro / Ultra; sigue funcionando con una clave de API de pago o una licencia empresarial. Pide permiso antes de cargar un skill.

## Personalización

Todos tus datos personales están en tu carpeta `users/<nombre>/`, sobre todo en `profile.md`. Para cambiar el nivel, los objetivos o el idioma de las explicaciones, edita el perfil o pídeselo al agente. No hace falta tocar los skills.

La carpeta `users/` está en `.gitignore`: tus lecciones, respuestas y progreso no acaban en el repositorio.

## Licencia

[MIT](LICENSE)
