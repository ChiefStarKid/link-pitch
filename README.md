# link-pitch

A Claude Code skill for **strategic SEO link insertion**. Point it at a client and a
target article and it researches both, finds the underserved keyword gaps ("blue
ocean"), and generates link placement options plus a publisher-ready pitch — grounded
in real research, not guesswork.

It's built for **premium content sites where editorial quality matters**: the goal is
a link an editor actually wants, because it genuinely serves their readers.

## What it produces

Three markdown files per run:

| File | Contents |
|---|---|
| `LinkPitch-options-<slug>.md` | All 9 placement variations (3 types × 3) |
| `LinkPitch-internal-<slug>.md` | Full analysis, recommendation, fallback design |
| `LinkPitch-external-<slug>.md` | The final publisher pitch, in your outreach voice |

## How it works

A 7-step flow with four checkpoints — see [`SKILL.md`](SKILL.md) for the overview and
[`references/steps.md`](references/steps.md) for the full mechanics:

1. Gather inputs → 2. Research both + contact gate → 3. Audience fit → 4. Anchor text →
5. Generate 9 options → 6. Internal proposal → 7. External pitch.

It uses parallel research agents and web search, so it works best in a Claude Code
environment with those tools available.

## Install

Copy the `link-pitch/` folder into your skills directory:

```
~/.claude/skills/link-pitch/
```

Then invoke it in Claude Code:

```
/link-pitch
```

## Configure your voice

Two style files control tone. Both are plain markdown — edit or replace them.

- **`styles/internal.md`** — how the skill talks to *you* and writes internal docs.
  Default is a direct, no-fluff working voice.
- **`styles/outreach.md`** — the voice of the *external publisher pitch* (Step 7).
  Ships with a neutral, reader-first default. Replace its contents with your own
  outreach voice, or point the skill at a different `.md` file when prompted at Step 1.
  Choose "No style — plain output" to disable styling entirely.

## License

MIT — see [`LICENSE`](LICENSE).
