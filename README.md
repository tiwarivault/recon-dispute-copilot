# Reconciliation & Dispute Copilot

A browser-based **payment reconciliation engine + chargeback dispute copilot** for payment gateways / PSPs. Runs 100% client-side — your data never leaves the page.

**Live:** https://tiwarivault.github.io/recon-dispute-copilot/

---

## What it does

### 1 · Reconciliation
Upload (or paste) two files — a **gateway/PG** file and a **bank/settlement** file — and the deterministic engine matches them and classifies every break:

- **Robust CSV parsing** — handles quoted fields, commas inside values, `₹` and `1,00,000` separators.
- **Multi-key matching** — tries every reference column common to both files (UTR / RRN / ARN / order_id / txn_id…), so files that use *different* id columns still reconcile.
- **Break classification:** Matched · Timing · Amount mismatch · Fee mismatch · Missing in settlement · Unmatched settlement · Duplicate.
- **Explainable root cause + confidence** — each break shows a *likely* cause to **confirm** (e.g. "settled net of fee", "~18% GST on fee", "T+n cycle"), never asserted as fact.
- **Priority-sorted exception queue** (P1/P2/P3), KPIs (matched %, ₹ at risk), pattern insights, and **download-all-breaks CSV**.
- **Handles large files** — parsing + matching run in a **Web Worker** so the UI never freezes. Benchmarked ~**1 lakh rows in <1s, 5 lakh in ~2.5s**. Built-in stress-test buttons let you verify on your own machine.

### 2 · Chargeback Copilot
Enter a reason code or description → the tool maps it to a category, lists the evidence that actually wins that category, flags your evidence gaps, gives a **rule-based Fight / Accept / Manual** recommendation, and generates a **representment draft** grounded in the case facts — for a human to review, edit and submit.

---

## Design principle: split brain
- **Deterministic** rules own all money math and matching.
- The copilot layer only **classifies, explains and drafts** — it never moves money or finalizes a decision.
- Consequential actions stay **human-in-the-loop**.

## Honest scope
This is a **browser prototype**, not an enterprise platform. It has **no backend, database, auth, audit log or scheduled ingestion** — those are the next phase for daily operational use. Reason codes and evidence lists are **illustrative**; verify against current Visa / Mastercard / RuPay / NPCI-UPI manuals before operational use. Nothing here is an official NPCI/network rule.

## Roadmap
1. Harden prototype (done) · 2. Backend + DB (persistence + history) · 3. Integrate real PSP/gateway data + refunds/reversals · 4. Automation, RBAC, maker-checker, audit · 5. Controlled pilot.

---
*Built as a payments-ops demo. Deterministic engine + rule-based copilot + human-in-the-loop.*
