---
title: [Use a script to load data]
tags:
keywords: "tsload,csv,script,load"
last_updated: tbd
summary: "Learn how to load one or files in bulk with a script."
sidebar: mydoc_sidebar
permalink: /:collection/:path.html
---
If you need to load data from multiple CSV files, create a script to automate the process. You can also use a similar script to automate recurring data feeds.

## Understand how to create a script

The data loading script is a text file that contains all the calls to `tsload` for loading the data from your CSV files.

The example script shown here uses the `cat` command to read the data file, and pipes it to tsload. When creating and testing your script, you may wish to replace each `cat` with `cat -10`, to load only the first ten lines of each file. This allows you to quickly run a test of your script. When the test succeeds for all the data files, you can then remove each `-10`, so the complete files will load when you run the script again.

1. Log in to the Linux shell using SSH.
2. Navigate to the directory that contains your CSV files and open a new file in a text editor.
3. Type in the commands to load the data. This example shows commands to load three files:

    ```
    cat Players.csv | tsload
       --target_database baseball --target_table "players"
       --empty_target --field_separator ","
       --max_ignored_rows 10 --bad_records_file bad_records.txt
       --has_header_row --alsologtostderr --null_value ""

    cat AllstarFull.csv | tsload
       --target_database baseball --target_table "allstarfull"
       --empty_target --field_separator ","
       --max_ignored_rows 10 --bad_records_file bad_records.txt
       --has_header_row --alsologtostderr --null_value ""

    cat Appearances.csv | tsload
       --target_database baseball --target_table "appearances"
       --empty_target --field_separator ","
       --max_ignored_rows 10 --bad_records_file bad_records.txt
       --has_header_row --alsologtostderr --null_value ""
    ```

4. Save the file.
5. Run the script:

    ```
    $ ./load_baseball_data.sh
    ```


## Loading data efficiently

If you have a very large data file that takes a long time to load, you can reduce the load time by splitting it up into multiple files and loading them in parallel using multiple invocations of `tsload`. If the size of any of your data files is greater than 50 million rows, running `tsload` in parallel can reduce the load time significantly.

* Split up your large data file into multiple smaller files.

* Stage the data files in a location accessible to the node on which you'll run the script. Usually you'll use an [NAS mounted file system]({{ site.baseurl }}/admin/setup/NAS-mount.html#).

* Create a script to load the files (see example below).

* Run the script to load the files. You will make your script multi-threaded by invoking multiple loader threads (between 1 and 5 are recommended).

To optimize the load time even further, determine what the bottleneck is and adjust your process accordingly.  If the disk I/O for reading the data files is the bottleneck, you can stage the data files on separate NAS mounted file systems and reference them accordingly in your script. If the CPU on the machine you're using to run the load script is the bottleneck, you can split the load script into the same number of parts as you have nodes in your ThoughtSpot instance, place one script on each node, and run them in parallel. Make sure the other nodes are able to access the data files where they are staged.  Running the load script on separate nodes will put the data on all the nodes, just as when you run the script on a single node. Running the script on all the nodes at the same time just lets you take advantage of CPU power of each node for hashing data files.

For example, suppose you have 30 days of data in 30 files, one for the data collected on each day. Each day's data file contains 10 million rows, for a total of 300 million rows of data. You want to load the whole month of data. For this example we'll have 5 loader processes, each one handling 6 days of data. Here is a sample script you could use the load the data files in parallel:

```
/* Script load_script.sh, loads 30 days of data in parallel */

#!/bin/bash

pidlist=""

cat day1.csv day2.csv day3.csv day4.csv day5.csv day6.csv | tsload  
--target_database sales --target_table SALES_FACT --max_ignored_rows 10
--bad_records_file ./SALES_FACT.bad --date_format %Y-%m-%d
--date_time_format "%Y-%m-%d %H:%M:%S" --source_data_format delimited
--field_separator "|" --null_value "" --enclosing_character "\""
--boolean_representation 1_0 &

pidlist="$pidlist $!" &

cat day7.csv day8.csv day9.csv day10.csv day11.csv day12.csv | tsload  
--target_database sales --target_table SALES_FACT --max_ignored_rows 10
--bad_records_file ./SALES_FACT.bad --date_format %Y-%m-%d
--date_time_format "%Y-%m-%d %H:%M:%S" --source_data_format delimited
--field_separator "|" --null_value "" --enclosing_character "\""
--boolean_representation 1_0 &

pidlist="$pidlist $!" &

cat day13.csv day14.csv day15.csv day16.csv day17.csv day18.csv | tsload  
--target_database sales --target_table SALES_FACT --max_ignored_rows 10
--bad_records_file ./SALES_FACT.bad --date_format %Y-%m-%d
--date_time_format "%Y-%m-%d %H:%M:%S" --source_data_format delimited
--field_separator "|" --null_value "" --enclosing_character "\""
--boolean_representation 1_0 &

pidlist="$pidlist $!" &

cat day19.csv day20.csv day21.csv day22.csv day23.csv day24.csv | tsload  
--target_database sales --target_table SALES_FACT --max_ignored_rows 10
--bad_records_file ./SALES_FACT.bad --date_format %Y-%m-%d
--date_time_format "%Y-%m-%d %H:%M:%S" --source_data_format delimited
--field_separator "|" --null_value "" --enclosing_character "\""
--boolean_representation 1_0 &

pidlist="$pidlist $!" &

cat day25.csv day26.csv day27.csv day28.csv day29.csv day30.csv | tsload  
--target_database sales --target_table SALES_FACT --max_ignored_rows 10
--bad_records_file ./SALES_FACT.bad --date_format %Y-%m-%d
--date_time_format "%Y-%m-%d %H:%M:%S" --source_data_format delimited
--field_separator "|" --null_value "" --enclosing_character "\""
--boolean_representation 1_0 &

pidlist="$pidlist $!" &

wait $pidlist
```

Call your script using a command like:

```
nohup bash ./load_script.sh > master_log.txt &

tail -f master_log.txt
```

Constructing your script in this way will execute all the commands in the background, and output to the file `master_log.txt`. You'll see a running status as the commands in the script execute. After the script completes, you can check the log file for detailed information, such as the number of rows that loaded successfully.
