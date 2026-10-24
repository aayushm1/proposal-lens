# How Proposal Lens works (plain English)

This page explains what each screen does and exactly how every number is calculated. Everything runs in the browser from one file (`index.html`). All data is fictional.

---

## The scenario

**Northwind Technologies** is planning its **Q2 Sales Kickoff 2027** in Chicago:

| Input | Value |
|---|---|
| Dates | May 12–14, 2027 (4 hotel nights) |
| Attendees | 450 |
| Room block per night | 140 · 380 · 380 · 90 = **990 room nights** |
| Planner's F&B projection | $135,000 (before service charge and tax) |
| Planner's AV plan | $48,000 with their preferred vendor |
| Venue budget | $600,000 |
| Chicago taxes used | 17.4% on rooms, 11.5% on F&B |

Five hotels replied, each in a different format: two PDFs, one Word document, one email, and one structured platform response.

---

## Screen 1: Proposal inbox

Shows the five proposals the way a planner sees them today, sorted by **quoted room rate**. That ranking is deliberately misleading. It's the "before" picture.

Clicking **Normalize & analyze** plays an extraction log: each file is "read", key terms are pulled out, and suspicious terms are flagged (⚠). In a real product this is where an LLM extracts the terms. In the prototype, the extracted terms are pre-written in the data.

---

## Screen 2: True cost

Every hotel is costed on the **same basis**, at 100% attendance.

```
True total cost =
    room nights × (rate + mandatory nightly fee) × (1 + room tax)
  − comp rooms   (1 free room per N booked, valued at rate + tax)
  − rebate       ($ per room night, if offered)
  + F&B          max(F&B minimum, your projection) × (1 + service charge) × (1 + F&B tax)
  + meeting room rental   (unless F&B reaches the hotel's waiver threshold)
  + AV           $48K × exclusivity markup  (+ any outside-vendor fee)
  + Wi-Fi        $ per person per day × 450 × 3 days
  + transport    (e.g. shuttles needed from an airport hotel)
```

- **The rank flip** draws a line from each hotel's position by quoted rate to its position by true cost. Red lines fall, green lines rise.
- **Where the money goes** is a stacked bar per hotel, so you can see *why* a hotel is expensive (fees, F&B, rental, AV).
- **Terms vs. benchmark** colours each term green (at or better than market), amber (slightly worse) or red (much worse).

"Costs not visible in quoted rates" compares the true cost with what you'd *expect* from the quoted rate alone (rate × room nights × tax + your F&B projection at a 24% service charge + your AV plan), summed across hotels.

---

## Screen 3: Contract risk

Each hotel has a list of **flags**. Every flag has:

- a **severity**: High (25 pts), Medium (12 pts), Low (5 pts), or Strength (0)
- a **type**: *contract* (terms) or *fit* (dates, capacity, location)
- the **contract excerpt**, with the risky words highlighted
- the **benchmark** ("Yours: 10% night-by-night · Market: 20% cumulative")
- a **suggested redline**

`Risk score = sum of severity points, capped at 100.` Contract risk and program-fit risk are shown separately.

---

## Screen 4: Stress test

**The attendance slider (0–40% drop)** recalculates two kinds of "money paid for nothing":

1. **Room attrition penalty.** Shoulder nights lose people faster than peak nights (the drop is weighted 1.6× / 0.85× / 0.85× / 1.4× across the four nights).
   - *Cumulative* contracts compare total pickup with the allowance across the whole stay.
   - *Night-by-night* contracts check every night separately, which is why they hurt more.
   - `Penalty = rooms short × rate × damages %` (80% for "lost profit", 100% otherwise). A resale credit cuts it by 25%.
2. **Unused F&B minimum.** As attendance drops, food actually consumed drops too, but you still pay the minimum. `Waste = (minimum − actual need) × service charge × tax`.

The line chart plots this exposure for every hotel from 0% to 40%.

**Cancellation:** `You'd owe = (room nights × rate + F&B minimum) × the hotel's cancellation % for that many days out.`

---

## Screen 5: Recommend

Each hotel gets four sub-scores (0–100):

| Dimension | How |
|---|---|
| Cost | Cheapest true cost = 100, most expensive = 20, linear in between |
| Contract safety | 100 − contract risk points |
| Attendee experience | Fixed per hotel (rating, location, walkability) |
| Program fit | 100 − fit risk points |

`Final score = weighted average using your slider weights.` Presets: **CFO mode** (cost only), **Balanced** (35/30/15/20), **Attendee-first**.

**Negotiation asks** are generated from each hotel's flags. Each ask's **dollar value** is calculated by re-running the cost model with that change applied (for example, attrition asks are valued at a 20–30% attendance drop, as labelled). **Draft counter-proposal** writes an email to the hotel's sales contact listing the selected asks.

---

## What would be real AI in production

| Prototype | Production |
|---|---|
| Pre-written extracted terms | LLM extraction from PDF/Word/email with per-term confidence and page citations |
| Illustrative benchmarks | Aggregated contract outcomes from the platform's own historical data |
| Template-based email | LLM-written email grounded in the selected asks and the hotel's own language |
| Deterministic cost model | **Stays deterministic.** The AI never invents numbers. |
