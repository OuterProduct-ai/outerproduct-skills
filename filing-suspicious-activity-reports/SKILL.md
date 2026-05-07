---
name: filing-suspicious-activity-reports
description: Guides a fraud or BSA/AML analyst from labeled case data through investigating a specific subject to drafting a Suspicious Activity Report (SAR) narrative. Use when the user is investigating possible fraud, money laundering, structuring, account takeover, synthetic identity, or other financial-crime typologies; when their dataset includes a fraud-outcome label (columns like is_fraud, sar_filed, chargeback, confirmed_fraud, aml_flag) alongside transaction or account behavior; or when the user mentions SAR, FinCEN, BSA, AML, OFAC, suspicious activity, fraud investigation, compliance review, or filing a report. The user is a non-technical risk analyst — lead with subject behavior, not raw ML output.
---

# Filing Suspicious Activity Reports

The user is a fraud or BSA/AML analyst. They have labeled
historical case data and a specific subject they want evaluated.
The deliverable, when asked for, is a draft SAR narrative — the
prose section a human investigator would write to support filing.

Their working vocabulary is investigator language — "subject",
"alert", "red flag", "typology", "case", "baseline". Risk
analysts also routinely use "model", "score", and "risk factor"
in their plain-English sense; those words are fine. What they
don't want is opaque ML output: SHAP values, feature attribution
weights, raw probabilities, confidence intervals. Lead with what
the subject did, then the assessment, then the drivers — not the
other way around.

The output is a regulatory artifact. An assessment without
explanations is useless here — every claim must be traceable to a
specific behavior, defensible to a bank examiner.

## Workflow

```
[ ] 1. Train the detector from the analyst's labeled case data
[ ] 2. Calibrate — which behaviors distinguish fraud from legit
[ ] 3. Evaluate the subject in question
[ ] 4. Present findings to the analyst (analytical summary)
[ ] 5. On request only — draft the SAR narrative
```

### 1. Train the detector

The analyst provides historical cases with a fraud-outcome label
(confirmed fraud, chargeback, prior SAR filed, etc.).

- Confirm the label column. Common names: `is_fraud`,
  `sar_filed`, `chargeback`, `confirmed_fraud`, `label`. Ask if
  not obvious. Don't guess — the wrong label produces a
  worthless detector.
- Set expectations: "I'm building a detector from your
  historical cases. This typically takes a few minutes."
- For data too large to paste inline, walk them through the
  upload flow without naming tools. Say "let me set up an upload
  link for that file" — never the underlying call name.

### 2. Calibrate

Before evaluating any individual subject, surface the global
patterns the detector learned. Report as:

> "Across your historical cases, the strongest fraud indicators
> were:
> 1. <plain-English description, e.g., 'wire amount relative to
>    the customer's 90-day average'>
> 2. ...
>
> Cases that get flagged generally deviate on these dimensions."

This calibrates the analyst and lets them sanity-check the
detector. If a top indicator is irrelevant or demographic
(customer ZIP, age, surname), pause — the training data likely
leaks or has a quality problem. Discuss before proceeding.

### 3. Evaluate the subject

When the analyst describes a specific subject:

- Build the subject's profile using the same fields the detector
  was trained on, in the same units. If any value is missing,
  ask. Do not default, do not impute, do not approximate.
- Pull two kinds of evidence, not one:
  - **Per-indicator contributions** — what about this subject's
    specific values pushed the assessment (e.g., "wire amount
    contributed heavily because it was 6× their baseline").
    These become the **red flags**.
  - **Pattern matches** — recurring combinations the detector
    recognized from prior cases (e.g., "same-day account
    opening + first transaction over $10K + foreign-IP login,
    seen in 78% of confirmed fraud cases"). These become the
    **patterns this subject matches** — strong evidence
    because they cite historical precedent, not just this
    subject's numbers.
  A bare risk score is not SAR-defensible. Both kinds of
  evidence are the audit trail.
- Optionally ask: what is the smallest realistic change to the
  subject's behavior that would have cleared the flag? This
  sharpens the "why is this case different from legitimate
  activity" framing the narrative will need.

### 4. Present findings (analytical summary)

Give the analyst this shape — before drafting any narrative:

```
SUBJECT: <identifier as supplied>
ASSESSMENT: <high / medium / low> risk; <N>% match to historical
  fraud cases in your data

TOP RED FLAGS (specific behaviors driving this assessment):
  1. <indicator> = <value>, vs. baseline <baseline> →
     <one-line plain-English description>
     Example: "Wire amount $48,500 — 6× this customer's 90-day
     average transaction"
  2. ...

PATTERNS THIS SUBJECT MATCHES (combinations the detector
  recognized from prior cases):
  - <plain-English pattern with historical precedent>
    Example: "Same-day account opening + first transaction
    over $10,000 + foreign-IP login — appeared in 78% of
    confirmed fraud cases in your data"
  - ...

WHAT WOULD HAVE CLEARED THE FLAG (if asked):
  <one-sentence summary>
```

Then stop and ask:

> "Want me to draft a SAR narrative based on this?"

The analyst decides whether the case warrants filing. The
detector is one input to that judgment, not the judgment.

### 5. Draft the SAR narrative (only on request)

If the analyst confirms, produce the prose narrative as a
self-contained HTML document and save it to the working
directory. The analyst opens it in a browser to review, print,
or paste into the bank's filing system. Do not attempt the
structured form fields (subject IDs, account numbers, filer
info) — those come from the bank's filing software.

Use the template below. The styling is intentionally plain — it
must read like a regulatory document, not a marketing page.
Inline the CSS so the file works standalone with no external
dependencies. Use the 5W+H structure for the facts block, and
fold both the **red flags** and the **patterns this subject
matches** into the Basis for Reporting list — restated as
behaviors with concrete numbers, never as model outputs.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Suspicious Activity Narrative — DRAFT</title>
<style>
  body { font-family: Georgia, "Times New Roman", serif; max-width: 720px; margin: 2rem auto; padding: 0 1rem; color: #1a1a1a; line-height: 1.55; }
  h1 { font-size: 1.4rem; border-bottom: 2px solid #1a1a1a; padding-bottom: 0.4rem; margin-bottom: 1rem; }
  h2 { font-size: 1rem; text-transform: uppercase; letter-spacing: 0.06em; color: #444; margin-top: 1.8rem; }
  dl.facts { display: grid; grid-template-columns: 110px 1fr; gap: 0.4rem 1rem; margin: 1rem 0 1.5rem; }
  dl.facts dt { font-weight: bold; color: #555; }
  dl.facts dd { margin: 0; }
  ul.flags { padding-left: 1.2rem; }
  ul.flags li { margin-bottom: 0.5rem; }
  .draft { background: #fff8e1; border-left: 4px solid #f0a800; padding: 0.7rem 1rem; margin-bottom: 1.5rem; font-size: 0.92rem; }
  .disclaimer { margin-top: 2rem; font-size: 0.85rem; color: #555; border-top: 1px solid #ccc; padding-top: 1rem; font-style: italic; }
</style>
</head>
<body>

<div class="draft"><strong>DRAFT</strong> — review and verify before filing.</div>

<h1>Suspicious Activity Narrative</h1>

<dl class="facts">
  <dt>Who</dt>      <dd><!-- subject identifier(s) as supplied --></dd>
  <dt>What</dt>     <dd><!-- typology: structuring / account takeover / synthetic ID / etc. --></dd>
  <dt>When</dt>     <dd><!-- date range --></dd>
  <dt>Where</dt>    <dd><!-- channels / locations --></dd>
  <dt>How</dt>      <dd><!-- mechanism --></dd>
  <dt>How much</dt> <dd><!-- aggregate dollar amount --></dd>
</dl>

<h2>Narrative</h2>
<p><!-- Lead: what the bank observed; alert, customer behavior, timeline --></p>
<p><!-- Specific behaviors in plain English; quote dollars and dates exactly. Never "the model said". --></p>
<p><!-- Close with the basis for treating this as a reportable event --></p>

<h2>Basis for Reporting</h2>
<ul class="flags">
  <li><!-- each top red flag (driver), restated as concrete behavior with numbers --></li>
  <li><!-- each pattern this subject matches, restated as a behavior with historical precedent ("seen in 78% of confirmed fraud cases in this institution's data") --></li>
</ul>

<p class="disclaimer">DRAFT prepared with analytical assistance. Review for accuracy, verify all transaction details against bank records, and consult your BSA officer before filing. This is not legal advice and does not constitute a determination that the activity is unlawful.</p>

</body>
</html>
```

Save to `sar_draft_<YYYYMMDD>_<HHMM>.html` in the current
working directory. Then tell the analyst the path and suggest
they open it in a browser:

> "Saved a draft narrative to `sar_draft_20260505_1432.html`.
> Open it in your browser to review."

## What you must not do

- **Do not file.** You produce a draft. The analyst and the BSA
  officer file.
- **Do not invent values.** Missing a transaction amount, date,
  counterparty, or account number? Ask. Never default, never
  round, never approximate.
- **Do not soften the red flags.** If the wire was $48,500 to
  Country X, write "$48,500 wire to Country X" — not "an
  unusually large international transfer".
- **Do not present the risk assessment without the red flags.**
  A score-only finding will not survive an exam.
- **Do not advise on whether to file.** That decision belongs to
  the BSA officer.
- **Do not surface implementation jargon.** "Risk score",
  "model", "risk factor" are fine — analysts say these every
  day. What's not fine: SHAP values, attribution weights,
  log-odds, confidence intervals, probability thresholds, AUC.
  Translate behind the scenes; describe behavior, not math.
- **Do not lead with the assessment number.** Lead with the red
  flags and the behavior driving them. The number is
  corroboration, not the headline.
