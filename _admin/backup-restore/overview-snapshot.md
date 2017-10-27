---
title: [Work with snapshots]
tags:
keywords: backup,snapshot
last_updated: tbd
summary: "A snapshot is a point-in-time image of your running cluster you can use to restore the cluster back to a specific point in time. "
sidebar: mydoc_sidebar
permalink: /:collection/:path.html
---
In this section, you learn how to work with the default snapshot configuration
that is enabled on ever cluster and how to take manual snapshots of your own.

## Create a manual snapshot

You should create a snapshot before making any changes to the environment,
loading a large amount of new data, or changing the structure of a table. You
can have up to 20 manual snapshots at a time, after which, you have to clear one
before you are able to create another. If you need to delete a snapshot, contact
ThoughtSpot Support.

{% include note.html content="When you upgrade, all existing snapshots from the previous version of ThoughtSpot will become manual snapshots." %}

Taking a snapshot is fast, about 20 seconds. It happens invisibly in the
background of a running cluster.  If you would like to restore from a snapshot
instead, contact ThoughtSpot Support.

To create a snapshot:

1. Log in to the Linux shell using SSH.
2. Initiate a snapshot, providing a name and reason for creating it: Snapshot names must be 44 characters or less.


    ```
    $ tscli snapshot create <snapshot_name> <reason>
    ```

3. Check that the snapshot was created:

    ```
    $ tscli snapshot ls
    ```


## Configure periodic snapshots

By default, each ThoughtSpot cluster is configured to take automatic, periodic snapshots of your cluster. This section explains how to learn more about the periodic snapshots in your cluster.

The default snapshot policy is enabled for every cluster. You can use the `tscli snapshot-policy show` command to display the current policy for periodic snapshots.

```

$ tscli snapshot-policy show
schedule {
    period {
        number: 1
        unit: HOUR
        }
        retention_policy {
            bucket {
                time {
                    number: 1
                    unit: HOUR
            }
            capacity: 3
            }
            bucket {
                time {
                    number: 4
                    unit: HOUR
            }
            capacity: 2
            }
    }
    offset_minutes_from_sunday_midnight: 0
}
enabled: true

```

This policy takes a snapshot every hour starting at midnight on Sunday. It retains the snapshots from the last three hour intervals and two snapshots from two of the previous 4 hour intervals. That means, there are 5 periodic snapshots retained overall. For detailed information about understanding the schedule, see [Understand backup/snapshot schedules](how-to-create-a-schedule.html).

You shouldn't change this default policy unless instructed to by support. If you have to adjust it for some reason, you can use, `tscli snapshot-policy update` command. This opens the current policy in an editor. Your policy should never retain more than 20 snapshots at any point in time. Exceeding this number can impact cluster performance.

You cannot delete the snapshot policy. However, you can disable the policy by executing the `tscli snapshot-policy disable` command. And you can re-enable it by running, `tscli snapshot enable-policy`.

CAUTION: Backups rely on the snapshot system. For this reason, you should never disable the periodic snapshot system. For example, if you have disabled the periodic snapshots and periodic backups are enabled, then the periodic backup may use a very outdated snapshot or it may fail all together.

To check your current periodic snapshot policy:

1. Log in to the Linux shell using SSH.
2. Enter `tscli snapshot-policy show` to view the policy.
