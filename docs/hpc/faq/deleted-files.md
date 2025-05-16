# Accidentally Deleted Some Files

Quite often RCS team receives tickets  in which users say that they have accidentally deleted their files and would like to recover them.

The process to recover recently deleted files is pretty straightforward and users can recover the recently deleted files themselves. We keep snapshots for the past 2 weeks of users home directory and any file deleted from the home directory in the last 2 weeks can be easily recovered.

Please follow the steps below.

Let us assume that the file named my_file was present in the folder /rds/general/user/lragta/home/Usr_query.

1 Change to the directory where the file was originally present.

```console
[user@login ~]$ cd /rds/general/user/lragta/home/Usr_query
```

2 Change to the .snapshots directory (where we keep the files from the last 2 weeks).

```console
[user@login ~]$ cd /rds/general/user/lragta/home/Usr_query/.snapshots
```

3 Type ls and you will see folders named with dates corresponding to last 2  weeks.

```console
[user@login ~]$ ls
```

4 This will give you output like

```console
apsync.userdirs.1702523964  @GMT-2023.12.01-19.30.14  @GMT-2023.12.08-19.30.14
@GMT-2023.11.25-19.30.14    @GMT-2023.12.02-19.30.14  @GMT-2023.12.09-19.30.14
@GMT-2023.11.26-19.30.14    @GMT-2023.12.03-19.30.14  @GMT-2023.12.10-19.30.14
@GMT-2023.11.27-19.30.14    @GMT-2023.12.04-19.30.14  @GMT-2023.12.11-19.30.14
@GMT-2023.11.28-19.30.14    @GMT-2023.12.05-19.30.14  @GMT-2023.12.12-19.30.14
@GMT-2023.11.29-19.30.14    @GMT-2023.12.06-19.30.14  @GMT-2023.12.13-19.30.14
@GMT-2023.11.30-19.30.14    @GMT-2023.12.07-19.30.14
```

5 Suppose you deleted the file on 7th Dec, then go to that directory by

```console
[user@login ~]$ cd @GMT-2023.12.07-19.30.14
```

6 Type ls again and you should see your deleted file or folder.

```console
[user@login ~]$ ls
#This will show your deleted file or folder.
 
my_file
```

7 Copy it back to your home directory.

```console
[user@login ~]$ cp my_file /rds/general/user/lragta/home/Usr_query/my_file
```

With this you should be able to recover your deleted files.

We hope that you will find this page useful and you will be able to recover your accidentally deleted files yourself. 
