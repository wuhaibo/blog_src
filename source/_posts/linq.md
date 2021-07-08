---
title: linq
description: some notes about linq
date: 2021-07-08 10:24:12
updated: 2021-07-08 10:24:12
tags:
  - linq
  - c#
categories:
  - 技術
comments: true
---
## 1. do not try to join list and database table. use where in instead.

```csharp
var inq = new int[] {1683,1684,1685,1686,1687,1688,1688,1689,1690,1691,1692,1693};
var result = from Q in db.Requirements // db.Requiremetns is a table from db
             where inq.Contains(Q.ID) // a in function
             orderby Q.Description
             select Q;
```

## 2 linq join

```csharp
// from ... in outerSequence
// join ... in innerSequence  
// on outerKey equals innerKey
// select ...
 
from customer in customers
join order in orders 
  on order.customer_id equals customer.id
select new 
{
  customer.id,
  order.id
}
  
```



3 linq to entity show sql query 

```csharp
db.Database.Log = s => System.Diagnostics.Debug.WriteLine(s);
```