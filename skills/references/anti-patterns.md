# Prompt Anti-Patterns — Reference

A checklist of the most common issues found in user-written prompts,
with examples and the fix to apply in each enhanced variant.

---

## Category 1 — Vagueness

| Anti-pattern | Bad example | Fix |
|---|---|---|
| **Weak imperative** | "help me with my essay" | Replace with action verb: "Rewrite the introduction of this essay to…" |
| **Undefined pronoun** | "make it better" | Specify what "it" is and what "better" means |
| **Vague quantity** | "give me some ideas" | "Give me 5 ideas" |
| **Underspecified domain** | "write about climate change" | Specify angle, audience, format, length |
| **Missing success condition** | "analyze this data" | "Identify the top 3 trends and flag any anomalies" |

---

## Category 2 — Missing context

| Anti-pattern | Bad example | Fix |
|---|---|---|
| **No audience specified** | "explain machine learning" | "Explain machine learning to a non-technical marketing manager" |
| **No format specified** | "summarize this article" | Add: "Return a 3-bullet summary + one key quote" |
| **No length constraint** | "write a product description" | "Write a 100-word product description for an e-commerce listing" |
| **No tone specified** | "write an email" | "Write a professional but warm email" |
| **No prior context** | "continue this" | Always include what "this" is |

---

## Category 3 — Structural bloat

| Anti-pattern | Bad example | Fix |
|---|---|---|
| **Politeness padding** | "Could you please kindly help me if you don't mind…" | Strip to the imperative |
| **Redundant hedging** | "I know you're an AI but maybe if possible…" | Delete entirely |
| **Restating the obvious** | "As an AI language model, please…" | Delete entirely |
| **Over-qualification** | "This might be a stupid question but…" | Delete entirely |
| **Narrative preamble** | 3 sentences of backstory before the actual ask | Compress to 1 context sentence |
| **Duplicate instructions** | Same constraint stated twice in different words | Keep once, clearest version |

---

## Category 4 — Ambiguous scope

| Anti-pattern | Bad example | Fix |
|---|---|---|
| **Hidden multi-task** | "write a blog post and make sure to research it and add citations" | Separate into explicit numbered steps |
| **Conflicting constraints** | "be brief but cover everything in detail" | Choose one or specify what takes priority |
| **Undefined reference** | "use the data from before" | Re-include the data or reference it explicitly |
| **Open-ended iteration** | "keep improving until it's good" | Define "good": "Iterate until it scores above 8/10 on clarity" |

---

## Category 5 — Agent / pipeline specific

| Anti-pattern | Bad example | Fix |
|---|---|---|
| **No input schema** | "process the user data" | "Input: `{{user_object}}` with fields: name, email, plan" |
| **No output schema** | "extract the key info" | "Return JSON: `{name, email, issues: []}` " |
| **No stop condition** | "keep asking follow-up questions" | "Stop after 3 follow-ups or when all fields are filled" |
| **No error handling** | (missing entirely) | "If input is missing or malformed, return `{error: 'invalid_input'}`" |
| **Stateful confusion** | Assumes memory between calls | "Treat each call as stateless. Input carries full context." |

---

## Scoring guide

Count the number of anti-patterns present in the original prompt:

| Count | Clarity score | Description |
|---|---|---|
| 0–1 | 8–10 | Good prompt, minor polish only |
| 2–3 | 6–7 | Usable but noticeably improvable |
| 4–6 | 4–5 | Will likely cause off-target responses |
| 7+ | 1–3 | Needs significant reconstruction |
