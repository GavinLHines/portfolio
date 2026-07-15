# stg_relocation_items (Notes)

## What it does

Converts a legacy inventory relocation log into a clean, auditable movement history. Each row is one relocation event with a full before/after view: asset, location, quantity, user, and timestamp. This makes 5+ years of relocation history queryable in a standardized form for operations analytics and auditing.

## Business context

The raw source log is a dense operational table with normalized foreign keys and no timestamp standardization. Downstream analysts had no clean layer to query against. This model provides that layer: typed fields, resolved lookups, timezone-aware timestamps, and a surrogate key that makes every event uniquely addressable.

## Technical highlights

- Delete+insert incremental strategy on a SHA-256 surrogate key. A 7-day lookback window reprocesses only ~0.3% of the table per build (~99% fewer rows than a full rebuild)
- 10 left joins across 5 schemas to resolve asset, location, user, and warehouse metadata
- Timezone-aware date conversion from raw UTC epoch values to local warehouse time
- Unique and not_null dbt tests on the surrogate key, catching integrity issues at build time

## Scale

- Total history: 1,815,913 relocation events (Nov 2020 – Jun 2026)
- Ongoing volume: ~22,000 events/month
- Incremental scan per run: ~5,400 rows (0.3% of total)

## What I'd do differently

The 7-day lookback window is conservative and covers late-arriving data, but it also re-scans ~150k rows per run unnecessarily on most days. With more production data I'd tune the window to the actual SLA for late-arriving source events rather than leaving it at a round number.

## Proof (available on request)

The full 5-commit Git patch (May 26 to Jun 11 2026, cryptographically signed under my work email), the merged pull request showing reviewer approval, and dbt run output are kept in a private repository and available to prospective employers on request.
