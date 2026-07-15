# OutboundSuppliesReports (Notes)

## What it does

Calculates what customers are billed for outbound packaging supplies on a 3PL warehouse-management platform. The model translates the backend CostPlus pricing rules into SQL and snapshots cost and price at shipment time so charges stay auditable even if pricing rules change later.

## Business context

Before this model, outbound supply charges had no durable, queryable billing record outside the application layer. The data team and finance team had no reliable place to query supply billing for reporting or audits. This model is that place.

## Technical highlights

- Merge incremental strategy on a SHA-256 surrogate key, reprocessing only changed rows rather than re-reading all 28.4M rows on each build
- Append-only schema evolution so new columns don't break existing runs
- ~11 joins across 4 schemas (public, configuration, billing, inventory) to resolve fee logic, product metadata, and warehouse configuration
- 27 documented columns with semantic-layer-ready metrics (cost, price, margin)
- Production indexes, identity columns, and role grants managed via dbt pre/post-hooks, with no manual DBA steps

## Scale

- Source table: 28.4M shipment rows
- Incremental window: new/updated rows only per run

## What I'd do differently

I'd add a reconciliation test that cross-checks the model's billed amounts against the application's invoice totals for a sample period. Right now the correctness guarantee is implicit. A test makes it explicit and gives future engineers something to run against.

## Proof (available on request)

The full 4-commit Git patch (May 27 to Jun 9 2026, cryptographically signed under my work email), the merged pull request showing reviewer approval, and dbt run output are kept in a private repository and available to prospective employers on request.
