# Domain-Specific Enhancement Rules

Load this file when the detected domain requires specialized treatment.
Apply the rules from the matching section ON TOP OF the core three-variant framework.

---

## Code & engineering

**Key additions to apply:**
- Specify the language and version (`Python 3.11`, `TypeScript 5`, `SQL/PostgreSQL 15`)
- Specify the framework if relevant (`FastAPI`, `React 18`, `dbt`)
- Include error-handling requirements ("include try/catch", "raise descriptive exceptions")
- Specify style constraints ("PEP8", "use type hints", "no external libraries")
- For debugging tasks: ask for root cause + fix + explanation, not just a fix
- For code review: specify what to look for (performance / security / readability)

**Option A sharpening** — add language + one-line output spec:
> "Write a Python 3.11 function that <task>. Return only the function, no explanation."

**Option B sharpening** — add constraints block:
> [CONSTRAINTS] Python 3.11, type hints required, no third-party libraries, PEP8 compliant.
> [FORMAT] Code block only. Include a docstring. No prose.

**Option C sharpening** — add IO schema:
> Input: `{{code_snippet: string}}`
> Output: `{fixed_code: string, explanation: string, issues_found: string[]}`

---

## Data analysis & SQL

**Key additions to apply:**
- Name the database / dialect explicitly (`PostgreSQL`, `BigQuery`, `SQLite`)
- Specify what "analyze" means: aggregations? anomalies? trends? correlations?
- Define the output: table, chart description, narrative, or JSON
- For SQL: specify whether CTEs are preferred, performance requirements, or index hints needed
- For data science: specify the library (`pandas`, `polars`, `dbt`) and Python version

**Option A sharpening:**
> "Write a PostgreSQL query that <task>. Return only the SQL, no explanation."

**Option C sharpening — add schema:**
> Input: `{{table_name}}`, schema: `{{columns}}`
> Output: SQL query as a string + a one-sentence plain-English summary of what it does.

---

## Creative writing

**Key additions to apply:**
- Specify genre, tone, and POV (`third-person limited`, `first-person`)
- Specify length precisely (`300 words`, `4 stanzas`, `3-act structure`)
- Specify audience (`adult literary fiction`, `middle-grade readers`)
- For editing tasks: specify what NOT to change (voice, key plot points)
- Avoid vague tone words ("engaging", "interesting") — use concrete anchors ("darkly funny like Roald Dahl", "spare like Hemingway")

**Option B sharpening:**
> [ROLE] You are a published <genre> author.
> [CONSTRAINTS] Tone: <anchor>. POV: <POV>. Length: <N> words. Do not alter: <protected elements>.

**Skip Option C** for most creative tasks — agent-ready format is rarely appropriate for purely creative output.

---

## Research & information retrieval

**Key additions to apply:**
- Specify the depth of answer needed (overview / deep-dive / comparison)
- Specify the audience's existing knowledge level
- Ask for sources to be cited inline or at the end (if relevant)
- For comparisons: ask for a structured table + narrative, not just prose
- Specify recency requirements ("focus on developments after 2022")

**Option A sharpening:**
> "Explain <topic> for a <audience> in <N> paragraphs. Focus on <aspect>."

**Option B sharpening:**
> [CONTEXT] Audience: <level>. They already know: <X>. They need to understand: <Y>.
> [FORMAT] Start with a 2-sentence summary. Then provide a structured breakdown with subheadings.

---

## Agent tasks & automation

This is where Option C becomes the primary variant. Reinforce these rules:

**Required elements:**
1. **Objective** — one sentence starting with "Your task is to…"
2. **Input spec** — named variables with types (`{{user_message: string}}`)
3. **Available tools** — list what the agent can call (if known)
4. **Steps** — numbered, imperative, unambiguous
5. **Output schema** — JSON or structured text with exact fields
6. **Error/fallback** — explicit instruction for invalid or missing input
7. **Stop condition** — when is the task complete?

**Option C template for agent tasks:**
```
Your task is to <objective>.

Inputs:
- {{input_1}}: <type and description>
- {{input_2}}: <type and description>

Steps:
1. <Step 1 — imperative verb + precise action>
2. <Step 2>
3. <Step 3>

Output (JSON):
{
  "result": <type>,
  "confidence": "high | medium | low",
  "reasoning": <string>
}

Edge cases:
- If {{input_1}} is empty: return {"error": "missing_input"}
- If task cannot be completed: return {"error": "insufficient_context", "reason": <string>}

Stop when: <condition>.
```

---

## System prompts (persona/instruction prompts)

When the user is writing a system prompt (not a user message), shift the framing:

- **Option A** — Tightest possible persona + capability spec
- **Option B** — Full system prompt with role, knowledge boundaries, tone, refusal rules
- **Option C** — System prompt as a state machine: define normal flow, edge cases, escalation paths

Always label these variants as "System prompt variants" in the output header.

**Anti-patterns specific to system prompts:**
- "Be helpful and harmless" (circular / already default behaviour)
- "Always answer in English" when the user may write in other languages — specify language policy precisely
- No persona anchor → model drifts in long conversations
- Contradictory rules ("be brief but thorough")
