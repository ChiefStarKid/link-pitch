# Execution detail — Steps 0–7

Full step-by-step logic for the `link-pitch` skill. SKILL.md owns the overview,
checkpoints, contact gate, and key rules; this file holds the verbatim agent
prompts and the per-step mechanics. Output-file structures live in
`references/templates.md`.

## Configuration

### Internal style
Read `styles/internal.md` (relative to the skill root). It governs Steps 3–6, all
file summaries, and working communications to the user. If the file is missing,
fall back to clear, direct prose.

### Outreach style
```
OUTREACH_STYLE_DEFAULT = "<skill-root>/styles/outreach.md"
```
Applied to the Step 7 external pitch only. Resolution:
- "Use bundled default" or blank → `OUTREACH_STYLE_DEFAULT`
- "No style — plain output" → `null` (generate confident plain prose)
- custom path → use the path the user provided

If the resolved file is not found, fall back to confident plain prose. Never let
the internal and outreach styles mix in the same output.

### Session state
Stored at `~/.claude/temp/link-pitch-session.json` (`~` = the user's home
directory on any OS). Used for resume across the four checkpoints.

---

## Step 0 — Resume Check

Before proceeding, check for the session state file. If found:
- Parse the saved state (inputs, current step, `contact_gate_override`, `contact_gate_findings`)
- Offer the user: "Resume previous session? [Yes / Start fresh]"
- If Yes:
  - Jump to the saved step with saved inputs
  - If `current_step` is `"2.5"`: re-display the contact gate warning from saved `contact_gate_findings` data and ask for Abort/Override again — do not re-run research
  - If `contact_gate_override` is `true`: load the override caveat into working memory so it propagates to Step 6 and Step 7 output files. Do not prompt the user about it again.
- If Start fresh: delete the state file and proceed to Step 1

## Step 1 — Gather Inputs

### Step 1a — Core inputs

Use `AskUserQuestion` with these fields. All required unless marked optional:

1. **Client website URL** (required) — "The website of the client/company you want to link to"
2. **Linked page URL** (required) — "The specific page on the client's site you want the link to point to (e.g., a resource, guide, or concept page)"
3. **ICP definition** (optional) — "Who is the client trying to reach? E.g., 'B2B SaaS marketers, 5-50 person teams' (leave blank if unsure)"
4. **Anchor text** (optional) — "Preferred keyword for the link text (leave blank for the skill to suggest)"
5. **Output folder** (required) — "Where should output files be saved?" Options: "Current working directory" (default) or user types custom path via Other
6. **Outreach style** (optional) — "Style for the external pitch. Defaults to the bundled outreach voice. Replace with a different .md file to change it." Options: "Use bundled default", "No style — plain output", or user types custom path via Other
7. **Target article** (required) — "Do you have the URL of the article where you want a link inserted, or should the skill find options?"
   - Option 1: "Find recommendations for me" — search for and pre-screen candidate articles
   - Option 2: "I have a URL" — user types the URL via Other

After the user submits, validate URLs are well-formed. If any required field is missing, re-ask.

### Step 1b — Branch on article input

**If user typed a URL via Other:** validate it, save `article_url`, proceed to Step 2.
**If user selected "Find recommendations for me":** proceed to Step 1c.

### Step 1c — Article Discovery Agent

Spawn a research agent with the following task:

```
You are researching candidate articles for a link insertion campaign.

Inputs:
- Linked page URL (client's page that will receive the backlink): {linked_page_url}
- ICP definition: {icp_definition or "not provided"}

PHASE 1 — Understand the linked page
Fetch {linked_page_url}. Extract:
- Core topic and title
- Key concepts and sub-topics covered
- Target audience signals (from copy, CTAs, framing)
- The page's core value proposition (what problem does it solve?)
If the page is uncrawlable, infer topic from the URL slug and ICP instead.

PHASE 2 — Search for candidate articles
Run 2–3 WebSearch queries with variation:
- Query 1: [core topic] guide [ICP role keyword] — broad discovery
- Query 2: [specific concept from linked page] [vertical keyword] — narrow, topic-adjacent
- Query 3: best [topic] articles [current year] — freshness filter
Collect 8–10 distinct article URLs from results. Exclude the client's own domain.

PHASE 3 — Pre-screen each candidate
For each candidate, fetch the article and assess against these checks:
| Check | Pass criteria |
|---|---|
| Accessible | Not paywalled, not 404 |
| Audience match | Targets roles/verticals that overlap with ICP |
| Topic adjacency | Covers a subject where the linked page adds value |
| Insertion seam | Has at least one section with a visible gap the linked page fills |
| Editorial quality | Not thin content, not spam, not competitor-promotional |
Score each: Strong / Moderate / Weak / Disqualified
Disqualify immediately: paywalled, 404, zero audience overlap, no insertion seam.

PHASE 4 — Return top 3
Return the 3 best Strong/Moderate candidates. For each, provide:
title, url, site, fit_score (Strong|Moderate), audience_match,
insertion_seam, gap_identified, red_flags (or "none").
```

Once the agent returns, present 3 options in `AskUserQuestion` (single-select):
- **label**: short article title (≤5 words)
- **description**: site name + fit score + one-line insertion seam
- **preview** (markdown):
```
[Full Article Title](https://full-url-here.com)

**Fit:** Strong | **Site:** example.com

**Where the link goes:** [insertion_seam]

**Gap filled:** [gap_identified]

**Audience:** [audience_match]

**Red flags:** [red_flags, or "None"]
```
User picks one — or types their own URL via "Other" (skip pre-screening, go to Step 2).

Once `article_url` is resolved, write the session state file:
```json
{
  "client_url": "<url>",
  "article_url": "<url>",
  "linked_page_url": "<url>",
  "icp_definition": "<text or null>",
  "anchor_text_provided": "<text or null>",
  "output_folder": "<path>",
  "session_slug": "<domain>-<date>",
  "outreach_style_path": "<resolved path or null>",
  "current_step": 2,
  "created_at": "<timestamp>",
  "contact_gate_override": false,
  "contact_gate_findings": null
}
```
Outreach style resolution: "Use bundled default" or blank → `OUTREACH_STYLE_DEFAULT`;
"No style" → `null`; custom path → use provided path.

## Step 2 — Research (Parallel Agents)

Spawn two agents in parallel.

### Agent 1: Company Researcher
```
Fetch and analyze the client website: {client_url}
Extract and report:
1. Client self-view: what they claim to do (mission, products/services, value props);
   tone and voice; target audience hints.
2. ICP profile: who they're trying to reach (role/title, industry, company size,
   pain points, goals). If the user provided an ICP, cross-reference where it matches
   or diverges and flag contradictions or gaps.
3. Key offerings to highlight: product/service names, differentiators, visible
   case studies or metrics.
Be factual. Only report what's actually on the site. If sparse, flag that.
```

### Agent 2: Article Analyzer
```
Fetch and analyze the article: {article_url}
Extract and report:
1. Insertion landscape: natural seams where new content fits; sparse sections;
   2-3 specific insertion points.
2. Blue ocean opportunities: concepts/questions/keywords the article touches but
   leaves incomplete; what a reader looks for "next"; 3-5 underserved gaps.
3. Article audience profile: who reads this (role, seniority, intent, industry);
   language register; what they find credible; what would feel like ad copy.
4. Publisher fit: editorial stance; what they link to; any "write for us" guidelines.
5. Contact information audit — report each criterion explicitly as FOUND or NOT FOUND:
   - Email address (editor/contributor/contact email; search byline, bio, footer, /contact, /about)
   - Contact form URL (working submission/contact form on the publisher domain)
   - Contributor/write-for-us page (/write-for-us, /contribute, /guest-post)
   - Social profile (named editor/author LinkedIn or X/Twitter from article or bio)
   For each, output exactly `[FOUND]` with the value, or `[NOT FOUND]` with a one-line
   note on where you looked.
Report only what's visible on the site and in the article itself.
```

After both complete, proceed to Step 2.5.

## Step 2.5 — Contact Information Gate

Parse Agent 2's audit and record each criterion. Store under `contact_gate_findings`:
```json
{
  "email": { "found": true/false, "value": "<email or null>" },
  "contact_form": { "found": true/false, "value": "<url or null>" },
  "contributor_page": { "found": true/false, "value": "<url or null>" },
  "social_profile": { "found": true/false, "value": "<url or null>" }
}
```

**IF any one criterion is FOUND:** leave `contact_gate_override` as `false`, proceed
to Step 3 silently — no checkpoint, no prompt.

**IF ALL FOUR are NOT FOUND:** display the warning (internal style — direct):
```
⚠️ No publisher contact information found.

| Criterion | Status | Where searched |
|---|---|---|
| Email address | Not found | Article byline, author bio, /contact, /about, footer |
| Contact form URL | Not found | [pages searched] |
| Contributor/write-for-us page | Not found | /write-for-us, /contribute, /guest-post, site nav |
| Social profile (named editor/author) | Not found | Article byline, author bio |

Without a contact route, this pitch has no delivery path.

Choose:
A — Abort this session (optionally clear saved state)
B — Override: proceed anyway (a caveat will appear in all output files)
```

**If Abort (A):** ask "Clear saved session state? [Yes / No]". If Yes, delete the
session state file. If No, set `current_step` to `"2.5"` and leave the file.
End: "Session ended. No output files were created."

**If Override (B):** set `contact_gate_override` to `true`. Confirm: "Override logged.
A caveat will appear in the internal proposal and external pitch." Proceed to Step 3.

## Step 3 — Audience Fit Assessment

Cross-reference Client ICP (Agent 1) vs Article audience (Agent 2). Score the fit:
- **Strong** — well-aligned; placement feels natural
- **Moderate** — some overlap; may need reframing
- **Weak** — minimal alignment; placement would feel forced

Surface the score with a one-line explanation. Then a best-effort placement value
estimate:
| Metric | Value |
|---|---|
| Domain Rating | [from research or "not available"] |
| Monthly traffic | [estimate] |
| Vertical | [category] |
| Comparable paid link (same DR/vertical) | $[range] |
| Estimated organic editorial equivalent | $[range] |

Include: "Cost of organic route: $0 if pitch succeeds — ROI is [range] if accepted."

Ask: "Proceed with this article, or choose a different one?" If weak fit and the user
wants to proceed, flag the lower acceptance likelihood and confirm.

## Step 4 — Anchor Text Selection

If the user provided anchor text, use it. Otherwise propose 2–3 SEO-optimized options
based on the blue ocean gaps (Agent 2), the client's offerings (Agent 1), and what the
article audience finds credible. For each: the phrase, why it fits naturally (with
context), and why it's a plausible target keyword. Ask the user to approve, amend, or
provide their own, then store it.

## Step 5 — Generate All Options (Internal)

Generate 9 variations across 3 option types. Quality bar: each variation must
independently deliver value to either the reader (ICP) or the publisher (audience +
editorial quality) — ideally both.

**Option A — Simple Link Placement (3 variations):** for each, identify a specific
sentence and show **Original** → **Modified** with the link inserted; mark anchor +
URL; one-line insertion-strategy note. Vary: existing paragraph / different paragraph /
transitional phrase.

**Option B — Comprehensive Edit (3 variations):** for each, write a new sentence or
2-3 sentence paragraph with an exact insertion point; must add genuine value; tone
matches the article. Vary: framework insight / common mistake / deeper-dive resource.

**Option C — New Content / Guest Post (3 variations):** for each, propose a new article
or section — proposed title, angle (gap it fills), 2-3 sentence structure, where the
link fits naturally. Vary: complementary how-to / framework guide / industry trend or
case study.

Save all 9 to `LinkPitch-options-<slug>.md` (see `references/templates.md`). When these
are copied into the internal proposal (Section 5), copy the **full text of every
variation inline** — do not reference the options file.

## Step 6 — Internal Proposal

Synthesise research + options into `LinkPitch-internal-<slug>.md` (see
`references/templates.md`). If `contact_gate_override` is `true`, insert the Contact
Route Caveat block below the metadata header; otherwise omit it. Surface to the user
and ask: "Ready for Step 7 (external proposal)?"

## Step 7 — External Proposal

Ask the user to curate (AskUserQuestion). Mark whichever option type was recommended
with "(Recommended)":
1. **Which option types to include?** (multiSelect) — Option A / B / C
2. **How many variations per option?** — "1 (strongest only) (Recommended)" / "2" / "3"
3. **Format preference?** — "Markdown (default)" / "Plain text (for email paste)"

Load the outreach style from `outreach_style_path` in session state:
- not null → read the file and apply its rules (tone, hook, close, register, length)
- null or not found → confident plain prose

Do not mention the style file to the user or reference it in the output.

Generate `LinkPitch-external-<slug>.md` (or `.txt`) per `references/templates.md`. If
`contact_gate_override` is `true`, insert the Note block after the **Re:** line.

Surface the file path, then delete the session state file
(`~/.claude/temp/link-pitch-session.json`).

---

## Error Handling

- **Paywalled article** → "I can't access the full content. Paste the article text and I'll analyze it."
- **Uncrawlable client site (JS-heavy, blocked, 403/404)** → "I can't crawl the site. Give me a brief description and I'll proceed."
- **Empty anchor text, no options resonate** → "Provide your own anchor text, or describe the core concept the linked page covers."
- **Step 1c finds no viable candidates** → "I couldn't find strong candidates. Paste a target article URL directly."
- **Fewer than 3 candidates** → present what was found (min 2); user can select or type their own.
- **All candidates Moderate (none Strong)** → present them but flag that stronger framing may be needed.
- **Weak audience fit** → surface at Step 3, ask "Proceed anyway?"; if yes, add "⚠️ Weak audience alignment — lower acceptance likelihood" to the internal proposal.
