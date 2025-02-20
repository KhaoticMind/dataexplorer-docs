---
title: minif() (aggregation function) - Azure Data Explorer
description: This article describes minif() (aggregation function) in Azure Data Explorer.
ms.reviewer: alexans
ms.topic: reference
ms.date: 02/13/2020
---
# minif() (aggregation function)

Returns the minimum value across the group for which *Predicate* evaluates to `true`.

* Can be used only in context of aggregation inside [summarize](summarizeoperator.md)

See also - [min()](min-aggfunction.md) function, which returns the minimum value across the group without predicate expression.

## Syntax

 `minif` `(`*Expr*`,`*Predicate*`)`

## Arguments

| Name | Type | Required | Description |
|--|--|--|--|
| *Expr* | string | &check; | Expression that will be used for aggregation calculation. |
| *Predicate* | string | &check; | Expression that will be used to filter rows. |

## Returns

The minimum value of *Expr* across the group for which *Predicate* evaluates to `true`.

## Example

This example shows the minimum damage for events with casualties (Except 0)

**\[**[**Click to run query**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA3WOsQ6CUAxFd7+iIwQGfwAXcHAwMWFwrlKlCa+Y16Ji/Hif1NXp3tvenrS1MYbtncR09QZ6GkkHDQa8UuVSx/GmhftD8hRtLqEhtF4rl4Yjna3wsJNuiQmnUwgY+UWwZ3HCka2vUSccjEmrwMKXzFdl5gDYrHPA9MZv/s05nGZoDY0gcR89xb/MVF+uWWU0mYYh+1PMPyFcEcH8AAAA)**\]**

```kusto
StormEvents
| extend Damage=DamageCrops+DamageProperty, Deaths=DeathsDirect+DeathsIndirect
| summarize MinDamageWithCasualties=minif(Damage,(Deaths >0) and (Damage >0)) by State 
| where MinDamageWithCasualties >0 and isnotnull(MinDamageWithCasualties)
```

**Results**

The results table shown includes only the first 10 rows.

| State          | MinDamageWithCasualties |
| -------------- | ----------------------- |
| TEXAS          | 8000                    |
| KANSAS         | 5000                    |
| IOWA           | 45000                   |
| ILLINOIS       | 100000                  |
| MISSOURI       | 10000                   |
| GEORGIA        | 500000                  |
| MINNESOTA      | 200000                  |
| WISCONSIN      | 10000                   |
| NEW YORK       | 25000                   |
| NORTH CAROLINA | 15000                   |
| ... | ... |