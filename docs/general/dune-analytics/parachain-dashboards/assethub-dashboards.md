---
id: assethub-dashboards
title: AssetHub Dashboards
sidebar_label: AssetHub Dashboards
description:
  AssetHub is a specialized parachain on Polkadot designed to facilitate the handling and management
  of digital assets across various applications. It supports asset tokenization, trading, and
  bridging services within the Polkadot ecosystem.
keywords: [polkadot, dashboard, dune, assethub, digital assets]
slug: ../assethub-dashboards
---

# AssetHub Dashboards

## Overview

AssetHub is a specialized parachain on Polkadot designed to facilitate the handling and management
of digital assets across various applications. It supports asset tokenization, trading, and bridging
services within the Polkadot ecosystem.

## Featured Dashboards on Dune

Here you'll find a variety of dashboards that help visualize data from the AssetHub parachain:

- [AssetHub](https://dune.com/substrate/assethub): Explore comprehensive analytics on digital asset
  management and operations within the AssetHub parachain.

## Key Tables

Data from the AssetHub parachain is organized into several key tables: `assethub.balances`,
`assethub.blocks`, `assethub.calls`, `assethub.events`, `assethub.extrinsics`, `assethub.transfers`

## Useful Queries

Here are **materialized** or **parameterized** queries for Unique that may be useful to build your
own charts:

- [AssetHub Sum of Stablecoin on Each Chain by Time](https://dune.com/queries/3526956)
  (`query_3526956(asset_id='[ASSET_ID]')`)

## Getting Started with Queries

To get started with querying data from Unique, you are welcome to use the mentioned materialized
queries. You can use the following DuneSQL queries as examples:

```sql title="Sum of USDC in Polkadot Parachains" showLineNumbers
select
  *
from
  "query_3526956(asset_id='1337')" -- AssetHub Sum of Stablecoin on Each Chain by Time
where
  not chain_name = ''
```

Query result:

<iframe src="https://dune.com/embeds/3527830/5933830/" height="350" width="100%"></iframe>

Visualized result:

<iframe src="https://dune.com/embeds/3527830/5933863/" height="350" width="100%"></iframe>

## DuneSQL

It is important to note that when querying on Dune Analytics, DuneSQL is employed. Although most
functions and syntax are similar to standard SQL, there are still some differences compared to other
versions of SQL. Below is a comparison table of common features between DuneSQL and Google BigQuery
SQL.

:::info For more information on DuneSQL, please refer to the
[DuneSQL documentation](https://docs.dune.com/query-engine/Functions-and-operators/index). :::

| Problem Type                                | BigQuery                                                                                                                                                                                                              | DuneSQL(V2)                                                                                                                                                  | Description                                                                                                                                      |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| JSON Reading Method                         | `JSON_EXTRACT_SCALAR(call_args, "$.remark")`                                                                                                                                                                          | `JSON_EXTRACT_SCALAR(JSON_PARSE(call_args), '$.remark')`                                                                                                     | In DuneSQL, `JSON_PARSE` is needed to split the JSON if it is initially not in JSON format but is transformed into a JSON string.                |
| JSON array to SQL array                     | `JSON_EXTRACT_ARRAY(JSON_EXTRACT(pv, '$.others'))`                                                                                                                                                                    | `cast(json_extract(pv, '$.others') as array<json>)`                                                                                                          | BigQuery uses a function for this conversion, while DuneSQL utilizes casting and supports the JSON data type.                                    |
| HEX to UTF8                                 | `SAFE_CONVERT_BYTES_TO_STRING(FROM_HEX(SUBSTR(hex_encode, 3)))`                                                                                                                                                       | `FROM_UTF8(from_hex(SUBSTR(hex_encode, 3)))`                                                                                                                 | In DuneSQL, the `SAFE_CONVERT_BYTES_TO_STRING` is not required.                                                                                  |
| Time Axis                                   | `TIMESTAMP_TRUNC(block_time, DAY) >= TIMESTAMP("2023-12-01")`                                                                                                                                                         | `block_time >= date('2023-12-01')`                                                                                                                           | Time conversion in DuneSQL is simpler, involving direct usage of `variable operator date(value)`.                                                |
| Data Type Conversion (FLOAT64 to DOUBLE)    | `CAST(JSON_EXTRACT_SCALAR(nominationpools_rewardpools, '$.lastRecordedRewardCounter') AS FLOAT64)`                                                                                                                    | `CAST(JSON_EXTRACT_SCALAR(nominationpools_rewardpools, '$.lastRecordedRewardCounter')`                                                                       | BigQuery refers to the data format as FLOAT64, while in DuneSQL, it is termed DOUBLE.                                                            |
| Handling Null Values                        | `IFNULL(prev_member_bonded, 0)`                                                                                                                                                                                       | `COALESCE(prev_member_bonded, 0)`                                                                                                                            | In DuneSQL, BigQuery's `IFNULL` is equivalent to `COALESCE`.                                                                                     |
| Calculating Local Time and Subtracting Days | `TIMESTAMP_TRUNC(ts, DAY) >= TIMESTAMP(DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY))`                                                                                                                                    | `ts >= date(current_date - interval '30' day)`                                                                                                               | In BigQuery, operations on dates require functions, but DuneSQL allows direct use of `+` and `-`.                                                |
| Using Hyperlinks in Tables                  | `SELECT concat(concat(concat("<a href='https://analytics.polkaholic.io/superset/dashboard/77/?account=", address_ss58), "'>"), if(address_name is null, concat(address_ss58, '</a>'), concat(address_name, '</a>')))` | `CONCAT('<a target="_new" href="https://analytics.polkaholic.io/superset/dashboard/77/?account=', address_ss58, '">', address_ss58 ,'</a>') AS address_ss58` | DuneSQL enables string concatenation using `CONCAT`, making it straightforward compared to the multiple `concat` functions required in BigQuery. |
