# Editorial Outreach Quality Rubric

A content quality evaluation framework for AI-generated link insertion pitches.

This rubric is what the link-pitch skill checks against before surfacing output to the user. It is published here as a standalone document so it can be adapted, extended, or used independently of the skill.

---

## What this rubric evaluates

AI models generating editorial outreach default to a "helpful assistant" register — deferential, hedged, generic. That register is the opposite of what earns a reply from an editor. This rubric defines the quality bar the skill enforces: confident, specific, reader-first prose that an editor receives as useful rather than promotional.

The rubric applies to the **external pitch only** (the publisher-facing output). Internal documents and working communications use a different voice standard.

---

## Evaluation criteria

Six criteria. Each is bidirectional — the pass condition and the fail condition are both defined, so the rubric is applicable to novel cases, not just the failure modes it was built from.

---

### Criterion 1 — Opener specificity

**What it tests:** Does the opener prove the writer read the article, or could it be sent to anyone?

| Rating | Description |
|---|---|
| **Pass** | References something specific — a named section, a specific claim, a particular framing choice in the article. The editor could not receive this opener from anyone else targeting the same publication. |
| **Fail** | Generic compliment or observation that could apply to any article on the same topic. "Your piece is the most practical framing I've come across" — could be sent to fifty writers. |

**Why it matters:** The opener is the only sentence the editor uses to decide whether to keep reading. Generic openers signal mail-merge. The pitch stops there.

**Test:** Remove the article title and publication name from the opener. If it still makes sense, it fails.

---

### Criterion 2 — Hook directness

**What it tests:** Is the observation stated directly, or hedged into timidity?

| Rating | Description |
|---|---|
| **Pass** | States the observation as a fact: "Your calendar piece skips the benchmark layer entirely." Direct. No preamble. |
| **Fail** | Passive or tentative framing: "One thing caught my eye", "I noticed something that might be worth considering", "I thought it might be interesting to…" |

**Why it matters:** Editors read dozens of pitches. Hedged language reads as low confidence in the observation itself. If the writer isn't sure the point is worth making, the editor won't be either.

**Test:** Does the hook work if you remove every word before the core observation? If removing the preamble makes it stronger, the preamble is failing.

---

### Criterion 3 — Link justification (absence of)

**What it tests:** Is the link earning itself through the surrounding argument, or is there a separate "why this link" section defending it?

| Rating | Description |
|---|---|
| **Pass** | No standalone justification block. The link placement, the gap it fills, and the reader value are woven into the pitch argument. The editor understands why the link belongs because the pitch is well-constructed, not because it was explained. |
| **Fail** | Dedicated section with "Why this link works" or "Why we think this is a good fit" — signals the writer wasn't confident the pitch made the case on its own. Reads as insecurity. |

**Why it matters:** A separate justification block tells the editor the writer knows the pitch is weak. Strong pitches don't need a defence section.

**Test:** Delete any paragraph whose sole function is justifying the link's existence. If the pitch is stronger without it, it was failing.

---

### Criterion 4 — Close confidence

**What it tests:** Does the pitch end with a clear preference and a direct CTA, or does it hedge at the moment of ask?

| Rating | Description |
|---|---|
| **Pass** | States a primary option and a direct next step: "B1 is my pick. Reply and I'll adjust to your house style." No apology for having a preference. |
| **Fail** | Hedge close: "Happy to adjust any of this to fit your needs", "Let me know if any of these feel right", "No pressure at all — just wanted to share." |

**Why it matters:** Hedge closes undo the entire pitch. They signal that the writer doesn't believe in their own recommendation. Editors don't make decisions for tentative people.

**Test:** Would a confident person in a sales meeting end with this sentence? If not, rewrite it.

---

### Criterion 5 — Register consistency

**What it tests:** Is the vocabulary and tone appropriate for an editor, or does it contain language from a different professional context?

| Rating | Description |
|---|---|
| **Pass** | Reads like a knowledgeable writer talking to another writer. Language is specific, direct, and unpretentious. |
| **Fail** | Marketing-speak in any form: "great value", "synergy", "low-hanging fruit", "anchor content", "leverage", "value-add", "circle back", "key takeaways". Also: SEO jargon ("DA", "backlink profile", "link equity") — the editor cares about readers, not rankings. |

**Why it matters:** Register mismatch tells the editor this pitch was written for a different audience. An editor who receives "this piece has strong anchor content potential" stops reading.

**Test:** Read the pitch as if you are the editor. Does any phrase remind you that you are being pitched? If yes, it fails.

---

### Criterion 6 — Mechanical accuracy

**What it tests:** Are there grammar errors, double constructions, or proofreading failures?

| Rating | Description |
|---|---|
| **Pass** | Clean on first read. No redundant constructions, no awkward phrasing, no errors that a read-aloud would catch. |
| **Fail** | Double adverbs or redundant phrases: "precisely answers exactly", "very uniquely", "currently ongoing". Grammatical errors: "helps get users go", "a resource which covers both topic". Anything a read-aloud would catch. |

**Why it matters:** Editorial pitches are evaluated partly as a writing sample. A pitch with proofreading failures signals the writer would submit copy with the same issues.

**Test:** Read the pitch aloud at normal speaking pace. Mark any phrase that requires a second pass to parse.

---

## Applying the rubric

### As a pre-send checklist

Run each criterion against the draft in order. A single Fail on Criterion 1 or 2 usually requires a full opener rewrite before proceeding to the others — there is no point polishing a pitch that loses the editor in the first sentence.

### Scoring

The rubric is pass/fail per criterion, not scored. A pitch with five passes and one fail on the close (Criterion 4) is not a 5/6 — it is a pitch that will be ignored because it ends badly. All six criteria must pass.

### On AI-generated output specifically

LLMs will reliably fail Criteria 2, 4, and 5 without explicit instruction. The default generation pattern is hedged (Criterion 2), deferential at the close (Criterion 4), and prone to register drift into business-speak (Criterion 5). Criterion 3 failures appear when the model is uncertain about the pitch's strength and compensates by adding justification. Criteria 1 and 6 require factual grounding and mechanical review that generation alone cannot guarantee.

The link-pitch skill enforces this rubric at Step 7 before surfacing the external pitch. If you are using this rubric independently, apply it after generation and before any human review step.

---

## Extending this rubric

This rubric covers the failure modes most common in AI-generated editorial outreach. It does not cover:

- **Audience fit** — whether the pitch is targeting the right publication for the client's ICP (evaluated at Step 3 of the skill)
- **Factual accuracy** — whether claims about the client or the article are accurate (enforced by the skill's "never fabricate" rule; requires human verification)
- **Option quality** — whether the proposed link placements are genuinely useful to the article's readers (evaluated in the internal proposal at Step 6)

Those dimensions have their own evaluation logic. This rubric evaluates the pitch's writing quality only.

Contributions, extensions, and alternative rubric designs for different outreach contexts are welcome via [Issues](../../issues).
