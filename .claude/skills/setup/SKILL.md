---
name: setup
description: First-run setup of the English tutor project — creates the learner's personal folder in users/ from templates/ and fills in the profile. Use when the user runs /setup, starts the project for the first time, or when users/ has no learner folder.
---

# Setup

Create a personal folder for a new learner. Project structure and file formats are described in `README.md`.

Rules:
- Start with one or two sentences explaining what will happen: a few questions, then the agent creates the learner's folder.
- Talk to the learner in their language. If it is not clear from their message (for example, they only wrote "setup" or "/setup"), greet them in simple English and first ask which language they prefer; continue in that language. Do not guess the language from system settings, global instructions or other projects.
- Take the answers only from the learner in this conversation. Do not fill them in from open editor tabs, other folders, memory or earlier sessions, and do not propose ready-made answers based on such sources.
- Do not create or change any files until all questions in step 2 are answered.

## Steps

1. Check `users/`. If a learner folder already exists, tell the user and ask whether to create another learner or update the existing profile. Do not overwrite existing files.
2. Ask the learner, a few questions at a time, in their language:
   - name for the folder (Latin letters, lowercase, e.g. `anna`);
   - native language. If the learner has already chosen a language for the conversation or is clearly writing in one, assume it is their native language and do not ask again; just state it together with the other answers so they can correct it. The language of explanations is the native language by default: do not ask about it separately, just mention that it can be changed later in the profile;
   - current English level (offer a short level check if they do not know);
   - target level;
   - where they will use English — one question, phrased concretely, with examples and the reason for asking, e.g.: "Which country do you live in or plan to use English in? For example: you live in the UK, you are moving to the US, or you work online with clients from different countries. This helps me choose between British and American English." Derive the variety from the country (UK → British, US → American, etc.); if the learner names a variety instead, take the country from it (British → UK) or leave it unspecified. If there is no particular country ("everywhere", international work, travel), record "international" and suggest one variety (American is the most common online, British if they prefer) for them to confirm. Do not ask a follow-up about the other half; state your assumption with the other answers so they can correct it;
   - one open question instead of separate questions about work, goals and interests: "Tell me a bit about yourself — what you do, your hobbies, and what you want English for." From the answer, fill in the profession and domains for professional scenarios, goals and general interests. Ask a short follow-up only for what is really missing (for example, whether they need English for work);
   - the check command and the review heading: do not ask about them and do not show them in the summary. Choose them yourself: a short check command in the learner's language (e.g. «check», «проверь», «revisa») and a review heading in the language of explanations (e.g. `Review`, `Разбор`, `Revisión`).

   When all questions are answered, show a short summary of the answers and your assumptions (native language and language of explanations, variety and country) and ask the learner to confirm or correct it in one message.
3. Create `users/<name>/` from `templates/`. In every file, translate the labels — headings, captions, table headers, instructions such as "Write your answer after the arrow" — into the language of explanations. Keep the structure, the `→` answer lines and the order of sections; English examples stay in English.
   - `profile.md` — filled in with the answers; remove template comments;
   - `english-progress.md` and `useful-phrases.md` — with translated labels;
   - empty `lessons/` and `exercises/` folders.
4. Make sure there are example requests in the learner's language. Only `prompts/en.md` is in the repository; other language versions are local and ignored by git. Files are named by ISO 639-1 language code (`ru.md`, `es.md`…). If the learner's language is not English and `prompts/<code>.md` does not exist, create it by translating `prompts/en.md`, including the prompts themselves, so the learner can type them as written. Put a link `[English](en.md)` at the top. No personal data.
5. Show the created structure and suggest 2–3 first requests from the prompts file in the learner's language. Base them on the "Lessons and exercises" section and adapt them to the learner's goals and interests (e.g. "I want to learn phrases for a ski trip" instead of the interview example). The suggestions must show what the tutor does beyond a plain chat: a lesson with exercises in files, checking answers, revision. Do not suggest plain conversation or one-off translation requests. Explain in one sentence how checking works: type the answers after `→` in the exercise file, then write the check command (e.g. "When you've filled in your answers, write *revisa*"). It can be changed later in the profile.

Ask only what the template needs. Do not start a lesson until setup is finished.
