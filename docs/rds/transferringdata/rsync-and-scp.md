# rsync and scp

While the login nodes and interactive services can be used to transfer smaller files, for larger transfers it is preferable to use either [Globus](./globus.md) or a "Data Transfer node" (dtn).

## Data transfer node

It is preferable to use a Data Transfer Node (dtn) when uploading/downloading larger volumes of data to/from the RDS as it keep the associated load off of the loading nodes. The dtn nodes themselves have a very limited OS and users will only have access to data transfer programs like rsync and scp. Users should NOT use a dtn node for general use.

Rsync and scp are two commonly used tools for moving data securely between local and remote servers. In this technical documentation, we will explain how to use rsync and scp to move data from a local server to the RDS (Research Data Storage) via the remote host `dtn-c.hpc.ic.ac.uk`. We will also explain how to connect to the remote host using university username and password and provide examples for transferring data.

## Prerequisites:
Before using rsync and scp, you need to have access to the remote host `dtn-c.hpc.ic.ac.uk` and the RDS.

To test if you can connect to the remote host `dtn-c.hpc.ic.ac.uk`, the use the following command:


```console
ssh username@dtn-c.hpc.ic.ac.uk
```

Replace username with your university username. When prompted, enter your university password.

## Moving data with rsync
Rsync is a powerful tool for moving data between local and remote servers. The basic syntax for using rsync is as follows:

```console
rsync [OPTIONS] SOURCE DESTINATION
```

There are many useful options for rsync. Some common ones are:

* `-a`, `--archive`: This option is used to create an archive mode of synchronization, which means that it preserves almost everything about the files, including permissions, timestamps, ownerships, and symbolic links. It is equivalent to using -rlptgoD options.
* `-r`, `--recursive`: This option is used to synchronize files and directories recursively, meaning that it synchronizes all subdirectories and their contents. It is useful for synchronizing entire directory trees.
* `-v`, `--verbose`: This option is used to increase the verbosity of rsync's output, providing more detailed information about the transfer process. It can be helpful for troubleshooting or monitoring the progress of a large transfer.
* `-u`, `--update`: This option is used to transfer only newer files, that is, only the files that have been modified or created since the last synchronization. It is useful for keeping two directories in sync while minimizing the amount of data transferred.
* `-n`, `--dry-run`: This option is used to perform a dry run of the synchronization process, which means that it simulates the transfer without actually making any changes. It can be helpful for testing the rsync command before executing it for real.
* `-h`, `--human-readable`: This option is used to make the output of rsync more human-readable by displaying file sizes in a more intuitive format. For example, it might display "10M" instead of "10485760".
* `-P`, `--partial --progress`: This option is used to enable partial file transfers and to display a progress bar during the transfer. It can be useful for resuming interrupted transfers or for monitoring the progress of a large transfer.
* `-c`, `--checksum`: This option is used to perform a checksum comparison between the source and destination files before transferring them. It ensures that files with the same name but different contents are not accidentally overwritten during the synchronization process.

When transferring data into an RDS project it is recommend to use the options `-rvltoD`. This is to prevent the "out of quota" error message then may otherwise occur. This is a side effect of the way quotas are allocated on 'projects" in the RDS. They are enforce by project group - any group other than a RDS project group has a very small quota. So if you copy a file that is owned by a non-project group and preserve its group ownership (either by `rsync -a` or `cp -rm`, `mv`) then you will go over quota on the the non project group very quickly. The `-rvltoD` options will do the following:

* `-r`: This option tells rsync to synchronize directories recursively, which means that it will transfer all files and subdirectories contained within the directories being synchronized.
* `-v`: This option increases the verbosity of rsync's output, providing more information about the files being transferred.
* `-l`: This option tells rsync to treat symbolic links as files and transfer the files they point to, rather than transferring the links themselves.
* `-t`: This option preserves the modification times of the files being transferred, so that the destination files have the same modification times as the source files.
* `-o`: This option preserves the ownership of the files being transferred, so that the destination files have the same owner as the source files.
* `-D`: This option is equivalent to using --devices --specials, which means that it will transfer device files and special files, such as sockets and fifos.

To transfer data from a local server to the RDS via the remote host "dtn-c.hpc.ic.ac.uk", you need to use the following command:

```console
rsync -rvltoD /path/to/local/folder username@dtn-c.hpc.ic.ac.uk:/rds/general/user/<username>/path/to/remote/folder
```

Replace `/path/to/local/folder` with the path to the local folder you want to transfer. Replace `<username>` with your university username. Replace `/path/to/remote/folder` with the path to the destination folder on the RDS. Note that you need to give the full path on the RDS when transferring data. For example, to transfer data to `~/home/new_data` you should use `/rds/general/user/<username>/home/new_data`.

To transfer date from the RDS to the local server simply swap the SOURCE and DESTINATION 

```console
rsync -rvltoD username@dtn-c.hpc.ic.ac.uk:/rds/general/user/<username>/path/to/remote/folder /path/to/local/folder
```

## Moving data with scp
Scp is a simple tool for transferring files between local and remote servers. The syntax for using scp is as follows:

```console
scp [OPTIONS] SOURCE DESTINATION
```

* `-r`: recursive mode, allows transferring folders.
* `-v`: verbose mode, displays the progress of the transfer.


To transfer data from a local server to the RDS via the remote host "dtn-c.hpc.ic.ac.uk", you need to use the following command:

```console
scp [OPTIONS] /path/to/local/file username@dtn-c.hpc.ic.ac.uk:/rds/general/user/<username>/path/to/remote/file
```

Replace /path/to/local/file with the path to the local file you want to transfer. Replace <username> with your university username. Replace /path/to/remote/ with the path to the destination folder on the RDS. Note that you need to give the full path on the RDS when transferring data.

As with rsync, to transfer from the RDS to a local machine you just switch the SOURCE and DESTINATION.

```console
scp [OPTIONS] username@dtn-c.hpc.ic.ac.uk:/rds/general/user/<username>/path/to/remote/file /path/to/local/file
```