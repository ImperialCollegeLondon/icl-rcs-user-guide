# Recovering Deleted Files

Every evening we take a snapshot of all of the files in Home and Project spaces (but not Ephemeral spaces) which we keep for up to two weeks. You can access these snapshots in order to recover files that you've accidentally deleted, or to recover an earlier version of a file. Please see the instructions below on how to access these snapshots.

Please note: these snapshots should not be considered as backups (as they reside on the same file system), and we may need to remove snapshots at short notice for operational reasons.

## Windows
From Windows, right-click on the folder that contained the deleted file (or file you want to retrieve an earlier version of) and select "Restore previous versions".

!!! info

    You must have authenticated to the RDS with your username as **IC\yourusername** in order to use the "Restore previous versions" option in Windows. If you are uncertain if this is the case, please disconnect the mapped drive and follow the instructions at [RDS on Windows](../access/windows.md) to reconnect.

## On the HPC systems

If working interactively on the HPC systems, do `cd .snapshots` in the relevant directory to see all snapshots arranged by date. These are read-only directories that you can copy files from.