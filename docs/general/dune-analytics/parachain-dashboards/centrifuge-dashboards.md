---
id: centrifuge-dashboards
title: Centrifuge Dashboards
sidebar_label: Centrifuge
description: Centrifuge is a decentralized finance hub and liquid staking platform.
keywords: [polkadot, dashboard, dune, centrifuge, DeFi]
slug: ../centrifuge-dashboards
---

# Centrifuge Dashboards

## Overview

Centrifuge is a platform for real-world asset tokenization. Through Centrifuge, investors gain
access to a diverse range of assets, improving transparency and achieving better insight into their
portfolio. Asset managers tokenize their funds and streamline access to necessary service providers
and investors, saving cost for fund operations and unlocking new sources of capital.

Centrifuge provides both the infrastructure and ecosystem to tokenize, manage, and invest into a
complete, diversified portfolio of real-world assets.

Asset pools are fully collateralized, investors have legal recourse, and the protocol is asset-class
agnostic with pools for assets spanning structured credit, real estate, US treasuries, carbon
credits, consumer finance, and more.

Centrifuge's ecosystem extends beyond its onchain financial infrastructure, incorporating a DAO
(decentralized autonomous organization) supported by a diverse community of finance professionals
and developers.

By bringing the entire structured credit market onchain across securitization, tokenization,
privacy, governance, and liquidity integrations, Centrifuge is building a more transparent,
affordable, and limitless financial system.

Some assets are managed on Ethereum, others are managed on Centrifuge (a polkadot parachain.)

## Featured Dashboards on Dune

Here you'll find a variety of dashboards that help visualize data from the Centrifuge parachain on
Polkadot:

- [centrifuge on Polkadot](https://dune.com/substrate/centrifuge): This dashboard provides details
  for
- assets pools on Centrifuge parachain. (As of June 2024: only one)

## Key Tables

Data from the centrifuge parachain is organized into several key tables: `centrifuge.balances`,

- `centrifuge.balances`
- `centrifuge.blocks`
- `centrifuge.calls`
- `centrifuge.events`,
- `centrifuge.extrinsics`
- `centrifuge.transfers`

The `centrifuge.traces` table is created by a snapshot script utilizing Centrifuge API calls to
fetch accurate values which would be difficult to calculate from the blockchain events alone.

## Useful Queries

Some of the most important queries for Centrifuge are mentioned here.

| Subject Area                | Query                                             | Description                                                     |
| --------------------------- | ------------------------------------------------- | --------------------------------------------------------------- |
| Portfolio                   | [query_3708897](https://dune.com/queries/3708897) | Provides details about the assets in the pools                  |
| Centrifuge Pool Data Anemoy | [query_3708939](https://dune.com/queries/3708939) | Provides details for the Anemoy pool (first pool on Centrifuge) |

Dune users are encouraged to study the source code of the queries, including parts of a query that
may have been commented out for future use.

Uncommenting these parts may accelerate your effort of adopting a query to a slightly different use
case.

## Getting Started with Queries

To get started with querying data from Centrifuge, you are welcome to use the mentioned queries. You
can also use the following DuneSQL queries as examples:

```sql title="Centrifuge Loan Market Data" showLineNumbers
with portfolio as
(
    select
    ts,
    cast(json_value(c.kv, 'strict $.asset_id.id') as int) as asset_id,
    cast(json_value(c.kv, 'strict $.asset_id.pool') as bigint) as pool_id,
    from_unixtime(cast(json_value(c.pv, 'strict $.maturity_date') as bigint)) as maturity_date,
    (cast(json_value(c.pv, 'strict $.outstanding_interest') as uint256)) as outstanding_interest,
    (cast(json_value(c.pv, 'strict $.outstanding_principal') as uint256)) as outstanding_principal,
    (cast(json_value(c.pv, 'strict $.present_value') as uint256)) as present_value,
    (cast(json_value(c.pv, 'strict $.total_borrowed') as uint256)) as total_borrowed,
    (cast(json_value(c.pv, 'strict $.total_repaid_interest') as uint256)) as total_repaid_interest,
    (cast(json_value(c.pv, 'strict $.total_repaid_principal') as uint256)) as total_repaid_principal,
    (cast(json_value(c.pv, 'strict $.total_repaid_unscheduled') as uint256)) as total_repaid_unscheduled,
    (cast(json_value(c.pv, 'strict $.pool_currency.symbol') as varchar)) as currency_symbol,
    (cast(json_value(c.pv, 'strict $.pool_currency.decimals') as int)) as decimals,
    (cast(json_value(c.pv, 'strict $.type') as varchar)) as type
    --pv
    from centrifuge.traces c
    where track='portfolio'
)
select
ts,
asset_id,
pool_id,
maturity_date,
outstanding_interest / pow(10, decimals) as outstanding_interest,
outstanding_principal / pow(10, decimals) as outstanding_principal,
present_value / power(10, decimals) as present_value,
total_borrowed / power(10, decimals) as total_borrowed,
total_repaid_interest / power(10, decimals) as total_repaid_interest,
total_repaid_principal / power(10, decimals) as total_repaid_principal,
total_repaid_unscheduled / power(10, decimals) as total_repaid_unscheduled,
currency_symbol
from portfolio
where type='Other'
order by maturity_date desc

```

The query is fairly typical for a parachain query on Dune. It parses details from the
`centrifuge.traces` table, and displays relevant values with suitable labels.

The query uses Dune's native UINT256 type, which allows to deal with very large numbers and still
maintain precision.

Query result:

<iframe src="https://dune.com/embeds/3734046/6280352/" height="350" width="100%"></iframe>

:::info DuneSQL Reference

For more information on DuneSQL, please refer to the [DuneSQL Cheatsheet](../dunesql-cheatsheet.md)
and
[DuneSQL Official Documentation](https://docs.dune.com/query-engine/Functions-and-operators/index).

:::
