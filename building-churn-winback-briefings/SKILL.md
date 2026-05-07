---
name: building-churn-winback-briefings
description: Guides a customer retention, customer success, or CX manager from a churn dataset through identifying drivers, segmenting churners, recommending a priority target with an outreach approach, and producing a leadership briefing as styled HTML. Use when the user is investigating churn, attrition, or cancellations; when the dataset has a churn-outcome label (columns like churn, churned, is_churned, cancelled, attrited, lost) alongside customer attributes; or when they mention churn rate, retention, win-back, save offer, NPS, CSAT, attrition cohort, or a VP / leadership review on retention. The user is a non-technical business manager — translate analytical outputs into business language and lead with the recommendation.
---

# Building a Churn Win-Back Briefing

The user is a customer retention, customer success, or customer
experience manager. They have churn data and a leadership meeting
coming up. The deliverable is a polished briefing — clear enough
to present, specific enough that the leadership team can act on
it.

Their working vocabulary is business language. "Customers", not
"records". "Churned", not "positive class". "Drivers", "reasons",
"segments", "cohort", "score" are all natural CSM vocab and fine
to use. What they don't want is implementation jargon: SHAP,
attribution weights, cluster centroids, feature importance
scores, log-odds. Translate behind the scenes; describe behavior
and reasons, not math.

The briefing must answer four questions in order:

1. **Who left, and why?** — top reasons most associated with last
   period's churn at the population level.
2. **Are there distinct stories?** — distinct segments of churners
   with different reasons, not one cause.
3. **Where do we focus first?** — a recommended priority segment
   with reasoning, plus a specific outreach approach (offer,
   channel, message).
4. **One customer to call before launch** — a specific customer
   from the priority segment with a hypothesis the manager can
   gut-check on a phone call before mass outreach.

## Workflow

```
[ ] 1. Train the churn model on the labeled customer data
[ ] 2. Identify top churn drivers (the "why" at the population level)
[ ] 3. Segment the churners (find the distinct stories)
[ ] 4. Recommend a priority segment + outreach approach
[ ] 5. Pick one customer from that segment + hypothesis
[ ] 6. Build the leadership briefing as HTML
```

### 1. Train the churn model

The user provides a dataset with a churn-outcome label.

- Confirm the label column. Common names: `churn`, `churned`,
  `is_churned`, `cancelled`, `attrited`, `attrition`, `lost`,
  `target`. Ask if it's not obvious — guessing wrong produces
  a worthless model.
- Set expectations: "I'm building a churn model from your customer
  data — typically a few minutes. We'll use it to find the
  strongest reasons people are leaving and to surface distinct
  groups of churners."
- For files too large to paste inline, walk them through the
  upload flow without exposing tool names. Say "let me set up an
  upload link for that file" — never the underlying call name.

### 2. Identify top churn drivers

Run global analysis and report the top 5 drivers in plain English.
Each driver should be paired with a *contrast* — what the churners
look like vs. retained customers — to make it concrete:

> "Across the customers who left last period, the strongest signals
> were:
>
> 1. <plain-English driver, e.g., 'high international usage on the
>    standard plan — averaged ~280 minutes/month vs. ~40 for
>    retained customers'>
> 2. ...
>
> These are population-level patterns. The story gets actionable
> when we look at distinct sub-groups of churners — that's next."

If a top driver is something the business can't operate on
(customer ZIP code, raw account number, demographic baseline), pause.
The model may be fine, but the win-back strategy needs *operable*
drivers (usage patterns, plan choice, service contacts, billing
events). Discuss with the manager before continuing.

### 3. Segment the churners

Run segmentation on the churned cohort. The user wants distinct
*stories*, not generically labeled clusters. For each segment,
describe:

- **Size** (% of churned base, count)
- **What defines them** (the behavior that makes this group
  distinct, in plain language)
- **Likely reason they left** (driver-led narrative)

Use this shape in the in-chat analytical summary:

```
SEGMENT A (~28% of churners; ~N customers):
  Defining behavior: heavy international users on the standard
    plan, no international add-on
  Likely reason: international charges add up, customers feel
    nickel-and-dimed
SEGMENT B (~22% of churners; ~N customers):
  Defining behavior: 4+ customer service calls in the final 30
    days
  Likely reason: unresolved service issues, walked off frustrated
SEGMENT C (~18% of churners; ~N customers):
  ...
```

Don't surface "cluster IDs", "centroids", or numerical labels.
Give each segment a name and a story.

### 4. Recommend a priority segment + outreach approach

For each segment, weigh:

- **Size** — bigger segments mean bigger savings if the campaign
  works
- **Recoverability** — do the drivers suggest a fixable problem?
  A pricing complaint can be addressed with an offer; a feature
  gap may not be
- **Effort** — what does outreach actually look like (discount,
  service-recovery call, plan change, win-back email)

Pick **one** segment as the recommended starting point. Write the
recommendation in 2–3 sentences a VP can read in 20 seconds:

> "Start with Segment A. They're the largest recoverable group
> (~28% of churners) and the driver is fixable: a plan-mismatch
> on international usage. Offer: 6 months free on the international
> add-on, delivered via direct email + service text follow-up.
> Pilot on a third of the segment, measure save rate vs. control."

For the other segments, write a one-line "why not first" — signal
priority order, not an answer-or-nothing.

### 5. Pick one customer + hypothesis

From the priority segment, pick one specific customer. Show:

- **Profile** — the fields the manager would describe on a call
  (plan, tenure, monthly spend, key usage, service history)
- **Top 3 drivers for this customer** — per-customer breakdown,
  not the global ones from step 2
- **What would have changed it** — the smallest realistic change
  that would have kept this customer
- **Hypothesis** — a 1–2 sentence story the manager can test on
  the phone, ending with the specific question to ask

Example:

```
CUSTOMER: account_id 4837 (anonymize as needed)
Profile: Standard plan, 18-month tenure, ~$78/mo. ~280
  international minutes/month, no international add-on. 3
  customer service calls in the final 30 days.
Top drivers (in order of contribution to this customer's churn):
  1. International charges spiked $42 in the final month
  2. Three customer service calls — unusual for this customer
  3. On a plan tier shared with much-less-engaged customers
What would have changed it: An international add-on bundled at
  signup ($15/mo) would have held them. Removing the
  service-call signal alone would not have.
Hypothesis: "The cost shock from international usage is the
  primary reason; the service calls are a symptom — they likely
  called to discuss the bill, didn't get a save offer, and
  walked. On the call, ask whether they ever called about
  pricing or international fees."
```

This is the gut-check the manager will do before launching the
full campaign. It's load-bearing for the VP meeting — leadership
will ask: "have you talked to one of these customers?"

### 6. Build the leadership briefing

Once the user confirms the analytical summary, package everything
as a self-contained HTML document and save it to the working
directory. The briefing reads like an executive summary — the VP
should be able to skim it in 90 seconds and walk into the meeting
prepared.

Inline the CSS so the file works standalone with no external
dependencies. Use clean sans-serif and a single accent color —
this is a business document, not a regulatory filing.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Customer Win-Back Briefing</title>
<style>
  body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif; max-width: 760px; margin: 2rem auto; padding: 0 1.2rem; color: #1a1a1a; line-height: 1.55; }
  h1 { font-size: 1.5rem; margin-bottom: 0.3rem; }
  .subtitle { color: #666; font-size: 0.95rem; margin-bottom: 1.6rem; }
  h2 { font-size: 1rem; text-transform: uppercase; letter-spacing: 0.05em; color: #444; border-bottom: 1px solid #ddd; padding-bottom: 0.3rem; margin-top: 2rem; }
  h3 { font-size: 1.05rem; color: #2a2a2a; margin: 0.4rem 0; }
  .tldr { background: #eef5ff; border-left: 4px solid #2962b8; padding: 0.95rem 1.2rem; margin-bottom: 1.4rem; border-radius: 3px; }
  .tldr h2 { margin-top: 0; border: 0; padding: 0; color: #2962b8; }
  .driver-list { padding-left: 1.4rem; }
  .driver-list li { margin-bottom: 0.5rem; }
  .segment { border: 1px solid #ddd; padding: 0.85rem 1.1rem; margin-bottom: 0.7rem; border-radius: 4px; }
  .segment.priority { border-color: #2962b8; border-width: 2px; background: #f7faff; }
  .tag { display: inline-block; font-size: 0.74rem; text-transform: uppercase; letter-spacing: 0.05em; padding: 0.18rem 0.55rem; border-radius: 3px; font-weight: 600; }
  .tag.priority { background: #2962b8; color: white; }
  .tag.alt { background: #e6e6e6; color: #555; }
  .stats { color: #666; font-size: 0.9rem; margin-top: 0.3rem; }
  .customer-card { background: #fafafa; border: 1px solid #e0e0e0; padding: 1rem 1.2rem; border-radius: 4px; margin-top: 0.7rem; }
  .customer-card dl { margin: 0.6rem 0; display: grid; grid-template-columns: 140px 1fr; gap: 0.35rem 0.9rem; }
  .customer-card dt { font-weight: 600; color: #555; font-size: 0.9rem; }
  .customer-card dd { margin: 0; }
  .hypothesis { background: #fff8e6; border-left: 3px solid #f0a800; padding: 0.65rem 0.9rem; margin-top: 0.7rem; }
  .footer-note { margin-top: 2.5rem; font-size: 0.82rem; color: #777; border-top: 1px solid #eee; padding-top: 0.8rem; }
</style>
</head>
<body>

<h1>Customer Win-Back Briefing</h1>
<div class="subtitle">Period: <!-- e.g., April 2026 --> · Churn rate: <!-- X.X% --> · Cohort: <!-- N customers who left --></div>

<div class="tldr">
  <h2>Recommendation</h2>
  <p><!-- 2-3 sentences. Lead with the priority segment, the offer, the channel, and the expected impact. The VP should be able to act on this paragraph alone. --></p>
</div>

<h2>Top reasons customers left</h2>
<ol class="driver-list">
  <li><!-- driver 1 in plain English with a concrete contrast (churners vs. retained, with numbers) --></li>
  <li><!-- driver 2 --></li>
  <li><!-- driver 3 --></li>
</ol>

<h2>Distinct segments of churners</h2>

<div class="segment priority">
  <span class="tag priority">Priority target</span>
  <h3><!-- e.g., "Cost-shocked international users" --></h3>
  <div class="stats"><!-- ~X% of churners · ~N customers --></div>
  <p><strong>What defines them:</strong> <!-- behavior, plain English --></p>
  <p><strong>Likely reason:</strong> <!-- driver-led story --></p>
  <p><strong>Recommended outreach:</strong> <!-- offer + channel + message angle --></p>
</div>

<div class="segment">
  <span class="tag alt">Secondary</span>
  <h3><!-- segment name --></h3>
  <div class="stats"><!-- ~X% of churners · ~N customers --></div>
  <p><strong>What defines them:</strong> <!-- behavior --></p>
  <p><strong>Likely reason:</strong> <!-- story --></p>
  <p><strong>Why not first:</strong> <!-- reason — smaller, harder to recover, etc. --></p>
</div>

<!-- Add additional segments as needed; tag them "alt" -->

<h2>One customer to call before launch</h2>
<p>Recommended for a verification call before the campaign launches — gut-check the hypothesis on a real customer before mass outreach.</p>

<div class="customer-card">
  <dl>
    <dt>Customer</dt><dd><!-- ID or anonymized name --></dd>
    <dt>Profile</dt><dd><!-- plan, tenure, monthly spend, key usage --></dd>
    <dt>Top drivers</dt><dd><!-- 1. ..., 2. ..., 3. ... --></dd>
    <dt>What would have changed it</dt><dd><!-- counterfactual in plain English --></dd>
  </dl>
  <div class="hypothesis">
    <strong>Hypothesis:</strong> <!-- 1-2 sentences. End with the specific question to ask on the call. -->
  </div>
</div>

<p class="footer-note">Briefing prepared with analytical assistance. Drivers are <em>associated with</em> last period's churn — correlations observed in the data, not proven causes. Verify the single-customer hypothesis on the call before launching the campaign.</p>

</body>
</html>
```

Save to `winback_briefing_<YYYYMMDD>_<HHMM>.html` in the working
directory. Tell the user the path and suggest they open it in a
browser:

> "Saved the briefing to `winback_briefing_20260505_1623.html`.
> Open it in your browser to review before the meeting."

## What you must not do

- **Do not produce raw analytics in the briefing.** No model
  jargon, no SHAP, no cluster IDs, no probabilities-with-three-
  decimals. The audience is a VP, not a data team. Translate.
- **Do not invent customer details, segment sizes, or churn
  numbers.** If a field or count is missing, ask. Hypotheses are
  fine; manufactured facts undermine the whole briefing.
- **Do not recommend more than one priority segment to start.**
  The VP is choosing where to focus first; ranked options help,
  but lead with one and stick with it.
- **Do not skip the single-customer hypothesis.** It's the
  gut-check the manager needs before launching at scale, and
  it's what makes the briefing land. Leadership will ask: "have
  you talked to one of these customers?"
- **Do not confuse correlation with causation.** Drivers are
  *associated with* churn — write that way. "Customers who left
  tended to..." not "X causes churn".
- **Do not surface implementation jargon in the deliverable.**
  "Reasons", "drivers", "patterns", "segments", "score" — fine,
  natural business language. SHAP, attribution weights,
  log-odds, feature importance scores, cluster centroids —
  translate behind the scenes.
