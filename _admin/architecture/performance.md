---
title: [Performance considerations]
tags:
keywords: data,cluster,memory,import,tsload
last_updated: tbd
summary: "Make sure you understand the performance considerations in your installation."
sidebar: mydoc_sidebar
permalink: /:collection/:path.html
---
ThoughtSpot is configured and licensed by memory availability. However, there
are other considerations that will impact the performance of your solution. It’s
important to understand these considerations prior to implementation, since some
solutions will perform better than others.

Each node in a ThoughtSpot cluster has been found to perform ideally with less
than 250GB of data and fewer than .5 billion total rows of data. For schemas
that are particularly complex, performance is increased with even fewer rows of
data per node. Ways to reduce the total amount of data and rows of data include
limiting the amount of data (number of years, etc.) or combining long, but
narrow tables together.

## Data Boundaries

Total rows in a result of a join can also have an impact. In general, it is
recommended that you have fewer than 10 billion rows in a many-to-many join.
Keep in mind these other boundaries:

|Description |Boundary|
|-------------------------|--------------------|
| Max number of rows that can be downloaded | 10M (default is 1M) |
|Size in CSV format| 1 TB per appliance|
|Total number of rows across all tables| 2B per appliance|
|Many-to-Many (Generic) join cardinality|10B per appliance|
|Load frequency| Once every hour|


## Worksheet Boundaries

Worksheets must have less than 1000 columns. For aggregated worksheets, you
should keep in mind the following:

* Number of columns should be less than 50
* Number of rows should be less than 10 millions

You can use an ETL process to circumvent these limitations. Speak with
ThoughtSpot Customer Support to learn more.

### Aggregated worksheets

To be able to join an aggregated worksheet with a base table, your installation
must be configured to allow the behavior. The aggregated worksheet cannot have
more than 5 tables involved. Moreover, the number of rows in the final
aggregated worksheet cannot be greater than 1000.

### Chasm Trap worksheets

For chasm trap scenarios (two or more fact tables joined via a shared dimension)
the following boundaries are recommended:

|Description |Boundary|
|-------------------------|--------------------|
|Max number of fact tables in a worksheet	|5|
|Max number of shared dimensions	|2|
|Max number of rows in non co-sharded shared dimension table of chasm trap	|10M|
|Max number of rows in co-sharded shared dimension table of chasm trap	| 1B |

## Row level security Boundaries

Max number of unique RLS rules with search data suggestions should not exceed
15K.

## Data Connect Boundaries

The maximum number of connections should be less than or equal to 25.

## Scheduled pinboards

For scheduled pinboards, ideal performance is to have 50 or fewer scheduled
pinboard jobs.
