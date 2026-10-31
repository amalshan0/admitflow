# Intent: admissions review and configured discounts

Institutions need an application review after payment because payment does not by itself
grant admission. Staff approve, waitlist or reject through authorised recorded actions.
Application/review states, discounts, Razorpay integration and staff surfaces now exist;
remaining latest-flow provider/handset/production acceptance is in STATUS and Task 73.

Institutions define discount codes in structured records. The assistant may suggest an
active code; backend validation owns its validity and final amount. AI cannot invent or
negotiate discounts. Refund handling remains a staff/provider responsibility.

Real admissions use authorised courses, rules, fees, intakes and review/capacity policy.
The legacy development reference exception (ADR-017) keeps a separate sample fee and does
not allow a discount to waive its required test payment. It grants no university seat.
See [SPEC.md](../../SPEC.md) and [development modes](../engineering/DEVELOPMENT_MODES.md).
