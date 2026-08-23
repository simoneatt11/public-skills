# public-skills
Some skills I developed that may be useful for other people.

## Skills

### `prompt-debrief`

Analyzes a completed (or in-progress) Claude conversation and gives you structured, honest feedback on how well you prompted — clarity, redundancy, likely wasted cost, and your recurring habits. It ends by writing/updating a `CLAUDE.md` in your project (or `~/CLAUDE.md` globally) with terse, imperative rules distilled from the debrief, so the lessons persist automatically into your next session instead of evaporating.

**What it does, in order:**
1. Reads every user turn in the session and rates overall efficiency (High/Medium/Low)
2. Flags ambiguous, over-specified, or under-specified prompts turn by turn
3. Spots groups of turns that could've been one prompt (redundancy/collapse opportunities)
4. Flags prompts that likely triggered expensive/unnecessary regenerations
5. Rewrites your 3 weakest prompts, with a one-line "why it's better"
6. Names 1–2 recurring patterns in *how you personally prompt* — this is the most useful part
7. Gives you one concrete thing to try next time
8. Writes the distilled rules into `CLAUDE.md` so they auto-load into future sessions

**How to use it well:**
- **Run it at the end of every substantial Claude conversation** — not just when something went wrong. The value compounds: each debrief updates the same `CLAUDE.md`, so your standing instructions to Claude get sharper over time instead of resetting each session. Treat it like a retro, not a bug report.
- Trigger it naturally — `/debrief`, "review this session", "how was my prompting?", or just ask for a retrospective at the end of a long back-and-forth.
- Let it write to `CLAUDE.md` even on short sessions. A single rule added consistently beats a big rewrite done rarely.
- If you disagree with a suggested rule, say so — the skill is instructed to merge, not overwrite, so you can prune or edit `CLAUDE.md` by hand between debriefs.
- **This skill was originally shaped around my own prompting habits.** Before relying on it, skim `prompt-debrief/SKILL.md` and adjust the report sections, tone guidelines, or `CLAUDE.md` template to fit how *you* actually work — everyone's inefficiencies look different, and the skill is only as useful as the personality it's tuned to.

**Install:** copy `prompt-debrief/` into `~/.claude/skills/` (available in all projects) or `.claude/skills/` inside a specific project (scoped to that project only).
