---
title: [Understand formulas in searches]
tags:
keywords: tbd
last_updated: tbd
summary: "To provide richer insights, you can add a formula to your search. "
sidebar: mydoc_sidebar
permalink: /:collection/:path.html
---
The Formula Builder includes many types of operators, such as logical (if, then, else), math, date, and text string functions.

You can create a formula from directly within a search. If you have the privilege that allows you to create or edit worksheets, you can also create a formula within a worksheet. Formulas in worksheets act as derived columns, so that anyone who uses the worksheet as a data source will see the formula as just another column.

Adding a formula within a search works much the same way as adding a formula to a worksheet. However, you will be able to edit the formula directly from within the answer. If you add the answer to a pinboard and share it with the **Edit** privilege, other people can see the formula results, too. In order to make edits to the formula, they also need to have the **Edit** privilege on the underlying data.

-   **[Add a formula to a search](../../complex-search/how_to_add_formula.html)**  
You can add a formula directly within a search. Some common reasons for using a formula in a search are to perform mathematical functions, check for and replace null values, or add if...then...else logic.
-   **[View or edit a formula in a search](../../complex-search/edit_formula_in_answer.html)**  
You can always go back and view or edit a formula that has been added to a search. Do this by clicking the edit icon next to its name in the **Columns** listing.
-   **[About aggregate formulas](../../complex-search/aggregation_formulas.html)**  
When working with formulas, it is useful to understand the difference between regular (or row-wise) formulas and aggregation formulas.
-   **[About conversion formulas](../../complex-search/conversion_formulas.html)**  
Some formulas require the input to be of a particular data type. If you find that you want to pass a value to the function, but it is of the wrong data type, you can convert it using a conversion formula.
-   **[About date formulas](/advanced-search/formulas/about_date_formulas.html)**  
Date formulas allow you to apply date related functions to your formulas.
-   **[About percent (simple number) calculations](/advanced-search/formulas/about_percent_calculations.html)**  
You can use simple number functions to perform useful percent calculations.
-   **[About conditional formulas (operators)](/advanced-search/formulas/conditional_sum.html)**  
Conditional formulas, or operators, allow you to apply if/then/else conditions in your formulas.
-   **[About nested formulas](../../complex-search/about_nested_formulas.html)**  
Nested formulas, or formula on formula, allow you to reference a formula within another formula.
-   **[About formula support for chasm trap schemas](../../complex-search/about_formula_support_for_chasm_trap_schemas.html)**  
You can create a formula that involves aggregated measures coming from multiple fact tables of a chasm trap.