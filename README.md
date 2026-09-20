# fraudwatch-streaming

## The business problem

Banks process millions of card/UPI/wire transactions daily and must flag suspicious ones within seconds, before the transaction settles — not hours later in a batch report. A transaction that looks fine in isolation (₹5,000 grocery purchase) can be highly suspicious in context (same card used in Mumbai and Dubai 10 minutes apart).

## problem statement:
"Build a real-time transaction monitoring system that ingests card transaction events, applies fraud-detection rules within seconds of each transaction, and surfaces flagged transactions on a live dashboard for a fraud-ops team to review."

## Realistic fraud scenarios to detect (pick 4–5, not all)

These are the actual pattern categories banks monitor — use them as your rule logic:

Velocity fraud — too many transactions from one account in a short window (e.g. 5+ transactions in 2 minutes). Real scenario: a stolen card gets used rapidly at multiple merchants before the owner notices.

Geo-impossibility ("impossible travel") — same card used in two locations too far apart to be physically possible in the time elapsed (Mumbai at 10:00, Dubai at 10:15). This is one of banks' most common real rules.

Amount anomaly — transaction significantly above the account's historical average (e.g. 10x their typical spend). Real scenario: a compromised card suddenly used for a ₹2,00,000 electronics purchase when the account normally spends ₹2,000–5,000.

Odd-hour activity — transactions at unusual times for that account's normal behavior (e.g. 3 AM when the account has no history of night activity).

New-merchant-category risk — first-ever transaction at a high-risk merchant category (crypto exchange, gambling, wire transfer service) for that account.

Round-number/structuring pattern — multiple transactions just under a reporting threshold (e.g. several ₹49,000 transactions to stay under a ₹50,000 reporting limit) — this mimics real anti-money-laundering (AML) logic, not just fraud.

You don't need ML for this — real fraud systems layer simple rule-based flags (what you're building) before ML models, precisely because rules are fast, explainable, and auditable, which regulators require. That makes your rule-based approach realistic, not simplified.

## Data schema to simulate

Design your fake data generator around fields real transaction systems actually carry:

Field	Example
transaction_id	UUID

account_id	consistent per simulated "customer"

card_id	linked to account

amount	INR/USD

merchant_category	grocery, electronics, travel, crypto, gambling, ATM

merchant_location (lat/long or city)	for geo-impossibility checks

timestamp	event time, not just ingestion time — important for windowing

transaction_type	POS, online, ATM, wire

Tip for realism: simulate ~50–100 "customer" accounts with a normal baseline behavior each (typical spend range, typical hours, typical locations), then inject a small percentage (~2–5%) of transactions that break one of the rules above. This mirrors how real fraud detection is evaluated — against a mostly-normal population with rare true positives.
