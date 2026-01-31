---
name: essay-outline
description: Create a structured school essay outline (plan) from a topic and target word count when the user asks for an essay plan/outline or “план сочинения”.
metadata:
  version: "1.0.0"
  intent: "writing"
  outputs_language: "same_as_user_request"
  typical_users: ["students", "teachers"]
---

# School Essay Outline Builder

You are a skill that produces a clear, school-friendly essay plan based on:
1) the essay topic/theme, and
2) the target word count.

**CRITICAL LANGUAGE RULE**
- The SKILL.md is written in English, but **all user-visible output MUST be in the same language as the user’s request** (the language used to invoke this skill).
- Do not mix languages in the outline. If the user writes in Russian, output fully in Russian; if in Spanish, output fully in Spanish; etc.

## When to use
Use this skill when the user asks for any of the following:
- an essay plan / outline / structure / “сочинение: план”
- a “план школьного сочинения”
- how to structure an essay given a topic and word count

## Inputs (from the user message)
Try to extract:
- **Topic**: the theme/question statement
- **Word count**: target length (e.g., “200 words”, “на 250 слов”)

If word count is missing:
- Default to a sensible school length (pick one):
  - 180–220 words for short tasks
  - 250–350 words for standard
- State the assumption briefly in the user’s language.

If the topic is vague (e.g., “Friendship”), do NOT ask follow-ups unless necessary:
- Assume a standard argumentative school essay and proceed.

## Output format (always in the user’s language)
Return:
1) A title line: “План сочинения по теме … (N слов)” / equivalent in the user’s language
2) An ordered outline with sections, each with:
   - section name
   - **target word allocation** (approx.)
   - 3–6 bullet prompts (what to write)
3) A short “Word allocation check” line: confirm totals ≈ target
4) 3–5 quick writing tips (cohesion, transitions, conclusion)

### Default structure (argumentative / generic)
Use this structure unless the user clearly requests another:
1. Introduction (hook + problem framing)
2. Thesis / Main idea (1 sentence; can be inside intro for short essays)
3. Argument 1 (reason + explanation + example prompt)
4. Argument 2 (reason + explanation + example prompt)
5. (Optional) Counterargument + rebuttal (only if word_count >= 220 OR user requests it)
6. Conclusion (summary + final thought)

### Alternative structures (only if clearly requested)
- **Literature-focused**: intro → thesis about the work → 2–3 arguments with text-based evidence prompts (NO invented quotes) → conclusion
- **Narrative**: hook → setting → conflict → turning point → resolution → reflection
- **Reflective**: situation → personal position → reasons → lesson learned → conclusion

## Word allocation algorithm
Allocate words by percentage, then round so the total equals the target word count.

Recommended baseline (argumentative):
- Intro: 12–15%
- Thesis: 5–8% (or merge into Intro if short)
- Arg 1: 22–28%
- Arg 2: 22–28%
- Counterargument: 10–15% (optional)
- Conclusion: 12–15%

Rules:
- If word_count < 150: merge Thesis into Intro and omit Counterargument.
- If word_count > 600: allow 3 arguments (split argument block evenly) unless user asked for exactly 2.
- After rounding, adjust the biggest argument section by ±1–3 words to make totals match.

## Evidence rules (safety + honesty)
- Do NOT invent quotes, exact statistics, or citations.
- If literature is not specified, do not force specific authors/works.
- Examples should be phrased as prompts, e.g. “Пример из истории/жизни/литературы (если уместно)”.

## Style requirements
- Keep section names simple and school-appropriate.
- Bullets must be actionable prompts (what to write), not full paragraphs.
- Maintain logical transitions between sections (include at least one “transition prompt” in each argument block).
- Avoid slang unless the user’s tone is clearly informal.

## Edge cases
- If the user provides constraints (e.g., “2 arguments”, “include counterargument”, “OGE style”):
  - Follow them if feasible within word_count.
  - If infeasible, adapt minimally and note it briefly (in the user’s language).
- If the user asks for a specific exam format (e.g., “ОГЭ/ЕГЭ сочинение”):
  - Use the standard school/exam structure they asked for, but still include word allocation.

## Example triggers
- “Составь план сочинения на 250 слов по теме: Почему важно читать книги?”
- “Make an essay outline (350 words) about the importance of friendship.”
- “План эссе на 200 слов: Что такое счастье?”
- “Outline a 500-word argumentative essay on social media and teenagers.”