**Out of cash events**

Same core idea as before — split the transaction into "your customer" vs. "someone else's customer" — but the US fee stack is structured differently and the surcharge, not interchange, is usually the bigger revenue line.

**How a US bank earns money per ATM transaction**

There are two separate revenue streams, and which ones apply depends on whose card is being used at whose ATM:

1. **Surcharge** — a fee the ATM-owning bank charges directly to a customer who isn't one of its own account holders. All the ATM-owning banks in a recent industry survey said they charge non-account-holders to use their ATMs, and the average surcharge has climbed to a record $3.22 per transaction, up from $3.19 the year before. This is real, immediate revenue that goes to whoever owns the machine.

2. **Interchange (the network calls it a "Cash Disbursement Reimbursement Fee")** — paid by the cardholder's *issuing* bank to the ATM-*owning* bank, set by the card networks (Visa/Mastercard), not negotiated per-bank. Visa's published schedule tiers this by volume level — roughly $0.30 to $0.60 per cash withdrawal depending on the ATM owner's tier, with alliance-network transactions priced at 0.35% + $0.55, capped at $3.00. This is much smaller than the surcharge line, but it's separate money on top of it.

**When each one kicks in**

| Whose card | Whose ATM | Surcharge? | Interchange? |
|---|---|---|---|
| Your customer | Your ATM | No | No — internal transaction |
| Your customer | Other bank's ATM | N/A to you | You pay it (as issuer) |
| Other bank's customer | Your ATM | **Yes — you collect it** | **Yes — you collect it** |

So the two fee lines only exist together, and only on the **non-customer share of transactions at your own ATMs**. For the far more common case — your own customer using your own ATM — an OOC event costs you *zero* fee/interchange revenue. The real cost there is the emergency CIT trip and the goodwill/reputational hit, same as we discussed for the dissatisfaction line.

**How to quantify it — mix-weighted, same pattern as the India version**

```
Revenue at risk per OOC event =
   (avg txns lost per event)
   × [ (% non-customer traffic at this ATM) × (surcharge + interchange per txn) ]
```

You'd need one more input you probably already track per-ATM or per-segment: **the non-customer transaction share** — this varies enormously by location (an airport or downtown ATM might be 40-60% non-customer traffic; a branch-lobby ATM might be under 10%). That mix is the single biggest lever in this number, more so than the fee rate itself.

**Practical note on the numbers to use:** $3.22 is an *industry average* surcharge from consumer surveys — your own bank's actual posted surcharge (often $2.50–$3.50) and your actual interchange tier from your network agreement are the real inputs to use, not the survey average, since that's what you'd actually stop earning.

Want me to rebuild the Assumptions/OOC_Benefit sheets with a US variant — splitting the flat "fee/interchange per txn" input into `% non-customer traffic`, `surcharge $`, and `interchange $` — so the benefit number reflects this mix instead of one blended rate?

--
Beyond fee/interchange revenue and the CIT-cost/dissatisfaction pair we've already modeled, there are several other benefit categories worth considering for a US bank's OOC-avoidance business case — some are real dollars, some are risk-avoidance that finance teams increasingly want quantified even if imprecisely.

**1. Channel cost-shifting avoidance**
When an ATM goes out of cash, the transaction doesn't just disappear — it usually migrates to a more expensive channel: a teller transaction at a branch, or a call to the contact center. Per-transaction cost differences between channels (ATM vs. teller vs. call center) are usually already tracked by your ops/finance team, and the delta is real, quantifiable savings. This is often a bigger number than the fee/interchange line, because branch/teller unit costs run several dollars per transaction versus cents for ATM self-service.

**2. Premium/unscheduled CIT cost vs. scheduled route cost**
We modeled a flat "emergency CIT cost per event," but it's worth being precise about what that premium actually is: an *unscheduled* CIT dispatch (off-route mileage, overtime, sometimes a contractual premium-service fee from the armored carrier) costs meaningfully more than a *scheduled* replenishment stop on the truck's normal route. The benefit isn't "avoiding a CIT trip" — it's "avoiding the premium over what a scheduled trip would have cost anyway."

**3. Network/SLA uptime obligations**
If your ATMs participate in a surcharge-free network (Allpoint, MoneyPass, CO-OP) or are white-labeled/managed by a third-party ISO, uptime is often contractually monitored, sometimes with penalty clauses or reputational scoring that affects your standing in the network. Preventing OOC events protects both the direct penalty exposure and the fleet's uptime metric used in vendor/partner scorecards.

**4. Regulatory and fair-access risk**
This is a real category for US banks specifically: repeated, predictable ATM downtime — especially if it clusters in lower-income or majority-minority service areas — can draw scrutiny under CRA (Community Reinvestment Act) access considerations, and chronic unreliability can factor into complaint patterns regulators watch under UDAAP. This is hard to put a precise dollar figure on, but it's often flagged qualitatively in a business case rather than ignored, since remediation/exam costs if it does become an issue are large.

**5. Complaint/dispute handling cost**
Distinct from the soft "dissatisfaction" line — this is the hard, auditable cost of an OOC event generating an actual complaint, CFPB complaint filing, or a Reg E dispute if a customer's card was retained or a transaction partially processed during the failure. Your servicing/complaints team should have a real average cost-per-complaint-ticket figure.

**6. Brand/digital reputation spillover**
App Store and Google Play review sentiment, and social media complaints, increasingly get tracked as a KPI by digital banking teams. A cluster of "ATM never has cash" complaints in reviews measurably drags down app ratings, which correlates with new-customer acquisition cost in some banks' own attribution models — again hard to price precisely, but often available if your bank has a marketing analytics team that's already built that link.

**7. Fleet utilization / capital efficiency (indirect)**
This isn't a benefit of *avoiding* OOC per se, but it's the flip side worth noting in the same business case: a recycling ATM that rarely goes out of cash also needs fewer scheduled replenishment trips overall (not just avoided emergency ones), which is a separate, larger operating-cost line usually captured elsewhere in your ATM opex model — worth cross-referencing so you don't double-count it against this OOC-specific benefit.

---

Of these, **#1 (channel cost-shifting)** and **#2 (premium vs. scheduled CIT cost precision)** are the ones I'd prioritize adding to the model first — they're the most defensible with data you likely already have. **#4 and #6** are better handled as a qualitative risk paragraph in your business case rather than forced into a dollar figure.

Want me to add a "channel cost-shifting avoidance" line item to the OOC_Benefit sheet, using teller-vs-ATM unit cost as the new input?

# Bin full events

Yes — bin full events have a genuinely different revenue/cost profile than OOC, because deposits and withdrawals sit on opposite sides of the fee stack. Worth laying out where the analogy holds and where it breaks.

## Where the OOC framing doesn't carry over

The OOC model works because non-customer *withdrawals* generate surcharge + interchange — real fee revenue you stop earning. Deposits don't work that way for most retail banks: accepting your own customer's deposit at your own ATM isn't a fee event, and most banks don't surcharge non-customers for deposits the way they do for withdrawals (and even where allowed, it's rare in practice). So there's no clean "revenue at risk = mix × fee rate" formula here for the retail case.

The exception is **commercial/merchant deposit relationships** — smart-safe or business-ATM deposit services where the bank charges cash-handling/processing fees per deposit or per dollar volume. There, a bin-full event *does* have a direct fee-revenue analog, similar in structure to the OOC surcharge line.

## Benefit categories for avoiding bin-full

**1. Channel cost-shifting avoidance**
A rejected ATM deposit doesn't vanish — it becomes a branch teller deposit, a night-drop/vault-drop, or in commercial cases a delayed pickup. Teller-vs-ATM unit cost delta applies the same way it did for OOC, and is likely the largest quantifiable line here too.

**2. Recycling-model cannibalization (specific to recyclers)**
This is the one with no OOC analog and probably the most important for your fleet. On a recycling ATM, deposited notes are what get redispensed to reduce refill trips. A bin-full event doesn't just block deposits — it removes the machine's ability to recycle, which quietly erodes the exact CIT-trip-reduction benefit your recycling conversion business case is built on. A bin-full event today can also *cause* an OOC event sooner, since notes that should have replenished the dispense cassette never made it there. Worth modeling this coupling explicitly rather than treating OOC and bin-full as independent failure modes — for a recycler they're two ends of the same cash loop.

**3. Premium/unscheduled CIT evacuation vs. scheduled**
Same structure as OOC #2: an emergency bin evacuation (off-route dispatch, overtime, carrier premium) costs more than a scheduled pickup that would've happened anyway. The benefit is the premium delta, not the full trip cost.

**4. Merchant/commercial deposit fee revenue** (where applicable)
If you run business banking cash-deposit services (smart safes, merchant ATM deposit programs), a bin-full event can mean a merchant's deposit doesn't count/settle on time — this hits both a fee line and, more importantly, a same-day-credit/funds-availability commitment that's often contractual.

**5. Partial-transaction / reconciliation risk**
A deposit that's interrupted mid-transaction because the bin fills during the transaction (bill accepted, not yet credited) creates a reconciliation exception — cash physically present but not matched to a ledger entry until manual resolution. This is a hard back-office cost (investigation, provisional credit, potential Reg E dispute if the customer disputes the missing credit) that doesn't have a direct OOC equivalent, since a failed withdrawal is just a failed withdrawal — there's no "cash in limbo" state.

**6. Regulatory/fair-access risk**
Same CRA/UDAAP logic as OOC — chronic deposit-side failures at ATMs in specific geographies read the same way to examiners as chronic withdrawal-side failures.

**7. Complaint/dispute handling cost**
Same structure as OOC #5, but note deposit-related complaints (money "swallowed," provisional credit delays) tend to run hotter and more expensive per ticket than withdrawal-failure complaints, since there's an actual funds-in-dispute component.

**8. Brand/digital reputation spillover**
Same as OOC #6 — "ATM ate my deposit" complaints are typically more damaging to app-store sentiment than "ATM was empty," since they imply the bank lost the customer's money rather than just being inconvenient.

## What I'd prioritize modeling first

**#2 (recycling cannibalization)** and **#1 (channel cost-shifting)** first — #2 because it's the one most specific to your recycling-conversion business case and probably the reason bin-full matters more to you than it would for a pure dispense-only fleet, and #1 because it's the same well-tracked cost delta you already have inputs for from the OOC work.

Want me to sketch how a `Bin_Full_Benefit` sheet would sit alongside your existing `OOC_Benefit` sheet — including a coupling term for the recycling cannibalization effect, so the two failure modes aren't double-counted or under-counted against each other?