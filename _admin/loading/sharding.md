---
title: [Sharding]
tags:
keywords: sharding,partitioning,distribution
last_updated: tbd
summary: "Sharding is a type of database partitioning that separates very large databases into smaller, faster, more easily managed parts called data shards."
sidebar: mydoc_sidebar
permalink: /:collection/:path.html
---
For the best performance, you should split (or shard) very large tables across
nodes. If you have a large dimension table, you might choose to co-shard it with
the fact table it will be joined with.

## Sharding a fact table

Use sharding to split large tables into parts for distribution across nodes.
This is typically done with large fact tables, to provide optimal performance.

When sharding, you'll choose a column to be the distribution key. This column
should contain a value that has a good distribution (roughly similar number of
rows with each value in that column). This is typically the primary key, but it
can be any single column or a set of columns.

The recommended number or shards depends upon the number of nodes in your cluster:

|Number of Nodes|Number of Shards|
|---------------|----------------|
|3|96|
|4-12|128|
|13-24|256|
|25-36|384|
|37-48|512|
|49-60|640|
|61-72|768|

To specify the number of shards, add `PARTITION BY HASH ( )` to your `CREATE TABLE`
statement, specifying the number of shards and the sharding key. For example:

```
TQL> CREATE TABLE ...

...PARTITION BY HASH (96) KEY ("customer_id");
```

If no sharding is specified or the number of shards specified is one, the table
is assumed to be unsharded (i.e. the table physically exists on each node).

If no sharding key is specified, but the number of shards is greater than one,
the table is assumed to be sharded randomly. The system does not use primary
keys as sharding keys by default.

If you want to use the primary key for sharding, you must specify that the table
is to be partitioned by hash on the primary key, as in this example:

```
TQL> CREATE TABLE "supplier" (

  "s_suppkey" BIGINT,

  "s_name" VARCHAR(255),

  "s_address" VARCHAR(255),

  "s_city" VARCHAR(255),

  "s_phone" VARCHAR(255),

  CONSTRAINT PRIMARY KEY ("s_suppkey")

  )  PARTITION BY HASH (96) KEY ("s_suppkey");
```

## Sharded (distributed) dimension tables

In a typical schema, you'd have a sharded fact table with foreign keys to
replicated dimension tables (which exist on every node). This works best where
dimension tables are small (under 50MB). So if your dimension tables are small,
you should shard the fact tables and not shard the dimension tables they will be
joined with.

If you have a large dimension table, you can distribute it the same way as the
fact table it joins to. You might choose to use distributed dimension tables if:

-   The dimension table is large (over 50MB).
-   The tables are always joined using the same columns.

If all of these requirements for the distributed dimension table are met, the
tables are automatically co-sharded for you:

-   The tables are related by a primary key and foreign key.
-   The tables are partitioned on the same primary key/foreign key.
-   The tables have the same number of regions (or shards).

If the dimension table will be joined to multiple fact tables, all of the fact
tables must be sharded in the same way as the dimension table. Self-joins are
not supported.

When a fact and its dimension table are co-sharded the two tables will always be
joined on the sharding key. Data skew can develop if a very large proportion of
the rows have the same sharding key.

This example shows the `CREATE TABLE` statements that meet the criteria for
co-sharding of a fact table and a distributed dimension table:

```
TQL> CREATE TABLE products_dim (
  "id" int,
  "prod_name" varchar(30),
  "prod_desc" varchar(100),
  PRIMARY KEY ("id")
)
PARTITION BY HASH (96) KEY ("id")
;

TQL> CREATE TABLE retail_fact (
  "trans_id" int,
  "product_id" int,
  "amount" double,
  FOREIGN KEY ("product_id") REFERENCES products_dim ("id")
)
PARTITION BY HASH (96) KEY ("product_id")
;
```

## Sharded (distributed) fact tables

You can also join two sharded fact tables with different shard keys, otherwise
know as _non co-sharded_ tables. It may take a while to join two tables sharded
on different keys since a lot of data redistribution is required. Therefore,
ThoughtSpot recommends that you use a common shard key for two fact tables.

You are not limited by the column connection or relationship type.
