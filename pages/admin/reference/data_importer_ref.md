---
title: [ThoughtSpot Loader flag reference]
tags: [formatting]
keywords: tbd
last_updated: tbd
summary: "blerg"
sidebar: mydoc_sidebar
---
For recurring data loads and for scripting loads, use the ThoughtSpot Loader \(tsload\). This reference section lists all the flags that can be used to modify the behavior of tsload.

## General tsload flags

|Flag|Description|Notes|
|----|-----------|-----|
|`--target_database <database>`|Specifies the pre-existing target database into which tsload should load the data.| |
|`--target_schema <schema>`|Specifies the target schema.|Default is "falcon\_default\_schema".|
|`--target_table <table>`|Specifies the tables that you want to load into the database.|The tables must exist in the database specified by `--target_database`.|
|`--empty_target`|Specifies that any data in the target table is to be removed before the new data is loaded.|If supplied, any rows that exist in the table specified by `--target_database` and `--target table` will be deleted before this data load. To perform an "upsert" on the existing data, omit this flag or specify `--noempty_target`.|
|`--max_ignored_rows <number>`|Specifies the maximum number of rows that can be ignored if they fail to load.|If the number of ignored rows exceeds this limit, the load will be aborted.|
|`--bad_records_file <path\_to\_file>/<file\_name>`|Specifies the file to use for storing rows that failed to load.|Input rows that do not conform to the defined schema in ThoughtSpot will be ignored and inserted into this file.|
|`--date_format <date\_formatmask>`|Specifies the format string for date values.|The default format is yearmonthday e.g. "Dec 30th, 2001" and is represented as 20011230. Use the date format specifications supported in the [strptime library function](http://man7.org/linux/man-pages/man3/strptime.3.html).|
|`--date_time_format <date\_formatmask> <time\_formatmask>`|Specifies the format string for datetime values.|The default is yearmonthday hour:minute:second e.g. Dec 30th, 2001 1:15:12 and is represented as 20011230 01:15:12. Use the datetime format specifications supported in the [strptime library function](http://man7.org/linux/man-pages/man3/strptime.3.html).|
|`--time_format <time\_formatmask>`|Specifies the format string for time values.|The default is hour:minute:second. Use the time format specifications supported in the [strptime library function](http://man7.org/linux/man-pages/man3/strptime.3.html).|
|`--v=[0|1|2|3`|Specifies the verbosity of log messages.|Provide a value for verbosity level. By default, verbosity is set to the minimum, which is 0. This value is similar to a volume control. At higher levels your log receives more messages and that log more frequently. This is used for debugging. You should not change this value unless instructed by ThoughtSpot Support.|
|`--skip_second_fraction`|Skips fractional seconds when loading data.|If supplied, the upserts logic may be affected, especially if the date time being loaded is a primary key, and the data has millisecond granularity. Load the data twice, once as a string with a primary key, and again with second granularity date time. There is no support to store fractional seconds in the ThoughtSpot system.|

## File loading tsload flags

The following flags are used when loading data from an input file:

|Flag|Description|Notes|
|----|-----------|-----|
|`--source_file <path\_to\_file>/<file\_name>`|Specifies the location of the file to be loaded.| |
|`--source_data_format [csv|delimited]`|Specifies the data file format.|Optional. The default is csv.|
|`--field_separator "<delimiter>"`| Specifies the field delimiter used in the input file.

 | |
|`-- trailing_field_separator` |Specifies that the field separator appears after every field, including the last field per row.|Example row with trailing field separator: a,b,c,The default is false.

|
|`--null_value "<null\_representation>"`|Specifies how null values are represented in the input file.|These values will be converted to NULL upon loading.|
|`--date_converted_to_epoch [true|false]`|Specifies whether the “date” or “datetime” values in the input file are represented as epoch values.| |
|`--boolean_representation [true_false | 1_0 | T_F | Y_N]`|Specifies the format in which boolean values are represented in the input file.|The default is T\_F. You can also use this flag to specify other values. For example, if your data used Y for true and NULL for false, you could specify:```
--boolean_representation Y_NULL
```

|
|`--has_header_row`|Indicates that the input file contains a header row.|If supplied, the first row of the file is ignored. If not supplied, the first row of the file is loaded as data.|
|`--escape_character "<character>"`|Specifies the escape character used in the input file.|If no value is specified, the default is "\(double quotes\).|
|`--enclosing_character "<character>"`| Specifies the enclosing character used in the input file.

 |If the enclosing character is double quotes, you need to escape it, as in this example: `--enclosing_character "\""`|
|`--use_bit_boolean_values = [true | false]`|Specifies how boolean values are represented in the input file.| If supplied, the input CSV file uses a bit for boolean values, i.e. the false value is represented as 0x0 and true as 0x1. If omitted or set to false, boolean values are assumed to be T\_F, unless you specify something else using the flag `--boolean_representation [true_false | 1_0 | T_F | Y_N]`.

 |

**Parent topic:** [Reference](../../admin/reference/intro_reference.html)