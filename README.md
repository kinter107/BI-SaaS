# Meridian Retail Analytics

A small-shop analytics SaaS concept, starting with an interactive, meeting-ready MVP. The current app is a browser-only prototype built with React, TypeScript, and Vite. It seeds a sample store on first launch and persists edits in that browser's local storage.

## Run it

Requirements: Node.js 20.19+ or 22.12+, and pnpm (or npm).

```sh
pnpm install
pnpm dev
```

Open the local URL printed by Vite. Use **Record a sale**, add a product, record a refund, then return to Overview to see the metrics update. The sample store contains 8 products, 46 sales across the recent month, and 4 refunds. To restore the original sample, clear this site's local storage and reload.

Production build:

```sh
pnpm build
pnpm preview
```

## What's in this milestone

- Overview with period filter, gross sales, refunds, net sales, transaction count, sales trend, top products, and recent sales.
- Product catalog with search and product creation; product types and starter categories.
- Multi-item sales with price snapshots and optional discount.
- Refund history and order-linked refund entry with a remaining-balance cap.
- Store name and currency settings.
- Responsive layout, local demo persistence, and realistic sample records.

## Demo limitations

This is a frontend prototype, not a production SaaS. There is no API, server-side database, real login, user/role management, tenant isolation, invitation flow, secure payment processing, or server-authoritative validation. Browser local storage is not secure and does not synchronize between users or devices. Demo authentication is intentionally not represented as real. Refunds are capped against the original order total, but the prototype doesn't reconcile item-level returned quantities. Analytics run in the browser from the local records. Use fictional data only.

The intended production architecture and staged delivery plan are in [docs/roadmap.md](docs/roadmap.md) and [docs/architecture.md](docs/architecture.md).
