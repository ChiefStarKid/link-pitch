# Output file templates

Three markdown files are produced, named by date and client domain (`<slug>` =
`<client-domain>-<date>`). Save them to the output folder chosen at Step 1.

---

## 1. Options file — `LinkPitch-options-<slug>.md`

All 9 variations, for reference.

```markdown
# Link Insertion Options — [Client] → [Article]

Generated: [date]
Client: [domain]
Article: [title]
Anchor text: [final choice]

---

## Option A — Simple Link Placement

### Variation A1
[original → modified with explanation]

### Variation A2
[original → modified with explanation]

### Variation A3
[original → modified with explanation]

## Option B — Comprehensive Edit

### Variation B1
[new content + insertion point + explanation]

### Variation B2
...

### Variation B3
...

## Option C — New Content / Guest Post

### Variation C1
[proposed title + angle + structure + link placement]

...
```

---

## 2. Internal proposal — `LinkPitch-internal-<slug>.md`

Full analysis + recommendation + fallback design. Insert the **Contact Route
Caveat** block (below the metadata header) only if `contact_gate_override` is
`true`; otherwise omit it entirely.

```markdown
# Internal Proposal — [Client] → [Article]

**Generated:** [date]
**Client:** [domain]
**Article:** [title]
**Publisher:** [publication name or domain]

---

<!-- INSERT ONLY IF contact_gate_override is true -->
> ⚠️ **Contact Route Caveat**
> No publisher contact information was found during research (no email, contact form, contributor page, or named editor social profile). This session was continued by user override. Outreach delivery path is unknown — manual research required before sending.
<!-- END CONDITIONAL -->

## 1. Value Proposition

What does this placement deliver to the Target Content's readers and editorial quality?

**To readers:** [how does the linked page help them solve their problem or understand the topic better?]
**To publisher:** [why would the publisher want this link? Enhances credibility? Provides resource? Fills a gap?]

---

## 2. Recommended Option

**Strongest choice: [A / B / C]**

**Why:** [2-3 sentences explaining the recommendation based on:
- Audience fit (client ICP vs article audience)
- Blue ocean opportunity (does it fill an identified gap?)
- Publisher fit (does the site link to similar resources?)
- Risk/acceptance likelihood]

**Fallback options:** [Alternative options if journalist rejects primary recommendation]

---

## 3. ⚠️ Publisher Acceptance Risk + Placement Value

**Publisher profile:**
- [Guest post policy, editorial standards, link acceptance likelihood]

**Estimated placement value:**

| Metric | Value |
|---|---|
| Domain Rating | [from research] |
| Monthly traffic | [estimate] |
| Vertical | [category] |
| Comparable paid sponsored link | $[range] |
| Estimated organic editorial equivalent | $[range] |

**Acceptance scenarios** (in order of likelihood):
- [Contact method 1]: [likelihood %], [pitch angle]
- [Contact method 2]: [likelihood %]
- [Contact method 3]: [likelihood %]

<!-- If contact_gate_override is true: replace the rows above with: "⚠️ No contact route identified — all acceptance likelihood estimates are not applicable until a delivery path is found." -->

---

## 4. Supporting Analysis

**Audience alignment:**
- Client ICP: [from Company Researcher]
- Article audience: [from Article Analyzer]
- Overlap: [specific dimensions where they align]

**Blue ocean gaps identified:**
- [gap 1]
- [gap 2]
- [gap 3]

**Publisher editorial profile:**
- [editorial standards, link types they accept, tone, etc. from Article Analyzer]

**Contact method:**
- Email: [value or "Not found"]
- Contact form: [value or "Not found"]
- Contributor/write-for-us page: [value or "Not found"]
- Named editor social profile: [value or "Not found"]

---

## 5. All Options + Variations

[Copy the full text of all 9 variations from the generation step here, inline and
readable — do not reference or link to the options file]

---

## 6. Fallback Design

**If this exact content is rejected:**

The framework and analysis above should be compelling on its own. You can use this document to:
- **Propose different options** from the 9 variations shown above
- **Pitch entirely new angles** using the blue ocean gaps as inspiration
- **Adjust for feedback** — if the publisher says "too promotional" or "doesn't fit", reference the Supporting Analysis to propose refinements

This is why we generated 3 variations per option type — each one a distinct entry point for negotiation.
```

---

## 3. External pitch — `LinkPitch-external-<slug>.md` (or `.txt` if plain text chosen)

The final publisher-facing pitch, in the **outreach voice**. Insert the Note block
(immediately after the **Re:** line) only if `contact_gate_override` is `true`.

```markdown
# Proposal: Link Insertion — [Client] → [Article]

**To:** [contact name/email if found — if contact_gate_override is true and no contact was found, use "Editor, [Publication Name] (unverified — no contact route found)"]
**Re:** Link suggestion for "[Article Title]"

<!-- INSERT ONLY IF contact_gate_override is true -->
> ⚠️ **Note:** No verified contact route was identified for this publisher during research. This pitch was generated under user override. Confirm delivery address before sending.
<!-- END CONDITIONAL -->

---

## Summary

I've identified a natural opportunity to strengthen your article "[Article Title]" by linking to a resource that fills an existing gap and genuinely serves your readers.

[1-2 sentence value pitch: what problem does the linked page solve? Why would your readers benefit?]

---

## The Suggestion

[Include selected variations here, formatted clearly]

### [Option type] — Variation [N]
[Exact edit or proposal]

---

## Why This Works

**For your readers:** [how does the link serve them? What question does it answer?]
**For your publication:** [why is this link editorially sound? What does it add?]

---

## Next Steps

I'm happy to refine this suggestion based on your feedback. Some alternatives are shown above if you'd prefer a different approach.

---

[Optional: contact info, your name/company]
```
