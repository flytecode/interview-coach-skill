# summarize — Coaching State Snapshot

### Purpose

Produce a concise, scannable dashboard of the candidate's current coaching state. This is a **read-only snapshot** — no drills, no analysis, no interactive questions. It's the answer to "where am I right now?" in under two minutes of reading.

Use this when:
- The candidate returns after a gap and needs to reorient
- The candidate wants a quick status check mid-engagement
- The coach needs to confirm state before recommending a next step

**Not a substitute for `progress`** — `progress` is a deep analytical review with calibration, calibration drift detection, self-assessment comparison, and drill recommendations. `summarize` is the dashboard; `progress` is the diagnostic.

### Logic

1. Read `coaching_state.md`. If it doesn't exist, say: "No coaching state found. Run `kickoff` to get started, or `help` to see all available commands."
2. Output the coaching state snapshot using the Output Schema below.
3. No interactive questions during `summarize`. Just read and report.
4. Adapt section depth to available data — omit sections with no data rather than printing empty tables.
5. Close with the single highest-leverage next action based on current state (same recommendation logic as Session Start Protocol in SKILL.md).

### Data Sourcing Rules

- **Profile**: Read directly from Profile section
- **Score snapshot**: Compute from Score History — take averages of the 3 most recent scored sessions (practice or analyze/mock). If fewer than 3 sessions, use all available. State how many sessions are averaged.
- **Trend**: Compare the 3 most recent scores to the 3 before that. If fewer than 4 total sessions, state direction as "establishing baseline"
- **Storybank**: Count rows from the Storybank table. Count strength 4-5 as "strong"
- **Active pipeline**: Read from Interview Loops — show only Status: Decoded / Researched / Applied / Interviewing / Offer entries. Omit Closed/Archived.
- **Outcome log**: Count from Outcome Log. Show last 3 outcomes for recency.
- **Coaching strategy**: Read from Active Coaching Strategy
- **Materials**: Read from LinkedIn Analysis, Resume Optimization, Positioning Statement, Outreach Strategy — show overall rating and date
- **Comp strategy**: Read from Comp Strategy — show research completeness and date

### Output Schema

```markdown
## Coaching Snapshot — [Name]
_As of [date]. Last updated: [coaching_state.md Last updated date]._

---

### Profile
- **Target role**: [role(s)] at [seniority band]
- **Track**: [Quick Prep / Full System]
- **Timeline**: [interview timeline — or "ongoing"]
- **Mode**: [triage / focused / full]
- **Biggest concern**: [from profile]

---

### Score Snapshot
_Averaged across last [N] scored sessions ([date range])._

| Dimension | Current Avg | Trend |
|---|---|---|
| Substance | [avg] | [↑ / → / ↓ / establishing baseline] |
| Structure | [avg] | [↑ / → / ↓ / establishing baseline] |
| Relevance | [avg] | [↑ / → / ↓ / establishing baseline] |
| Credibility | [avg] | [↑ / → / ↓ / establishing baseline] |
| Differentiation | [avg] | [↑ / → / ↓ / establishing baseline] |

- **Current bottleneck**: [lowest scoring dimension, or primary bottleneck from Active Coaching Strategy]
- **Self-assessment tendency**: [over-rater / under-rater / well-calibrated / unknown]
- **Calibration status**: [from Calibration State — uncalibrated / calibrating / calibrated / miscalibrated]

_No scored sessions yet — run `practice` or paste a transcript with `analyze` to get your first data point._
[Omit score table and show this line if no Score History exists]

---

### Storybank
- **Total stories**: [count] (target: 8-12)
- **Strong (4-5)**: [count] — [healthy / needs more / critical gap]
- **Earned secrets**: [count of stories with real earned secrets] of [total]
- **Competency gaps**: [list gaps for target role — or "none identified" / "not assessed yet"]
- **Most used**: [S### — Title] ([Use Count] times)
- **Never used in real interview**: [count] stories

_No storybank yet. Run `stories add` to build your first story._
[Omit storybank section and show this line if Storybank table is empty]

---

### Active Pipeline
| Company | Status | Next Round | Fit Verdict |
|---|---|---|---|
| [company] | [status] | [date or format, or "—"] | [verdict] |
[One row per active Interview Loop. Omit Closed/Archived entries.]

_No active company loops. Run `research [company]` or `decode` to start building your pipeline._
[Omit table and show this line if no active loops]

---

### Outcomes
- **Real interviews completed**: [count]
- **Results**: [X advanced / Y rejected / Z pending]
- **Recent**: [last 3 outcome entries — Company, Round, Result]

_No interview outcomes logged yet._
[Omit section and show this line if Outcome Log is empty]

---

### Application Materials
| Material | Status | Last Updated |
|---|---|---|
| Resume | [Strong / Needs Work / Weak / Not assessed] | [date or "—"] |
| LinkedIn | [Strong / Needs Work / Weak / Not assessed] | [date or "—"] |
| Positioning Statement | [exists — core hook excerpt / Not created] | [date or "—"] |
| Outreach Strategy | [exists — channels covered / Not created] | [date or "—"] |
| Comp Strategy | [research completeness — thorough / partial / none / Not created] | [date or "—"] |

---

### Current Coaching Focus
- **Primary bottleneck**: [from Active Coaching Strategy]
- **Approach**: [from Active Coaching Strategy — current approach in 1 line]
- **Drill stage**: [1-8, from Drill Progression — or "not started"]
- **Pivot if**: [from Active Coaching Strategy]

---

**Recommended next**: `[command]` — [one-line reason based on coaching state]. **Alternatives**: `[command]`, `[command]`
```

### Adaptation Rules

- **No coaching state**: "No coaching state found. Run `kickoff` to get started, or `help` to see all available commands."
- **New candidate (kickoff run, 0-1 sessions)**: Show Profile and Storybank sections only; omit Score Snapshot trends, Outcomes, and Calibration Status. Recommend `stories` or `practice`.
- **Quick Prep track**: Omit Drill Progression detail. Omit storybank health detail beyond count.
- **Missing sections**: If a section like Comp Strategy or Outreach Strategy doesn't exist in the state file, show "Not created" in the materials table — don't omit the row.
- **Empty sections**: Don't show empty tables. Replace with the "not yet" line specified per section.
- **Trend arrows**: ↑ = avg improved by 0.3+ compared to prior period. ↓ = dropped by 0.3+. → = within 0.2 in either direction. "establishing baseline" = fewer than 4 total scored sessions.
- **Core hook excerpt**: For Positioning Statement, show the Hook (10s) field from coaching state, truncated to ~10 words if longer. Format: `"[truncated hook]..."`
