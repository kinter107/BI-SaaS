# Product roadmap

The immediate goal is to validate that shop owners find value in entering or importing their sales and that the resulting reports support decisions. Do not confuse a convincing local prototype with a secure multi-customer SaaS.

## Milestone 1 — Meeting MVP (current)

**Outcome:** demonstrate product catalog → sale → dashboard update → refund → net sales update.

- Working responsive React prototype with seeded fictional store data.
- Product create, sale entry, refund entry, dashboard, history, store settings.
- Local browser persistence for a repeatable meeting demo.
- Present as a concept validation build, not as production software.

## Milestone 2 — API, identity, and tenant foundation

**Outcome:** a real shop owner can register, create a shop, and safely use a persistent workspace.

- Go modular monolith, PostgreSQL, migrations, Docker Compose, environment configuration, OpenAPI.
- Password hashing, secure server-side sessions, registration/login/logout, shop creation.
- Organizations and memberships; owner role; API authorization and tenant-scoped data access.
- Database constraints, parameterized SQL, audit events, auth rate limits, secure headers.
- Integration tests for registration, membership, and cross-tenant access denial.

## Milestone 3 — Catalog and trustworthy transactions

**Outcome:** shop staff can manage a real catalog and record accurate sales/refunds.

- Categories and product types; product CRUD/archive, SKU, cost and selling price.
- Sale and item snapshots, server-side totals, atomic persistence, idempotency.
- Partial and multiple refunds against original sale items, reasons, over-refund prevention.
- Sales/refund history, pagination, validation, meaningful errors.
- Tests for price history, concurrent refund limits, totals, duplicates, tenant isolation.

## Milestone 4 — Analytics and onboarding

**Outcome:** accurate, understandable metrics help the owner take an action.

- API analytics for today, week, month, last month, and custom dates in shop timezone.
- Gross sales, refunds, net sales, order count, units, average order value, previous-period comparisons.
- Daily trend and best-selling products using the same date boundaries as summary metrics.
- Empty states, first-run onboarding, data export, accessible loading/error states.
- Validate metric definitions with prospective shop owners before expanding charts.

## Milestone 5 — Teams, plans, and operations

**Outcome:** a shop can invite staff and understand plan limits.

- Role permissions for owner, manager, cashier, analyst, viewer.
- Email invitations with hashed one-time tokens, expiry, revocation, acceptance.
- Subscription plans, entitlements, usage limits, billing provider integration and webhook reconciliation.
- Audit log, account recovery, backup/restore, production observability, deployment pipeline.

## Milestone 6 — Imports, integrations, and advanced intelligence

**Outcome:** reduce manual work and expand decisions supported by the platform.

- CSV imports with preview, column mapping, validation, duplicate detection, rollback.
- POS/e-commerce connectors with sync status, retries, idempotency, reconciliation.
- Branches, inventory movements, product variants, stock alerts as customer evidence supports them.
- Profit reporting with cost snapshots; customer behavior only with clear privacy/legal design.
- Forecasts and anomaly detection with explanations, confidence, and opt-in controls.

## Architecture decisions to preserve

1. Modular monolith before microservices.
2. Tenant membership is a first-class relation; tenant scope is enforced in the API and database design.
3. Financial values are exact and historical transactions are immutable.
4. Reports share one definition of timezone and date boundaries.
5. Prefer integrations/imports over assuming customers will enter every transaction manually.
6. Build advanced inventory, CRM, AI, or multi-branch features only after customer discovery confirms the problem.

## Tomorrow's discovery conversation

- How are sales captured today: paper, spreadsheet, or POS?
- What would make repeated data entry worth their time? Would CSV import or a POS connector be required immediately?
- Do they need revenue only, or margin/profit (and are product costs available)?
- Which decisions/reports do they review weekly? Which are missing today?
- Who else needs access, and what should each person be allowed to do?
- Are there multiple locations, currencies, or sales channels?
- What evidence would make them pay monthly, and what onboarding support would they expect?
