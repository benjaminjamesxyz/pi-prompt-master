# Prompt Master for Pi

A [pi](https://pi.dev) skill that writes accurate prompts for the pi coding agent. Zero tokens wasted. No re-prompting your way to an answer you should have gotten on attempt one.

**Exclusive to pi** — generates task briefs, skill descriptions, prompt templates, and extension instructions grounded in pi's real harness: its tools, skills system, trust model, and agentic workflow.

---

## 🚀 Install

### From npm (once published)

```bash
pi install npm:pi-prompt-master
```

### From git

```bash
pi install git:github.com/benjaminjamesxyz/pi-prompt-master@main
```

### From local clone (development)

```bash
pi install /home/simson/pi-projects/pi-prompt-master-skill/prompt-master
```

Or symlink just the skill:

```bash
ln -s /path/to/prompt-master/skills/prompt-master ~/.pi/skills/prompt-master
```

Restart pi (or run `/reload`) after installing. Verify with `pi list` or `/skill:prompt-master`.

---

## 🔥 The Problem This Solves

Every agent user wastes tokens the same way:

> Write vague prompt → get wrong output → re-prompt → get closer → re-prompt again → finally get what you wanted on attempt 4

That's 3 wasted API calls. Multiply by 50 prompts a day.

### The key insight

> "The best prompt is not the longest. It's the one where every word is load-bearing."

Most "prompt generators" make prompts longer. This skill makes them sharper — and grounds them in what pi actually has: file tools, bash, skills, MCP, project trust.

---

## 🎯 Usage

Invoke naturally:

```
Write me a pi prompt to refactor my auth module
```

```
I need a skill that reviews my git diffs for scope creep — write the SKILL.md
```

```
Here's a bad prompt I gave pi, fix it: [paste prompt]
```

```
Turn this one-shot prompt into a prompt template for pi
```

Or explicitly:

```
/skill:prompt-master write a task brief for building a REST endpoint with tests
```

---

## How It Works

1. **Detects the pi surface** — session task brief, skill, prompt template, or extension instructions
2. **Extracts 9 dimensions of intent** — task, surface, output, constraints, context, audience, success criteria, examples
3. **Asks targeted clarifying questions** — max 3 if critical info is missing, never more
4. **Routes to the right template** — Pi Task Brief by default for code work; skill template for SKILL.md authoring
5. **Applies safe techniques only** — role assignment, few-shot, grounding anchors, explicit verification criteria
6. **Checks pi recency** — verifies settings keys and tool names against official docs when the prompt depends on them
7. **Runs a token efficiency audit** — strips every word that doesn't change the output
8. **Delivers the prompt** — one clean copyable block with a one-line strategy note

---

## Structure

```
prompt-master/                 ← pi package root
├── package.json               ← "pi-package" keyword → pi.dev gallery
└── skills/
    └── prompt-master/
        ├── SKILL.md           ← core instructions (loaded on match)
        └── references/
            ├── templates.md   ← 9 templates, loaded on demand
            └── patterns.md    ← 37 failure patterns, loaded on demand
```

---

## License

MIT — see [LICENSE](LICENSE).
