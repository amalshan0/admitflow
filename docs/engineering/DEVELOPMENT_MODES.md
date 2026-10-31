# Development data and operating modes

The maintained product serves authorised institutions. Synthetic fixtures, public programme
references and sandbox payments support development; they do not grant admissions authority.
Nothing in repository cleanup changes a running institution's configuration or data.

## Authorised institutional operation

Staff manage institution-owned courses, active intakes, structured eligibility rules,
application fees, discounts and approved knowledge. Course selection, academic evaluation,
application review and payment confirmation remain separate code-owned actions. Unknown
rules, fees or capacity must remain unknown. Enabling live payments or hosting requires
the operational checks in [security and production](security-and-production.md).

## Isolated synthetic development

[Compose](COMPOSE_RUNTIME.md) creates a separate database, private credentials and synthetic
courses. The native seed is also a development tool. Use synthetic student data in tests and
provider evaluations. Seed/configuration scripts must preserve existing credentials and
records unless an explicit, separately authorised reset is intended.

## Public programme references

The tracked JAIN Kochi snapshot contains 64 programme references and official-source URLs.
It is an independent public-information development dataset, not an institution integration.
Listing a course does not establish verified admission rules, fees, seats, intakes or
permission to process university applications. Do not populate missing facts with zeros
presented as real prices or invent batches. See [ADR-016](../decisions/ADR-016-public-reference-catalogue.md).

`replace_demo_catalogue` is a destructive, one-off development utility. Do not run it as
ordinary setup or against an institution's live data. It is not a production onboarding API.

## Legacy sample-application exception

[ADR-017](../decisions/ADR-017-demo-applications.md) records a development-only exception
retained for interpreting existing records and testing the application workflow. It needs
the explicit setting, the configured development institution, an opted-in reference course,
Razorpay test credentials and live mode disabled. The recorded sample fee of INR 3,000 is
separate from all institution/university fees. Persistent application/invoice markers and
paid-before-approval checks remain mandatory. Sample approval never grants a university seat.

The local test-only provider status check is not a substitute for authenticated production
payment confirmation. Live invoices retain signed-event matching and replay controls.
Disable the exception before a real pilot; do not relabel existing sample records or strip
their safeguards. Retiring the legacy code/schema later needs a record-preserving migration
and regression coverage. This context consolidation does not delete financial records,
reverse migrations or modify ignored environment files.

## Test evidence and isolation

Historical provider probes, a created/cancelled test link and simulated payment responses
are not a completed latest-flow test-card payment. Keep provider, handset, database and
browser evidence separate in [verification](../verification/README.md). Run live evaluations
only with an intended data profile and explicit authority for external messages/charges.
