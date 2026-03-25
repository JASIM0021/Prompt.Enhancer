---
name: prompt-enhancer
description: >
  Analyze and rewrite any user-written prompt into 3 optimized variants for better AI responses.
  Trigger this skill whenever a user: says "enhance/improve/rewrite my prompt", pastes a rough prompt and asks for help, mentions reducing tokens or context, wants a prompt to work better with Claude or other AI agents, says "make this clearer for the AI", or shares a prompt that is visibly vague, rambling, or missing key context.
  Also trigger proactively if the user shares a long or unclear prompt before sending it somewhere — even if they haven't explicitly asked for enhancement.
---

# Prompt Enhancer

Transform rough, vague, or bloated user prompts into three sharply optimized variants —
each tuned for a different goal: speed, structure, or agentic pipelines.

---

## When this skill activates

- "enhance / improve / rewrite / fix my prompt"
- "make this better for Claude / GPT / agents"
- "reduce tokens / context usage"
- User pastes a messy prompt and asks for help
- Prompt is visibly vague, rambling, contradictory, or missing output instructions
- User is building an AI workflow and needs tight system or user prompts

---

## Core workflow (run every time)

### Step 1 — Analysis pass

Before writing anything, internally assess the raw prompt across these dimensions:

| Dimension | Questions to ask |
|---|---|
| **Domain** | Code · writing · data · research · creative · agent task · other |
| **Intent clarity** | Is the core ask unambiguous? |
| **Missing context** | Role? Audience? Format? Constraints? Success criteria? |
| **Anti-patterns** | See `references/anti-patterns.md` for the full checklist |
| **Verbosity** | Filler words, redundancy, hedging phrases |
| **Output spec** | Is the desired output format/length/structure specified? |

Compute an approximate token count for the original (rough heuristic: `word_count × 1.35`).

---

### Step 2 — Generate 3 variants

Produce all three in a single response. Never ask the user to choose before showing them.
Each variant must be self-contained and copy-pasteable.

#### Option A — Lean & Precise
**Goal**: Maximum compression without losing intent.
**Rules**:
- Strip filler ("please", "could you", "I was wondering if", "as an AI", "feel free to")
- Replace weak verbs (help me, look at, think about) with strong imperatives (write, analyze, compare, extract)
- Collapse multi-sentence setups into one context sentence
- Add a single output-format line at the end if missing ("Respond in bullet points." / "Return valid JSON." / "Max 3 paragraphs.")
- Target: 40–60% fewer tokens than original
- **Best for**: Quick one-off queries, simple factual tasks, cost-sensitive API calls

#### Option B — Structured Expert
**Goal**: Full clarity through explicit structure.
**Template to follow** (adapt, don't force):
```
[ROLE] You are a <domain expert / specific persona>.
[CONTEXT] <1–2 sentences of relevant background>.
[TASK] <Precise imperative verb + what to produce>.
[CONSTRAINTS] <Tone / length / audience / what to avoid>.
[FORMAT] <Exact output structure: markdown table / numbered list / JSON / prose paragraphs>.
```
**Rules**:
- Every section must add information not already implied
- Skip any section that would be filler (e.g. no [ROLE] if it's obvious)
- Constraints must be specific ("max 200 words" beats "be concise")
- **Best for**: Complex multi-step tasks, tasks requiring consistent tone, prompts that will be reused

#### Option C — Agent-Ready
**Goal**: Optimized for AI agents, automated pipelines, or multi-turn workflows.
**Rules**:
- Open with a clear **objective statement** (one sentence: "Your task is to…")
- Define **inputs** explicitly if the prompt will receive variable data (`{{variable_name}}` syntax)
- Specify **success criteria** or done-condition ("Stop when all fields are populated." / "Return DONE when complete.")
- Add **edge-case handling**: what to do if input is ambiguous, missing, or malformed
- Specify **output schema** precisely — JSON fields, array structure, or required headings
- Use numbered steps for multi-stage tasks
- **Best for**: Tool-calling agents, automated workflows, API chaining, tasks run without human supervision

---

### Step 3 — Output format

Render the response in this exact structure:

```
## Prompt analysis

**Domain detected**: <domain>
**Issues found**: <comma-separated list of anti-patterns detected>
**Original token estimate**: ~<N> tokens

---

## Option A — Lean & precise
> *Best for: quick tasks · ~X% token reduction · clarity score: N/10*

<enhanced prompt — plain text, copy-pasteable>

---

## Option B — Structured expert
> *Best for: complex or repeated tasks · clarity score: N/10*

<enhanced prompt — plain text, copy-pasteable>

---

## Option C — Agent-ready
> *Best for: pipelines, agents, automated workflows · clarity score: N/10*

<enhanced prompt — plain text, copy-pasteable>

---

## What changed
<2–4 bullets summarising the most impactful fixes applied across the variants>
```

**Clarity score** (1–10): Score each variant independently.
- 1–3: Still vague or incomplete
- 4–6: Understandable but improvable
- 7–8: Clear, specific, ready to use
- 9–10: Optimally precise, no ambiguity

---

### Step 4 — Follow-up offer

After presenting the three variants, always add one short line:

> *Want a custom blend of these? Tell me what to prioritise (brevity / structure / agent use) and I'll combine them.*

---

## Quality rules (apply to all variants)

1. **Never hallucinate context** — if you don't know the user's stack / audience / use-case, either ask once OR add a `[CONTEXT: replace with your X]` placeholder
2. **Preserve full intent** — a shorter prompt that loses a key requirement is worse than the original
3. **No meta-commentary inside the prompt** — the enhanced prompts must not contain phrases like "as instructed" or "per your request"
4. **Domain-specific sharpening** — see `references/domain-rules.md` for code / data / creative / research specifics
5. **Token estimate delta** — always show original vs Option A token estimates so the user sees the saving

---

## Edge cases

| Situation | How to handle |
|---|---|
| Prompt is already excellent | Say so, give minor tweaks only, score 9/10 |
| Prompt is one word or < 5 words | Ask one clarifying question before enhancing |
| Prompt contains PII or secrets | Redact in the enhanced version and note it |
| Prompt is in another language | Enhance in the same language |
| Prompt is for image generation | Apply Option A/B only; skip Option C (agent-ready rarely applies to image prompts) |
| User provides a system prompt, not user prompt | Label output clearly as "System prompt variants" and adjust accordingly |

---

## Reference files

Read these when needed — do not load both by default:

- `references/anti-patterns.md` — Full checklist of 20+ prompt anti-patterns with examples and fixes
- `references/domain-rules.md` — Domain-specific enhancement rules for: code, data/SQL, creative writing, research, agent tasks
