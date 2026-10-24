<div align="center">

# 🔎 Proposal Lens

### The quoted room rate is half the story. Proposal Lens shows the other half.

An AI hotel-proposal analyst for event planners. It reads every hotel proposal in whatever format it arrives, normalizes them to one **true total cost**, scores each contract clause against market benchmarks, stress-tests what happens if attendance drops, and drafts the counter-proposal.

[**▶ Live prototype**](https://aayushm1.github.io/proposal-lens/) · [How it works (plain English)](HOW-IT-WORKS.md)

![Domain](https://img.shields.io/badge/domain-Event%20Tech%20%C2%B7%20Hospitality%20Sourcing-0A6CFF)
![Role](https://img.shields.io/badge/role-Product%20Management-00A99D)
![Stack](https://img.shields.io/badge/stack-Vanilla%20JS%20%C2%B7%20single%20file-0B1F3A)
![Data](https://img.shields.io/badge/data-100%25%20fictional-12A150)

</div>

> Independent product concept, built as a PM exercise for the event-management space (Cvent-style platforms). Not affiliated with or endorsed by Cvent. **Northwind Technologies**, all five hotels, contacts, contract text and benchmarks are fictional.

---

## TL;DR

| | |
|---|---|
| **Problem** | Planners get 5–20 hotel proposals per program, each in a different format (PDF, Word, email, platform response). Shortlists are built on the **quoted room rate**, but fees, F&B minimums, AV exclusivity and penalty clauses decide the real price, and they're buried on page 11. |
| **Insight** | Platforms already made it fast for hotels to *respond* to an RFP. The planner-side *comparison* is still a spreadsheet, and that's where money and risk leak. |
| **Solution** | A 5-step flow: **Inbox** → **True cost** (normalized TCO + rank flip) → **Contract risk** (clause-level flags, benchmarks, redlines) → **Stress test** (attendance-drop and cancellation exposure) → **Recommend** (weighted scoring + negotiation asks + drafted counter-proposal). |
| **Demo result** | 5 proposals, **$273K** of cost not visible in quoted rates, **22 issues flagged**. The "2nd-cheapest" hotel is actually the most expensive, **$142K** more than the recommended pick. |
| **North-star metric** | Hidden cost surfaced per RFP (with time-to-decision as the speed metric) |

## 1. Problem

**Primary user:** Priya, Sr. Event Manager at a 3,000-person company, sourcing a 450-person sales kickoff (990 room nights). **Approvers:** Procurement and Finance.

| Pain | What it looks like |
|---|---|
| 🧾 No common basis | Five formats, five ways of quoting. Comparing them is a 2–3 day spreadsheet job. |
| 🙈 Costs hide outside the rate | Destination/resort fees, F&B minimums above real spend, meeting-room rental with unreachable waivers, exclusive in-house AV. |
| ⚖️ Risk hides in clauses | Night-by-night attrition, 100% damages, no resale credit, harsh cancellation scales, rate-escalation deadlines. |
| 📉 Downside is invisible | Nobody models what a 20% headcount cut costs under each contract until it happens. |

> **How might we** help planners pick and negotiate the right hotel on true cost and true risk, in an hour instead of three days?

## 2. Approach

**Options considered** (from a review of the event-tech market and recent platform launches):

| Option | Verdict |
|---|---|
| Room-block attrition radar | Strong, but closer to existing room-block tooling roadmaps |
| Live event-day control room | Very demoable, but heavier to make credible with fake data |
| CFO "budget defense" brief | Overlaps with BI/reporting assistants already shipping |
| **Proposal comparison + contract risk** ✅ | Clear gap on the planner side, measurable in dollars, uses benchmark data no generic LLM has |

**Design principles**
1. **Lead with the surprise.** The rank flip (quoted rate vs. true cost) is the first thing you see after analysis.
2. **Cite the source.** Every flag shows the contract excerpt with the risky words highlighted.
3. **Benchmark, don't opine.** Each term is compared with the market norm, not with an AI's opinion.
4. **Price the downside.** Risk is expressed in dollars at a given attendance drop, not as a vague score.
5. **Human decides.** Weights are the planner's call. "CFO mode" and "Attendee-first" give different winners, on purpose.
6. **Numbers are never generated.** A deterministic model calculates every dollar. In production, an LLM would extract terms and write the narrative only.

## 3. Try it in 90 seconds

1. Click **✦ Normalize & analyze**. Watch it read five files and flag the hidden fee on page 11.
2. **True cost:** Meridian River North drops from #2 to #5. Lakeshore Grand rises from #4 to #2.
3. **Contract risk:** open Meridian to see night-by-night attrition, a 37%-too-high F&B minimum, and the suggested redlines.
4. **Stress test:** click *Hiring freeze −20%*. Meridian bills **$133K** for rooms and food nobody used vs. **$17K** at Hotel Wabash.
5. **Recommend:** switch to *CFO mode* (the cheap airport hotel wins), then *Balanced* (Lakeshore wins). Pick asks and **draft the counter-proposal**.

Deep links: [`#cost`](https://aayushm1.github.io/proposal-lens/#cost) · [`#risk`](https://aayushm1.github.io/proposal-lens/#risk) · [`#stress`](https://aayushm1.github.io/proposal-lens/#stress) · [`#recommend`](https://aayushm1.github.io/proposal-lens/#recommend)

## 4. Metrics

| Type | Metric | Target |
|---|---|---|
| ⭐ North star | Hidden cost surfaced per RFP | Tracked per program |
| Speed | Last proposal received → hotel selected | 3 days → < 1 hour |
| Quality | % contracts signed with at-market attrition terms | +25 pts |
| Outcome | Attrition penalties paid (measured via room-block data) | −30% |
| Guardrail | Extraction accuracy on confirmed terms | ≥ 98% |
| Guardrail | Hotel-side proposal win rate for good-faith proposals | No decline |

## 5. Rollout

| Phase | Scope | Why this order |
|---|---|---|
| **V1** | Structured platform RFP responses only. TCO view + rank flip. | No OCR risk; proves the "wow" on clean data |
| **V2** | PDF / Word / email ingestion, clause risk scoring, redlines | The long tail of proposals arrives outside the platform |
| **V3** | Negotiation agent paired with the hotel-side response assistant | Both sides negotiate in one place; a two-sided network effect |

## 6. Risks & mitigations

| Risk | Mitigation |
|---|---|
| Hotels push back on being "scored" | Score **terms, not brands**. Offer hotels a "proposal health" preview before they submit. |
| Extraction errors | Confidence per term, source-page citations, planner confirms flagged terms |
| Perceived as legal advice | Frame output as benchmark guidance; redlines are suggestions |
| Benchmarks leak competitive data | Aggregate only, minimum cohort sizes, market-level not property-level |

## 7. What's in this repo

```
index.html        The full prototype: UI, fictional data and cost model in one file
README.md         This case study
HOW-IT-WORKS.md   Plain-English walkthrough of every screen and every formula
```

Run locally: open `index.html` in a browser, or `npx serve .`

---

Built by **Aayush Mehan**, Product Manager · [Portfolio](https://aayushm1.github.io/) · [GitHub](https://github.com/aayushm1)
