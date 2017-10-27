---
title: ["Row level security (RLS)"]
tags:
keywords: tbd
last_updated: tbd
summary: "Using row level security, you can effectively customize search results and pinboards for each group that views them."
sidebar: mydoc_sidebar
permalink: /:collection/:path.html
---
Row level security (RLS) allows you to define which groups can see individual
rows in a table, based on the values in one of its columns. This RLS feature can
handle thousands of groups, and allows you to set up flexible rules that are
self-maintaining.

## How RLS works

RLS works at the group level, not the individual user level. By default, all groups can see all rows for any table they can view. You can limit the rows a group can see by setting conditions on column values. The row level security rules you define on a table also apply to any worksheets and pinboards based on that table.

There are several reasons you might want to use row level security:

|Reason|Example|
|------|-------|
|Hide sensitive data from groups who should not see it.|In a report with customer details, hide potential customers (those who have not yet completed their purchase) from everyone except the sales group.|
|Filter tables to reduce their size, so that only the relevant data is visible.|Reduce the number of rows that appear in a very large table of baseball players, so that players who are no longer active are not shown except to historians.|
|Enable creation of a single pinboard or visualization, which can display different data depending on the group who is accessing it.|Create one sales pinboard that shows only the sales in the region of the person who views it. This effectively creates a personalized pinboard, depending on the viewer's region.|

## RLS and administrators

If your installation has enabled the **Can Administer RLS** privilege, users groups with this privilege (directly or indirectly) can bypass row-level security (RLS) rules. This privilege can only be assigned by a user who already **Has administration privilege**.

Users with **Has administration privilege** are administrators and can see can see all data sources, and no type of row level security applies to them.
