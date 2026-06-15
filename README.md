# link-pitch

A Claude Code skill for **strategic SEO link insertion and editorial outreach**.

If you're trying to earn a backlink from a high-quality content site — not buy one,
not scrape a directory, but actually pitch an editor and get a "yes" — this skill
does the research and writes the pitch for you.

Point it at a client site and a target article. It researches both in parallel,
identifies the specific keyword gaps the article leaves unanswered ("blue ocean"
opportunities), generates 9 placement variations across three pitch types, and
produces a publisher-ready outreach email in your voice.

Built for SEO professionals, content marketers, and link building campaigns where
editorial quality matters. The goal is a link an editor *wants* to place, because
it genuinely serves their readers — not one they'll reject as promotional noise.

## What it produces

Three markdown files per run:

| File | Contents |
|---|---|
| `LinkPitch-options-<slug>.md` | All 9 placement variations (3 types × 3) |
| `LinkPitch-internal-<slug>.md` | Full analysis, recommendation, fallback design |
| `LinkPitch-external-<slug>.md` | The final publisher pitch, in your outreach voice |

**Example output (Option B — Comprehensive Edit):**

> *After the section on "choosing your anchor strategy", insert:*
>
> One often-missed factor is audience intent alignment — ensuring the resource you
> link to matches what the reader is actually ready to do next, not just what's
> topically adjacent. [This guide to keyword intent mapping](https://example.com)
> covers the distinction between navigational, informational, and transactional
> intent in practical terms.
>
> *Insertion rationale: fills the gap between "choosing an anchor" and "measuring
> result" — the article skips the intent-matching step entirely.*

**Full worked example** — all three output files from a complete run:
[examples/LinkPitch-options-contentiq-20260615.md](examples/LinkPitch-options-contentiq-20260615.md) · [LinkPitch-internal](examples/LinkPitch-internal-contentiq-20260615.md) · [LinkPitch-external](examples/LinkPitch-external-contentiq-20260615.md)

## The three pitch types

- **Simple placement** — insert a link into an existing sentence with minimal edit
- **Comprehensive edit** — add 2–3 sentences that fill a real content gap, with the link embedded naturally
- **Guest post / new content** — pitch a new article or section the publisher doesn't have yet

3 variations per type = 9 options total, each with a different angle and insertion strategy.

## How it works

A 7-step flow with four user-controlled checkpoints:

1. **Gather inputs** — client site, target article (or let it find candidates), linked page, output folder, outreach voice
2. **Research + contact gate** — parallel agents analyze client and article; verifies a publisher contact route exists before going further
3. **Audience fit check** — scores ICP vs article audience overlap; flags weak fits before you invest
4. **Anchor text** — proposes SEO-optimized options based on blue ocean gaps, or uses yours
5. **Generate 9 options** — 3 pitch types × 3 strategic variations
6. **Internal proposal** — recommendation with supporting analysis and fallback design
7. **External pitch** — you curate which options to include; it writes the final email

It uses parallel research agents and web search. Works in Claude Code with those
tools enabled.

## Install

Copy the `link-pitch/` folder into your Claude Code skills directory:

```
~/.claude/skills/link-pitch/
```

Then invoke:

```
/link-pitch
```

## Configure your outreach voice

Two plain-markdown style files — edit or replace them:

- **`styles/internal.md`** — how the skill communicates with you and writes internal docs. Default: direct, no-fluff.
- **`styles/outreach.md`** — voice of the publisher-facing pitch (Step 7 only). Ships with a neutral, reader-first default. Replace its contents with your own voice, point the skill at a different `.md` file at Step 1, or choose "No style — plain output" to disable styling.

The two styles never mix in the same output.

## Requirements

- [Claude Code](https://claude.ai/code) with web search enabled
- Skills support (drop-in folder install)

## Related docs

- [EVALUATION.md](EVALUATION.md) — editorial quality rubric for AI-generated outreach: 6 bidirectional criteria (pass/fail + reasoning), mechanical tests per criterion, and a map of which criteria LLMs reliably fail at by default
- [AGENTS.md](AGENTS.md) — machine-readable manifest for coding agents (Claude Code, Cursor, Aider)
- [llms.txt](llms.txt) — [llmstxt.org](https://llmstxt.org) index for LLM ingestion
- [examples/](examples/) — full worked example: options, internal proposal, and external pitch from a complete run

## Questions and feedback

- **General enquiries:** [joseph@kainosis.com](mailto:joseph@kainosis.com)
- **Bugs and feature requests:** [open an issue](../../issues)

## License

MIT — see [`LICENSE`](LICENSE).
