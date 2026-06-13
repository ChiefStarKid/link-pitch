# AGENTS.md — link-pitch skill manifest

Instructions for coding agents (Claude Code, Cursor, Aider, etc.) wiring or invoking this skill.

## What this is

`link-pitch` is a Claude Code skill — an interactive multi-step workflow, not a script or library. It runs inside a Claude Code conversation, uses web search and parallel research agents, and pauses at four checkpoints for user decisions. It does not shell out and has no CLI invocation pattern.

## Install

Drop the `link-pitch/` folder into the user's Claude Code skills directory:

```
~/.claude/skills/link-pitch/
```

No other setup required. The skill auto-discovers its own style files relative to the skill root.

## Invocation

```
/link-pitch
```

Or with pre-filled inputs (power-user mode):

```
/link-pitch <client-url> <linked-page-url> <target-site-url>
```

## What the skill does (step summary)

| Step | What happens | Pauses? |
|---|---|---|
| 0 | Check for saved session state; offer resume | Only if prior session found |
| 1 | Gather inputs: client URL, linked page, target article (or discover), output folder, outreach style | Always |
| 2 | Parallel research: Company Researcher + Article Analyzer agents | No |
| 2.5 | Contact gate: verify publisher contact route exists | Only if no contact found |
| 3 | Audience fit check: score ICP vs article audience overlap | Always |
| 4 | Anchor text: propose or confirm link text | Always |
| 5 | Generate 9 placement variations (3 types × 3 strategies) | No |
| 6 | Internal proposal: synthesis, recommendation, fallback design | Always |
| 7 | External pitch: user curates options, skill writes publisher email | Always |

## Output files

| File | Contents |
|---|---|
| `LinkPitch-options-<slug>.md` | All 9 variations |
| `LinkPitch-internal-<slug>.md` | Analysis + recommendation + fallback |
| `LinkPitch-external-<slug>.md` | Final publisher pitch in outreach voice |

Written to the output folder specified at Step 1.

## Style files

| File | Governs | Editable? |
|---|---|---|
| `styles/internal.md` | Steps 3–6, all working communications | Yes — edit freely |
| `styles/outreach.md` | Step 7 external pitch only | Yes — replace with your own voice |

Path resolution is relative to the skill root. If `styles/outreach.md` is missing, the skill falls back to confident plain prose.

## Session state

Saved to `~/.claude/temp/link-pitch-session.json` across the four checkpoints. If a session was interrupted, `/link-pitch` offers to resume. Deleted on successful completion of Step 7.

## Hard rules

1. **Never fabricate client claims.** Only use what is visible on the client's website.
2. **Contact gate is not optional.** Always evaluate all four contact criteria after Step 2 before proceeding — do not skip even if research "seems" to mention contact info.
3. **Option B (comprehensive edit) must add editorial value.** Reject any variation that is pure marketing copy.
4. **Blue ocean first.** Prioritise filling identified content gaps over finding convenient insertion points.
5. **Override caveat propagates silently.** If the contact gate was overridden, insert the caveat into Step 6 and Step 7 output files automatically — do not re-ask or announce it.
6. **Internal and outreach styles never mix.** Internal style governs Steps 3–6 and all working comms; outreach style governs Step 7 only.

## Repo

Canonical source: <https://github.com/ChiefStarKid/link-pitch>
