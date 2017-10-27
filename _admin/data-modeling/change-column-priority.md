---
title: [Set the search suggestions priority]
tags:
keywords: tbd
last_updated: tbd
toc: false
summary: "You can change the priority that determines which columns are shown in search suggestions and the order in which they appear."
sidebar: mydoc_sidebar
permalink: /:collection/:path.html
---
A column's **INDEX PRIORITY** determines the order or rank in which it and its
values appear in the search dropdown.

![]({{ site.baseurl }}/images/priority-effect.png)

 By default, the **INDEX PRIORITY** value is set to “1” for all columns. You can push
 a column up in the order (increase the rank) by increasing its **INDEX PRIORITY**
 value. A higher value (like 2) will cause the corresponding column and its
 values to appear higher up in the search dropdown than columns with lower value
 (like 1).

![]({{ site.baseurl }}/images/set-priority.png)

You should only use numbers between 1-10 in the **INDEX PRIORITY** field. These values impact the [usage based rankings (UBR)]({{ site.baseurl }}/end-user/search/recent-searches.html#usage-based-ranking).

1. Change the value to a number between 1 and 10.
   Use a value between 8-10 for important columns to improve their search ranking. Use 1-3 for low priority columns.  
2. Save your changes.


## Related information  

[Model the data for searching](semantic-modeling.html#)
