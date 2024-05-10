---
id: moonbeam-dashboards
title: Moonbeam Dashboards
sidebar_label: Moonbeam Dashboards
description:
  Moonbeam is a fully Ethereum-compatible parachain on the Polkadot network, enabling developers to
  deploy existing Solidity smart contracts and DApp frontends with minimal changes. It is designed
  to provide interoperability and compatibility, bridging the gap between Ethereum and Polkadot.
keywords: [polkadot, dashboard, dune, moonbeam, Ethereum]
slug: ../moonbeam-dashboards
---

# Moonbeam Dashboards

## Overview

Moonbeam is a fully Ethereum-compatible parachain on the Polkadot network, enabling developers to
deploy existing Solidity smart contracts and DApp frontends with minimal changes. It is designed to
provide interoperability and compatibility, bridging the gap between Ethereum and Polkadot.

## Featured Dashboards on Dune

Here you'll find a variety of dashboards that help visualize data from the Moonbeam parachain:

- [Moonbeam DEX](https://dune.com/substrate/moonbeam-dex): Explore decentralized exchange activities
  and token swaps within the Moonbeam ecosystem.
- [Moonbeam Governance](https://dune.com/substrate/moonbeam-governance): Detailed insights into
  governance proposals, voting, and outcomes within the Moonbeam community.

## Key Tables

Data from the Moonbeam parachain is organized into several key tables: `moonbeam.balances`,
`moonbeam.blocks`, `moonbeam.calls`, `moonbeam.events`, `moonbeam.extrinsics`, `moonbeam.transfers`

## Useful Queries

Currently, there are no specific useful queries provided. Please check back later as this section
will be updated with materialized queries for Moonbeam.

## Getting Started with Queries

To get started with querying data from Unique, you are welcome to use the mentioned materialized
queries. You can use the following DuneSQL queries as examples:

```sql title="Moonbeam Referenda Result" showLineNumbers
SELECT DISTINCT
  CAST(JSON_EXTRACT_SCALAR(data, '$[0]') as INTEGER) as referenda_id,
  get_href (
    'https://moonbeam.subscan.io/referenda_v2/' || cast(JSON_EXTRACT_SCALAR(data, '$[0]') as VARCHAR),
    cast(JSON_EXTRACT_SCALAR(data, '$[0]') as VARCHAR)
  ) as referenda_id_url,
  varbinary_to_uint256 (
    from_hex(SUBSTR(JSON_EXTRACT_SCALAR(data, '$[1].ayes'), 3))
  ) / pow(10, 18) as aye_total,
  varbinary_to_uint256 (
    from_hex(SUBSTR(JSON_EXTRACT_SCALAR(data, '$[1].nays'), 3))
  ) / pow(10, 18) as nay_total,
  varbinary_to_uint256 (
    from_hex(
      SUBSTR(JSON_EXTRACT_SCALAR(data, '$[1].support'), 3)
    )
  ) / pow(10, 18) as support,
  method as result
FROM
  moonbeam.events
WHERE
  section = 'referenda'
  and (
    method = 'Confirmed'
    or method = 'Rejected'
    or method = 'Cancelled'
    or method = 'TimedOut'
  )
ORDER BY
  referenda_id DESC
```

Query result:

<iframe src="https://dune.com/embeds/3679042/6187736/" height="350" width="100%"></iframe>

## DuneSQL

It is important to note that when querying on Dune Analytics, DuneSQL is employed. Although most
functions and syntax are similar to standard SQL, there are still some differences compared to other
versions of SQL. Below is a comparison table of common features between DuneSQL and Google BigQuery
SQL.

::: info For more information on DuneSQL, please refer to the
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
