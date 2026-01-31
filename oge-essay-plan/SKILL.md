---
name: oge-essay-plan
description: Create a structured OGE-style Russian school essay plan (сочинение ОГЭ) from a topic and target word count when the user asks for an OGE essay plan, “план сочинения ОГЭ”, or a plan in the OGE format.
metadata:
  version: "1.1.0"
  intent: "writing"
  outputs_language: "same_as_user_request"
  exam_format: "OGE"
  typical_users: ["students", "teachers"]
---

# OGE-Style Essay Plan Builder (Сочинение ОГЭ)

You are a skill that produces a clear, exam-friendly **OGE-style** essay plan based on:
1) the essay topic/problem statement, and
2) the target word count.

**CRITICAL LANGUAGE RULE**
- This SKILL.md is written in English, but **all user-visible output MUST be in the same language as the user’s request** (the language used to invoke this skill).
- Do not mix languages in the outline.

## When to use
Use this skill when the user asks for any of the following:
- “план сочинения ОГЭ”
- “сочинение в формате ОГЭ”
- “OGE essay outline/plan”
- “сделай структуру/план под ОГЭ”

If the user just asks for a generic essay plan (without OGE), prefer a generic school-outline skill instead.

## Inputs (from the user message)
Extract:
- **Topic / problem** (what the essay is about)
- **Word count** (target length, e.g., “200 слов”, “250 words”)
- Any constraints: number of arguments, required elements, style (“строго”, “кратко”), etc.
- If provided: specific works/authors the user must use.

If word count is missing:
- Assume a short OGE-style essay length appropriate for school practice (e.g., **180–220**).
- Briefly state the assumption in the user’s language.

If the topic is vague:
- Proceed with a standard OGE-style plan; do not ask follow-ups unless absolutely necessary.

## Output format (always in the user’s language)
Return:
1) A title line: “План сочинения (ОГЭ) по теме … (N слов)” / equivalent in the user’s language
2) An ordered plan with blocks, each containing:
   - block name
   - **target word allocation** (approx.)
   - 2–5 bullet prompts (what to write)
3) A section: **“Подбор произведений для аргументов”** (or equivalent in the user’s language)
4) A “Word allocation check” line confirming the total ≈ target
5) 3–5 short exam-oriented tips (e.g., logic, связки, тезисность)

## Required OGE-style blocks
Use these blocks by default (names must be in the user’s language):

1) **Вступление / постановка проблемы**
   - Introduce the topic and formulate the problem question clearly.

2) **Комментарий к проблеме**
   - Explain what the problem means and why it matters.
   - Provide 1–2 micro-examples as prompts (life/history/literature only if specified).
   - Add a logical link to the author’s/your position.

3) **Позиция автора (или формулировка основной мысли)**
   - State the implied author’s stance or the main idea (1–2 sentences).
   - If no text is provided, phrase as “можно считать, что авторская позиция такова: …” (without claiming quotes).

4) **Собственная позиция**
   - Agree/disagree/partly agree, clearly and briefly.

5) **Аргументация (1–2 аргумента)**
   - Each argument: claim → explanation → example prompt → mini-conclusion.
   - If word_count is large enough, allow 2 arguments; if short, keep 1 strong argument.

6) **Вывод**
   - Summarize and return to the problem; final thought.

### Optional blocks (only if requested / feasible)
- Counterargument + rebuttal (rare for strict OGE unless user asks)
- “Актуальность сегодня” (if user requests modern relevance)

## Word allocation algorithm
Allocate words by percentage, round, then adjust to match the target.

Suggested allocation (OGE-style):
- Intro/problem: 12–15%
- Comment: 22–28%
- Author position: 8–12%
- Own position: 8–12%
- Arguments: 25–32% total
- Conclusion: 12–15%

Rules:
- If word_count < 170: use **1 argument** and compress “author position” + “own position” into a single block (still label both ideas).
- If word_count >= 230: use **2 arguments** (split argument words roughly evenly).
- After rounding, add/subtract 1–3 words from the largest argument block to make totals match exactly.

## Literature module: selecting works for arguments
Add a dedicated section in the output: **“Подбор произведений для аргументов”** (in the user’s language).

### What to output in “Recommended works”
Provide **3–6** suitable works and make them usable in an exam-style argument.

For each work include:
- **Title + author** (as plain text)
- **Why it fits** (1 short sentence: which side of the topic/problem it supports)
- **Argument angle** (1 bullet: which idea/episode/character conflict can be used)
- **Safe evidence prompt** (no quotes): e.g., “упомянуть поступок героя / развязку конфликта / изменение героя”

Keep it concise. The goal is to give the student “hooks” to build arguments.

### Selection heuristics (topic → works)
1) Identify 2–4 key themes from the topic/problem:
   - e.g., friendship, mercy, choice, responsibility, honesty, courage, betrayal, patriotism, nature, family, selflessness, dream, freedom, conscience, education, etc.
2) Prefer **canonical school-level literature** (especially if call language is Russian):
   - pick works commonly studied in school; avoid obscure titles.
3) Cover variety:
   - at least one work for “argument FOR/positive model”
   - optionally one work for “argument AGAINST/negative example”
4) If the user provided required works/authors:
   - include them first, then add 2–4 compatible options.
5) If the topic is not clearly literary (e.g., ecology, technology):
   - still suggest works where the theme is present indirectly (nature/humanity/responsibility), but phrase as prompts.

### IMPORTANT evidence constraints
- Do NOT invent quotes, exact statistics, or precise citations.
- If the user did not provide the text excerpt, do not claim “the author says…” with invented lines.
- You may reference general plot facts and character actions as prompts, but keep them high-level.

### Fallback list (use only if the topic is too abstract)
If the topic is abstract and no strong matches appear, choose from broadly applicable works and adapt angles:
- А. С. Пушкин — «Капитанская дочка» (честь, выбор, ответственность)
- Н. В. Гоголь — «Тарас Бульба» (долг, товарищество, предательство)
- М. Ю. Лермонтов — «Герой нашего времени» (нравственный выбор, ответственность)
- И. С. Тургенев — «Отцы и дети» (ценности, конфликт поколений)
- Л. Н. Толстой — «Война и мир» (нравственный рост, долг, сострадание)
- М. Шолохов — «Судьба человека» (стойкость, человечность)

(Do not output this fallback list as-is; it’s a back-pocket set. Output only the selected 3–6 works that best match the topic.)

## Style requirements
- Bullets must be actionable prompts, not full paragraphs.
- Use clear linking phrases prompts (e.g., “Таким образом”, “Следовательно”, “Кроме того”).
- Avoid long, complex sentences in bullets.
- Maintain exam tone: concise, logical, structured.

## Handling constraints
- If user demands “2 arguments” but word_count is very small, still provide 2 arguments as mini-arguments, and note briefly that they will be short.
- If user asks for strict Russian OGE formatting, prioritize these blocks and keep wording formal.

## Example triggers
- “Составь план сочинения ОГЭ на 200 слов по теме: Что такое настоящая дружба?”
- “План в формате ОГЭ: Почему важно учиться признавать ошибки? 220 слов.”
- “OGE essay plan: The value of honesty, 250 words.” (Output must be in English if the user wrote in English.)