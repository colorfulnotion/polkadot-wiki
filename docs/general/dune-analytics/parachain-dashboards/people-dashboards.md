---
id: people-dashboards
title: People Dashboards
sidebar_label: People Dashboards
description:
  People is a parachain on Polkadot focused on decentralized identity and social interactions,
  enabling users to manage their digital identity and engage in community governance.
keywords: [polkadot, dashboard, dune, people, identity]
slug: ../people-dashboards
---

# People Dashboards

## Overview

People is a parachain on Polkadot focused on decentralized identity and social interactions,
enabling users to manage their digital identity and engage in community governance.

## Featured Dashboards on Dune

Here you'll find a variety of dashboards that help visualize data from the People parachain:

- [People Dashboard](https://dune.com/substrate/people): A comprehensive view of identity management
  and social interaction activities within the People ecosystem.

Please also visit our dashboards for People on
[Dune Analytics](https://dune.com/discover/content/relevant?q=title:People%20author:substrate).

## Key Tables

Data from the People parachain is organized into several key tables: `people.balances`,
`people.blocks`, `people.calls`, `people.events`, `people.extrinsics`, `people.transfers`

## Useful Queries

Currently, no specific queries have been provided. Please check back later for updates.

## Getting Started with Queries

To get started with querying data from Unique, you are welcome to use the mentioned materialized
queries. You can use the following DuneSQL queries as examples:

```sql title="Kusama People Chain Metrics" showLineNumbers
WITH
  holders as (
    SELECT
      date(ts) as logDT,
      COUNT(DISTINCT address_ss58) as cnt
    FROM
      people_kusama.balances
    WHERE
      (free + reserved + misc_frozen) > 0
    GROUP BY
      date(ts)
    ORDER BY
      date(ts) DESC
  ),
  union_account as (
    SELECT
      block_time as logDT,
      balances_cnt as active_cnt
    FROM
      "query_3550689(chain='people_kusama',table_name='balances',count='DISTINCT address_ss58',time_name='ts',where_parameter='WHERE (free + reserved + misc_frozen) > 0')"
  ),
  combine as (
    SELECT
      CAST(holders.logDT as TIMESTAMP (3)) as logDT,
      holders.logDT as date_logDT,
      cnt,
      active_cnt
    FROM
      holders
      LEFT JOIN union_account on union_account.logDT = holders.logDT
    UNION ALL
    SELECT
      CAST(logDT as TIMESTAMP (3)) as logDT,
      logDT as date_logDT,
      0 as cnt,
      active_cnt
    FROM
      union_account
    ORDER BY
      logDT DESC
  )
SELECT
  *,
  SUM(cnt) OVER (
    ORDER BY
      logDT ASC
  ) AS all_cnt,
  SUM(active_cnt) OVER (
    ORDER BY
      logDT ASC
  ) AS all_union_cnt
FROM
  combine
ORDER BY
  logDT DESC
```

Query result:

<iframe src="https://dune.com/embeds/3802657/6393678/" height="350" width="100%" ></iframe>

Visualizations using the query result:

<iframe src="https://dune.com/embeds/3802657/6393679/" height="350" width="100%"></iframe>

<table style={{ width: '100%', border: 'none' }}>
  <tr>
    <td style={{ width: '50%', padding: 0, border: 'none' }}>
      <iframe src="https://dune.com/embeds/3802657/6393684/" height="350" width="100%"></iframe>
    </td>
    <td style={{ width: '50%', padding: 0, border: 'none' }}>
      <iframe src="https://dune.com/embeds/3802657/6393685/" height="350" width="100%"></iframe>
    </td>
  </tr>
</table>

<!-- <iframe src="https://dune.com/embeds/3802657/6393684/" height="350" width="50%" style="float:left;></iframe>

<iframe src="https://dune.com/embeds/3802657/6393685/" height="350" width="50%" style="float:right;></iframe> -->

:::info DuneSQL Referece

For more information on DuneSQL, please refer to the [DuneSQL Cheatsheet](../dunesql-cheatsheet.md)
and
[DuneSQL Official Documentation](https://docs.dune.com/query-engine/Functions-and-operators/index).

:::
