# Prompt Templates Reference

Full template library for Prompt Master (pi edition). Read the relevant template when the user's task type matches. Do not load all templates at once — only the one you need.

## Table of Contents

| Template | Best For |
|----------|----------|
| [A — RTF](#template-a--rtf) | Simple one-shot tasks |
| [B — CO-STAR](#template-b--co-star) | Professional documents, business writing |
| [C — RISEN](#template-c--risen) | Complex multi-step projects |
| [D — CRISPE](#template-d--crispe) | Creative work, brand voice |
| [E — Auditable Reasoning](#template-e--auditable-reasoning) | Logic, math, analysis, debugging |
| [F — Few-Shot](#template-f--few-shot) | Consistent structured output, pattern replication |
| [G — Pi Task Brief](#template-g--pi-task-brief) | Any pi task touching code, files, or commands |
| [H — Pi Skill Prompt](#template-h--pi-skill-prompt) | Writing a new skill's description and SKILL.md |
| [I — Prompt Decompiler](#template-i--prompt-decompiler) | Breaking down, adapting, or splitting existing prompts |

Templates A–F are surface-agnostic prompt architectures. Templates G–H are pi-native. Use G by default for coding work — pi is agentic, so scope locks and done-criteria are not optional.

---

## Template A — RTF

*Role, Task, Format. Use for fast one-shot tasks where the request is clear and simple.*

```
Role: [One sentence defining who the AI is]
Task: [Precise verb + what to produce]
Format: [Exact output format and length]
```

**Example:**
```
Role: You are a senior technical writer.
Task: Write a one-paragraph description of what a REST API is.
Format: Plain prose, 3 sentences maximum, no jargon, suitable for a non-technical audience.
```

---

## Template B — CO-STAR

*Context, Objective, Style, Tone, Audience, Response. Use for professional documents, business writing, reports, and marketing content where full context control matters.*

```
Context: [Background the AI needs to understand the situation]
Objective: [Exact goal — what success looks like]
Style: [Writing style: formal / conversational / technical / narrative]
Tone: [Emotional register: authoritative / empathetic / urgent / neutral]
Audience: [Who reads this — their knowledge level and expectations]
Response: [Format, length, and structure of the output]
```

**Example:**
```
Context: I am a founder pitching a B2B SaaS tool that automates expense reporting for mid-size companies.
Objective: Write a cold email that gets a reply from a CFO.
Style: Direct and conversational, not salesy.
Tone: Confident but not pushy.
Audience: CFO at a 200-person company, busy, skeptical of vendor emails.
Response: 5 sentences max. Subject line included. No bullet points.
```

---

## Template C — RISEN

*Role, Instructions, Steps, End Goal, Narrowing. Use for complex projects, multi-step tasks, and any output that requires a clear sequence of actions.*

```
Role: [Expert identity the AI should adopt]
Instructions: [Overall task in plain terms]
Steps:
  1. [First action]
  2. [Second action]
  3. [Continue as needed]
End Goal: [What the final output must achieve]
Narrowing: [Constraints, scope limits, what to exclude]
```

**Example:**
```
Role: You are a product manager with 10 years of experience in mobile apps.
Instructions: Write a product requirements document for a habit tracking feature.
Steps:
  1. Define the problem statement in one paragraph
  2. List user stories in the format "As a [user], I want [goal] so that [reason]"
  3. Define acceptance criteria for each story
  4. List out-of-scope items explicitly
End Goal: A PRD that an engineering team can begin sprint planning from immediately.
Narrowing: No technical implementation details. No wireframes. Under 600 words total.
```

---

## Template D — CRISPE

*Capacity, Role, Insight, Statement, Personality, Experiment. Use for creative work, brand voice writing, and any task where personality, tone, and iteration matter.*

```
Capacity: [What capability or expertise the AI should have]
Role: [Specific persona to adopt]
Insight: [Key background insight that shapes the response]
Statement: [The core task or question]
Personality: [Tone and style — witty / authoritative / casual / sharp]
Experiment: [Request variants or alternatives to explore]
```

**Example:**
```
Capacity: Expert copywriter specializing in SaaS product launches.
Role: Brand voice for a productivity tool aimed at developers.
Insight: Developers hate marketing speak and respond to honesty and specificity.
Statement: Write the hero headline and sub-headline for the landing page.
Personality: Sharp, dry, confident — no adjectives, no exclamation marks.
Experiment: Give 3 variants ranging from minimal to bold.
```

---

## Template E — Auditable Reasoning

*Use for logic-heavy tasks, math, debugging, and multi-factor analysis where the result must be checkable without requesting private reasoning.*

```
[Task statement]

Return:
1. Conclusion
2. Assumptions
3. Evidence or intermediate results needed to audit the conclusion
4. Verification checks performed
5. Remaining uncertainty, if any

Do not reveal hidden chain-of-thought or private reasoning. Keep the rationale concise and decision-relevant.
```

**When to use:**
- Debugging where the cause is not obvious
- Comparing technical approaches
- Math or calculation requiring verification
- Analysis where evidence and assumptions must be inspectable

**When NOT to use:**
- Simple tasks where the answer is clear
- Creative tasks where an audit trail adds noise

---

## Template F — Few-Shot

*Use when the output format is easier to show than describe. Examples outperform written instructions for format-sensitive tasks every time.*

```
[Task instruction]

Here are examples of the exact format needed:

<examples>
  <example>
    <input>[example input 1]</input>
    <output>[example output 1]</output>
  </example>
  <example>
    <input>[example input 2]</input>
    <output>[example output 2]</output>
  </example>
</examples>

Now apply this exact pattern to: [actual input]
```

**Rules:**
- 2 to 5 examples is the sweet spot. More rarely helps and wastes tokens.
- Examples must include edge cases, not just easy cases.
- If the same formatting correction has been needed twice, switch to few-shot instead of rewriting instructions.

---

## Template G — Pi Task Brief

*Default template for any pi prompt that touches code, files, commands, or dependencies. Pi is agentic with real system access — front-load outcome, scope, boundaries, and a runnable done-check. Do not script every step; state what done looks like and let pi's tool loop work.*

```
## Objective
[What needs to be built, fixed, or produced — one clear sentence. Add WHY if it affects approach.]

## Context
[What exists now — relevant file paths, current behavior, stack already in place, what was tried and failed]

## Target State
[What done looks like — specific files changed, behavior produced. Binary where possible.]

## Scope
- Work only in: [specific files and directories]
- Do NOT touch: [forbidden files — .env, lockfiles, configs, anything outside scope]

## Constraints
- [Stack version, naming conventions, no new dependencies without asking]
- Only make changes directly requested. Do not add features, abstractions, or files beyond what was asked.

## Done When
- [ ] [Binary check 1 — ideally a runnable command: `npm test`, `tsc --noEmit`, `cargo check`]
- [ ] [Binary check 2]
- [ ] [Binary check 3]

## Stop and Ask Before
- Deleting any file
- Adding any dependency
- Any change outside the stated scope
- [Other destructive or irreversible actions]

## Progress Evidence
For long-running work, report progress only when it changes or a checkpoint is reached. Ground every completion claim in a tool result, changed artifact, or verification output.
```

**Example:**
```
## Objective
Fix the intermittent 401 on token refresh so active sessions survive 24h.

## Context
`src/auth/middleware.ts` checks expiry with `<` at line 42; tokens expiring in the same second as the check fail. Jest suite in `src/auth/__tests__/`.

## Target State
Expiry check uses `<=`; refresh path covered by a regression test.

## Scope
- Work only in: src/auth/
- Do NOT touch: src/auth/legacy/, package.json, any config file

## Constraints
- TypeScript strict mode; no new dependencies
- Only make changes directly requested.

## Done When
- [ ] `npm test -- auth` passes including new regression test
- [ ] `npx tsc --noEmit` clean

## Stop and Ask Before
- Changing the token format or secret handling

## Progress Evidence
Report test output with the final summary.
```

**When to use:** Every pi session task involving code or files. Trim sections for small tasks — Objective + Scope + Done When is the minimal viable brief.

---

## Template H — Pi Skill Prompt

*Use when the user wants a prompt shaped as a pi skill. Pi advertises skills by name + description only; the body loads on match. The `description` field IS the router — it must state what the skill does AND when it applies, or the skill never fires.*

```
Directory: .pi/skills/[skill-name]/SKILL.md   (or ~/.pi/skills/ for personal)

---
name: [lowercase-hyphens, matches directory]
description: [What it does + when to activate + explicit "Does not activate for" exclusions. This is routing text read by the model — front-load trigger phrases the user would actually say. Max 1024 chars.]
---

# [Skill Name]

[Direct instructions. Assume the model already read this file — no meta-preamble.]

[Rules, workflow steps, constraints — imperative voice, strongest signal words.]

[Reference additional files with paths relative to this skill directory. Load on demand, not all at once.]
```

**Rules:**
- Description must contain activation triggers AND exclusions — both halves prevent misrouting
- Keep SKILL.md bodies lean; push bulk into `references/` files read on demand
- Never invent pi frontmatter fields — valid: `name`, `description`, `license`, `compatibility`, `metadata`, `allowed-tools`, `disable-model-invocation`
- After installing or editing a skill, remind the user: restart pi or run `/reload`; force with `/skill:name`

**Example description:**
```
description: Reviews git diffs for over-engineering — speculative abstractions,
reinvented stdlib, dead config. Activates on "review for over-engineering",
"simplify review", or /simplify-review. Does not activate for correctness,
security, or performance review.
```

---

## Template I — Prompt Decompiler

*Use when the user pastes an existing prompt and wants to break it down, adapt it for a different pi surface, simplify it, or understand its structure. This is analysis and adaptation, not building from scratch.*

**Detect which decompiler task is needed:**
- **Break down** — explain what each part of the prompt does
- **Adapt** — rewrite for a different pi surface (session prompt → skill, skill → template, etc.)
- **Simplify** — remove redundancy and tighten without losing meaning
- **Split** — divide a complex one-shot prompt into a cleaner sequence

**For Adapt tasks, always ask:**
"What surface is the original prompt for, and which pi surface are you adapting it to?"

**Break down output format:**
```
Original prompt: [paste]

Structure analysis:
- Role/Identity: [what role is assigned and why]
- Task: [what action is being requested]
- Constraints: [what limits are set]
- Format: [what output shape is expected]
- Weaknesses: [what is missing or could cause wrong output]

Recommended fix: [rewritten version with gaps filled]
```

**Adapt output format:**
```
Original ([source surface]): [original prompt]

Adapted for [target pi surface]:
[rewritten prompt using the target surface's structure — see Template G or H]

Key changes made:
- [change 1 and why]
- [change 2 and why]
```

**Split output format:**
```
Original prompt: [paste]

This prompt is doing [N] things. Split into [N] sequential prompts:

Prompt 1 — [what it handles]:
[prompt block]

Prompt 2 — [what it handles]:
[prompt block]

Run these in order. Each output feeds the next.
```
