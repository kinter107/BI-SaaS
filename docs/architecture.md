# Architecture direction

## Product direction

Meridian is a retail data platform for small shops. Owners record or import business activity; the product turns it into understandable, decision-oriented reports without requiring analytics expertise. Manual entry validates the workflow hypothesis, but importing from existing POS systems will likely be essential for adoption.

## Production target

Start as a modular monolith: React + TypeScript web client, versioned Go HTTP API, PostgreSQL, and background jobs only when needed. Keep domain modules separate (identity, organizations, catalog, transactions, analytics, billing). Avoid microservices and streaming infrastructure until measured scale or integration requirements justify them.

### Tenant identity and access

- `users` are global identities; `organizations` represent shops; `memberships` connect users to shops and carry role/status.
- Every organization-owned row carries `organization_id`.
- Resolve active organization from the authenticated membership, never trust a tenant identifier as authorization from request input.
- Scope every repository query and composite foreign key by organization. Add PostgreSQL row-level security as defense in depth after application query paths are established.
- Roles begin with owner, manager, cashier, analyst, and viewer. Enforce permissions in API handlers/domain services, not just in navigation.
- Invitations store a hash of a random, expiring, single-use token. An invite cannot grant a role beyond the inviter's authority.

### Data model domains

- Identity: `users`, `organizations`, `memberships`, `invitations`, `sessions`.
- Catalog: `product_types`, `categories`, `products`, later `product_variants`.
- Commerce: `sales`, `sale_items`, `refunds`, `refund_items`.
- Platform: `plans`, `subscriptions`, `audit_events`; later `branches`, `inventory_movements`, integration/import records.

Use UUID primary keys. Use composite uniqueness/foreign keys such as `(organization_id, id)` to prevent accidental cross-tenant references. Store monetary values in integer minor units plus ISO currency code (or exact decimal with a clear scale policy), never binary floating point. Store timestamps in UTC; keep an IANA timezone on the organization for reporting. Snapshot product name, SKU, and unit price in sale items so later catalog edits do not rewrite history. Treat posted sales/refunds as immutable; corrections are reversal records.

### Transaction and analytics rules

- Create a sale and all line items in one database transaction. Server computes totals from catalog values and validates organization membership and active products.
- Use idempotency keys for sale/refund creation. Do not trust browser totals.
- Refund items reference original sale items; serialize or lock refund checks so concurrent requests cannot over-refund. Enforce cumulative refunded quantity and amount limits.
- Define gross sales as completed sale totals before refunds; refunds as posted refund totals; net sales as gross minus refunds. Define order count, units, and average order value in a shared analytics service so cards and charts use the same date window and timezone.
- For larger reporting loads, introduce daily organization aggregates after observing query pressure; keep source transactions authoritative.

## MVP implemented here

The current Vite client is intentionally a browser-only meeting prototype. Seeded fictional records and user actions are stored in local storage. It is useful to demonstrate the shop workflow and product experience, but does not implement the production architecture above.
