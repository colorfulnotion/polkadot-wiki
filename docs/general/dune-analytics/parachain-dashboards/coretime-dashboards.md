---
id: coretime-dashboards
title: Coretime Dashboards
sidebar_label: Coretime Dashboards
description:
  Coretime is a parachain on Polkadot that focuses on time-stamping and data certification,
  providing a decentralized and secure mechanism for verifying data integrity.
keywords: [polkadot, dashboard, dune, coretime, data certification]
slug: ../coretime-dashboards
---

# Coretime Dashboards

## Overview

Coretime is a parachain on Polkadot that focuses on time-stamping and data certification, providing
a decentralized and secure mechanism for verifying data integrity.

## Featured Dashboards on Dune

Here you'll find a variety of dashboards that help visualize data from the Coretime parachain:

- [Coretime Dashboard](https://dune.com/substrate/coretime): Explore comprehensive data
  visualizations related to time-stamping services on Coretime.
- [Kusama Coretime Sales History](https://dune.com/substrate/kusama-coretime-sales-history):
  Detailed historical data and trends of sales activities on the Kusama network.

Please also visit our dashboards for Coretime on
[Dune Analytics](https://dune.com/discover/content/relevant?q=title:Coretime%20author:substrate).

## Key Tables

Data from the Coretime parachain is organized into several key tables: `coretime.balances`,
`coretime.blocks`, `coretime.calls`, `coretime.events`, `coretime.extrinsics`, `coretime.transfers`

## Useful Queries

Currently, no specific queries have been provided. Please check back later for updates.

## Getting Started with Queries

To get started with querying data from Unique, you are welcome to use the mentioned materialized
queries. You can use the following DuneSQL queries as examples:

```sql title="Kusama Coretime Core Task" showLineNumbers
WITH
  max_time AS (
    SELECT
      max(block_time) as max_bt,
      CAST(JSON_EXTRACT_SCALAR(data, '$[0]') AS BIGINT) as core_id
    FROM
      coretime_kusama.events
    WHERE
      section = 'broker'
      AND method = 'CoreAssigned'
    GROUP BY
      CAST(JSON_EXTRACT_SCALAR(data, '$[0]') AS BIGINT)
  )
SELECT
  block_time,
  get_href (
    concat(
      'https://coretime-kusama.subscan.io/extrinsic/',
      cast(extrinsic_id as VARCHAR)
    ),
    extrinsic_id
  ) as extrinsics_url,
  extrinsic_id,
  CAST(JSON_EXTRACT_SCALAR(data, '$[0]') AS BIGINT) as core_id,
  CAST(
    JSON_EXTRACT_SCALAR(data, '$[2][0][0].task') AS BIGINT
  ) as task,
  chain_name
FROM
  coretime_kusama.events
  LEFT JOIN max_time on max_time.core_id = CAST(JSON_EXTRACT_SCALAR(data, '$[0]') AS BIGINT)
  LEFT JOIN query_3765180 as parachain on parachain.chain_id = CAST(
    JSON_EXTRACT_SCALAR(data, '$[2][0][0].task') AS BIGINT
  )
WHERE
  section = 'broker'
  AND method = 'CoreAssigned'
  AND block_time = max_bt
```

Query result:

<iframe src="https://dune.com/embeds/3765471/6333152/" height="350" width="100%"></iframe>

:::info DuneSQL Referece

For more information on DuneSQL, please refer to the [DuneSQL Cheatsheet](../dunesql-cheatsheet.md)
and
[DuneSQL Official Documentation](https://docs.dune.com/query-engine/Functions-and-operators/index).

:::
