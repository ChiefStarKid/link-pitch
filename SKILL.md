---
name: link-pitch
description: SEO link insertion strategy for high-value content sites. Given a client and target article, research both, identify blue ocean keyword gaps, and generate strategic link insertion suggestions (simple placement, comprehensive edit, new content pitch). Use this when you want to propose a natural link on an existing article, find underserved angles for guest posting, or develop a value-led pitch to a publisher. Works best for premium content sites where editorial quality matters.
---

# link-pitch

Generate strategic link insertion options and external pitches for SEO link
building campaigns — grounded in real research, focused on filling blue ocean
keyword gaps rather than forcing a link.

## Quick start

```
/link-pitch
```

Or, power-user mode:

```
/link-pitch <client-url> <linked-page-url> <target-site-url>
```

If the popup closes mid-run, type `/link-pitch` again to resume where you left off.

## How it works

A 7-step flow with four checkpoints so you stay in control. Full mechanics,
verbatim agent prompts, and output templates live in `references/` — read them
when you reach the relevant step.

1. **Gather inputs** — client site, target article (or let it find candidates),
   the page the link points to, output folder, and outreach style.
2. **Research + contact gate** — analyze client and article in parallel, then
   verify a publisher contact route exists (email, form, contributor page, or
   editor social). If none, abort or override.
3. **Fit check** — confirm the client's audience overlaps the article's readers.
4. **Anchor text** — propose SEO-optimized link text (or use yours).
5. **Generate options** — 3 placement types × 3 variations = 9 options.
6. **Internal proposal** — synthesise research + options + a recommendation.
7. **External proposal** — you curate options; it writes the final publisher pitch.

**Output:** three markdown files — options, internal proposal, external pitch.

→ Execution detail: `references/steps.md`
→ Output file structures: `references/templates.md`

## Two configurable styles

Voices are plain markdown files you can edit or replace — this is the "drop your
style guide here" mechanism.

| File | Governs | Default |
|---|---|---|
| `styles/internal.md` | Steps 3–6, file summaries, all messages to you | direct, no-fluff working voice |
| `styles/outreach.md` | Step 7 external pitch only | neutral, confident publisher voice |

- **Internal** is read at runtime relative to the skill root. Edit it to change how
  the skill talks to you.
- **Outreach** defaults to `styles/outreach.md`. At Step 1 you can keep the default,
  point to your own `.md` voice file, or choose "No style — plain output". The two
  styles never mix in a single output.

## Checkpoints

The flow pauses at four decision points:

1. **After Step 2 (contact gate):** if no contact route is found, abort or override.
2. **After Step 3 (fit check):** audience fit score — proceed or pick another article.
3. **After Step 4 (anchor text):** approve, amend, or try again.
4. **After Step 6 (internal proposal):** review the recommendation before the pitch.

## Failure handling

- **Paywalled article** → paste the article text instead.
- **Uncrawlable client site** → provide a brief description.
- **Weak audience fit** → warned at Step 3; you decide whether to continue.
- **No contact info** → four criteria evaluated after research; if none found, abort
  or override (a caveat is logged in all output files).

See `references/steps.md` for the full error-handling table.

## Key rules

- **Never fabricate claims** about the client. Only use what's on the website.
- **Edits must match article tone.** Casual article → casual suggestions.
- **Option B (comprehensive edit) must add value.** Reject pure marketing copy.
- **Blue ocean focus.** Prioritise filling identified gaps over mere insertion points.
- **Resume state is scoped.** Don't affect global plan mode or other commands.
- **Internal style governs all internal output;** outreach style governs Step 7 only.
  The two never mix in the same file.
- **Contact gate is not optional.** Always evaluate all four contact criteria
  explicitly after Step 2 before proceeding.
- **Override caveat propagates silently.** If overridden, insert the caveat into the
  Step 6 and Step 7 files automatically — don't re-ask or announce it.

## Installation

Copy this `link-pitch/` folder into your skills directory (`~/.claude/skills/`),
then invoke with `/link-pitch`. See `README.md` for details.
