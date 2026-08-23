---
name: prompt-debrief
description: >
  Analyze a completed Claude conversation and give the user structured feedback on their prompting efficiency, clarity, and cost-effectiveness. Use this skill whenever the user says things like "/debrief", "review this session", "how was my prompting?", "was this conversation efficient?", "give me feedback on my prompts", "how could I have prompted better?", or "analyze this chat". Also trigger at the end of a long back-and-forth session when the user asks for a summary or retrospective. Especially useful after Claude Code sessions. Use proactively if the user mentions wanting to improve their prompting habits.
---

# Prompt Debrief Skill

Analyze the current conversation and produce a structured debrief to help the user become a more efficient, cost-effective prompter.

## Your job

Read the full conversation history (all Human and Assistant turns visible in context). Then produce the debrief report below. Be honest and specific — vague praise is useless. The goal is actionable improvement.

---

## Debrief Report Structure

Produce the report in this exact order:

### 1. Session Overview
- Total number of user turns
- Estimated efficiency rating: **High / Medium / Low** with one sentence of justification
- Overall tone: collaborative, exploratory, iterative, unclear, etc.

### 2. Clarity Score (per prompt)
Go through each user turn and flag any that were:
- **Ambiguous** — Claude had to guess or ask a clarifying question
- **Over-specified** — so much detail it likely inflated the response unnecessarily
- **Under-specified** — missing context that caused a wrong first attempt
- **Good** — clear, well-scoped, got a useful response on the first try

Format: a compact table with columns: `Turn # | Summary | Issue | Suggestion`

Only include turns with issues. If all turns were clean, say so.

### 3. Redundancy & Collapse Opportunities
Identify groups of turns that could have been merged into one prompt.

Example format:
> Turns 3, 4, and 5 were sequential refinements of the same output. A single prompt like *"Do X, formatted as Y, and make sure Z"* would have achieved the same result in one round.

List up to 3 collapse opportunities. If none, say so.

### 4. Cost Signal
Estimate which prompts likely triggered large (expensive) responses and whether that was warranted:
- Flag any prompts that asked Claude to regenerate a large artifact when only a small change was needed
- Flag any prompts that were vague enough to require Claude to hedge extensively
- Flag missed opportunities to use targeted follow-ups instead of full rewrites

Keep this section to 3–5 bullet points.

### 5. Top 3 Rewrite Suggestions
Pick the 3 weakest prompts from the session. For each, show:
- **Original:** (the user's actual prompt, quoted briefly)
- **Improved:** (a rewritten version)
- **Why it's better:** one sentence

### 6. Prompting Patterns (meta-feedback)
Look across the whole session and identify 1–2 recurring habits — good or bad — in how this user prompts. Examples:
- "You tend to add constraints after seeing output rather than upfront — try front-loading requirements."
- "You're good at giving examples, which consistently produced better first attempts."
- "You often ask for 'something like X' without specifying format, leading to mismatches."

This is the most valuable section — be honest and specific.

### 7. One Thing to Try Next Time
End with a single, concrete experiment the user could try in their next session to measurably improve efficiency. Make it specific to their observed patterns, not generic advice.

---

## Tone guidelines

- Be direct and constructive, not flattering
- Use concrete examples from the actual conversation — don't be vague
- Keep the whole report skimmable (use tables and bullets liberally)
- Don't pad — if a section has nothing to flag, say "Nothing to flag here" and move on
- Adapt depth to session length: a 3-turn session gets a lighter debrief than a 20-turn one

---

## Step 8: Generate latest prompting context.md (always do this last)

After completing the debrief report, **always** generate or update a `latest prompting context.md` file. This file captures your distilled prompting preferences so they can carry into future sessions.

Note: unlike `CLAUDE.md`, Claude Code does **not** auto-load this file by name — it's a plain reference file. If you want it to auto-inject every session, add a line to your `CLAUDE.md` referencing it (e.g. `@latest prompting context.md`), or paste its contents in manually at the start of a new conversation.

### How to write it

Extract the most actionable insights from the debrief and distill them into terse, imperative instructions Claude should follow from session start. Think of it as a standing brief that primes Claude with your working style.

### File location

- **If the user is in a project directory** (Claude Code context): write to `./latest prompting context.md` in the current working directory
- **Otherwise**: write to `~/latest prompting context.md` as a global user-level file
- **If a `latest prompting context.md` already exists**: read it first, then merge — preserve existing rules, update or add based on new learnings. Never delete rules unless the user explicitly asks.

### latest prompting context.md structure

```markdown
# My Prompting Preferences
> Last updated: {DATE} · Session: {brief topic of the session}

## How I communicate
<!-- 1–3 sentences about the user's natural style, so Claude can match it -->

## What to do at session start
<!-- Things Claude should do automatically without being asked -->
- ...

## Prompt efficiency rules (learned from debriefs)
<!-- Distilled from Section 6 "Prompting Patterns" — updated each debrief -->
- ...

## Cost guardrails
<!-- Rules to avoid expensive unnecessary work — updated from Section 4 -->
- ...

## Standing preferences
<!-- Persistent output format, tool, or style preferences observed across sessions -->
- ...
```

### Rules for writing good latest prompting context.md entries

- **Imperative and terse**: "Always ask for output format before starting" not "The user prefers to specify output format"
- **Behavioral, not descriptive**: rules Claude can act on, not observations about the user
- **Dated**: always update the `Last updated` line so the user knows it's fresh
- **Additive**: each debrief adds or refines — the file grows smarter over time
- **Max 40 lines**: if it grows beyond this, consolidate and remove stale rules

### After writing the file

Tell the user:
1. Where the file was written
2. What was added or changed vs the previous version (if it existed)
3. One-line reminder: "This file isn't auto-loaded like CLAUDE.md — reference it with `@latest prompting context.md` from your CLAUDE.md, or paste it in manually, if you want it applied automatically."

---

## Edge cases

- **Very short session (≤3 turns):** Give abbreviated feedback, focus only on sections 2 and 7
- **Code-heavy session (Claude Code):** Pay extra attention to Cost Signal — regenerating large files for small edits is the #1 efficiency drain in code sessions
- **Creative/exploratory session:** Lower bar for "redundancy" — iteration is expected. Focus instead on whether the user anchored their creative direction early or drifted
- **If the user shares a conversation from another session:** Analyze that conversation, not the current one
- **latest prompting context.md always gets written** — even for short sessions. A 2-turn session might only add one rule, but the habit of updating it is the point.
- **User says "just the file" / "just latest prompting context.md":** Skip the full debrief, generate only the file with a one-paragraph summary of what changed and why.
