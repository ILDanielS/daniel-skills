---
name: how
description: Explain how some part of the codebase works by exploring the code. Use when the user asks "how does X work?", "how is X implemented?", "walk me through X", or invokes /how. Scales effort to the question — a single explainer for narrow scope, parallel explorers plus a synthesizer for broad subsystems.
argument-hint: <what to explain, e.g. "the auth middleware">
---

# How

Answer "How does X work?" about the current codebase. The subject is: **$ARGUMENTS**
(If empty, use the user's question from the conversation.)

## Subagents

You spawn every subagent yourself with the Agent tool. Each role's prompt is a template in this skill's `references/` directory. Read the template, fill in every placeholder, and pass the result **verbatim** as the Agent `prompt`. Do not paraphrase or shorten it. If a template has a header above a `---` line, that header is instructions for you; pass only the part below the `---`.

| Role | Prompt template | subagent_type | model | effort |
|---|---|---|---|---|
| Explorer | [references/explorer.md](references/explorer.md) | `Explore` | `opus` (Opus 5.5) | xhigh |
| Explainer | [references/explainer.md](references/explainer.md) | `Explore` | `opus` (Opus 5.5) | xhigh |
| Synthesizer | [references/explainer.md](references/explainer.md) | `Explore` | `opus` (Opus 5.5) | xhigh |

All subagents use `subagent_type: Explore` so they are read-only. Always pass `model` explicitly and `run_in_background: false`. If the Agent tool accepts an effort setting, pass the effort from the table.

## Step 1 — Assess complexity

Do a quick orientation yourself (a few Grep/Glob calls, no deep reading) to find where the subject lives and how big it is. Then classify:

- **Simple** — a single module, class, function, or otherwise narrow scope that lives in one or a few closely related files. → Step 2b.
- **Complex** — spans multiple files, modules, layers or services; involves a flow across components (request lifecycle, build pipeline, sync engine, etc.). → Step 2a.

When unsure, choose Complex. Tell the user in one line which path you chose and why.

## Step 2a — Explore (complex)

1. Split the subsystem into **2 to 4 exploration angles**. Each angle is a distinct part of the subsystem (e.g. entry points & public API, core data model & state, main control flow, persistence/IO, config & extension points, error handling). Angles must not overlap; together they should cover the subject. Use 2 for moderately broad subjects, 4 for large ones.
2. Read `references/explorer.md`. For each angle, fill `{QUESTION}` with the user's question and `{EXPLORATION_ANGLE}` with what this angle covers, the starting files/dirs you found in Step 1, and one line per other angle so explorers stay in their lane.
3. Spawn **all explorers in parallel in a single message** (Explorer row of the table).
4. Wait for all of them to return, then go to Step 3.

## Step 2b — Explain (simple)

Read `references/explainer.md`, fill `{{subject}}` and `{{starting_points}}` (from Step 1), and spawn one agent (Explainer row of the table). It explores and explains in one pass. Go to Step 4.

## Step 3 — Synthesize (complex only)

The synthesizer uses the explainer prompt. Read `references/explainer.md`, fill `{{subject}}`, and fill `{{starting_points}}` with every explorer's full report, each under a heading naming its angle. Do not summarize the reports before passing them. Spawn one agent (Synthesizer row of the table).

## Step 4 — Present

Present the explainer's or synthesizer's output to the user. Light edits for clarity are fine (fix formatting, broken references, obvious typos, trim repetition). Do **not** rewrite it. The output must follow this structure:

1. **Overview**
2. **Key concepts**
3. **How it works**
4. **Where things live**
5. **Gotchas**

If a section is missing or the output is clearly wrong about something you can verify quickly, fix that part only and note it.
