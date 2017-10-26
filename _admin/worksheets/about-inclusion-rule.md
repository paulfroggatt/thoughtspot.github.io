---
title: [How the inclusion rule works]
tags:
keywords: "inclusion rule, table, join"
last_updated: tbd
summary: "Use the inclusion rule to specify which data to include in a worksheet where two or more tables are joined. If you are familiar with SQL, you might think of it as a JOIN condition."
sidebar: mydoc_sidebar
permalink: /:collection/:path.html
---
In the case where some of the rows in a fact table do not have a match in any
dimension tables, the inclusion rule determines whether or not the rows are
shown. Because of this, the inclusion rule can affect the number of rows the
worksheet will contain.  The inclusion rule is important if some table values
are empty (or NULL) or if some primary key column values in a fact table do not
have a match to a foreign key in the dimension table. In these cases, how you
set an inclusion rule affects the number of rows in the worksheet.

Only rows in the fact table (also known as the LEFT table) are affected by the inclusion rule. It works like this:

| If you choose |  Then... |
|---------------|------------|
| **Apply full outer join (Full Outer Join)** | The results of both the left and right outer joins are combined, and all matched or unmatched rows from the tables on both sides are shown.|
| **Apply left outer join (Left Outer Join)** | All possible rows in the fact table are shown, regardless of whether they have a match in the dimension tables.|
|  **Apply right outer join (Right Outer Join)** | All possible rows in the second table are shown, regardless of whether they have a match in the dimension tables. |
| **Exclude empty rows (Inner Join)** | Any rows that do not have a match in one of the dimension tables, won't be shown in search results.|

When using **Exclude empty rows (Inner Join)**, the number of rows in the
resulting worksheet can differ from the number of rows in the table when
accessing it directly, because of the join condition. The worksheet acts like a
materialized view. This means that it contains the results of a defined query in
the form of a table.

If you find that the charts and tables built on a worksheet contain a large number of null values (which display as `{blank}` in the web browser), you can fix this by [changing the inclusion rule for the worksheet](change-inclusion-rule.html#).

The answer returned when searching using a worksheet as the source can be different from the answer you get when using the table directly as a source. When using a worksheet as a source, even if you were to select fields that come from only one table in your search, any underlying joins to other tables will still be active. When using the table directly as the source, you will see every value.

This is best understood through an example.

A typical sales fact table contains a column with the employee ID of the person who made the sale. The employee ID column has a foreign key in the employee dimension table. This is the relationship used to join the two tables.

Sometimes a sale has been made directly or through a reseller, without involving a sales person. In this case, the employee ID value for the sale will be empty in the fact table. If you wanted the worksheet to include all sales, regardless of whether or not they were associated with a sales person, you would choose **Include all rows (Left Outer Join)**. If you only want the worksheet to contain sales made by employees, you would choose **Exclude empty rows (Inner Join)**.