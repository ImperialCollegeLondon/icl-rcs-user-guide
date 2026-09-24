# Rclone

[Rclone](https://rclone.org/) is a versatile command-line data transfer tool that supports moving data between a wide range of storage systems. It can be used to transfer data to and from cloud-based services such as SharePoint and Box, as well as on-premises storage platforms that use protocols such as SMB, including RDF-Active.

The RDF-Active is compatible with rclone releases 1.74.4 and later, using the [SMB storage type](https://rclone.org/smb/).

!!! warning
    Please do **NOT** use the **mount** option with Rclone. This will often conflict with the underlying shared file system (such as the RDS), and if you try to mount the file system with Rclone, the only way you/we can unmount the filesystem is to reboot the server on which you established the mount.

## Accessing Rclone on the HPC login nodes

HX1, HX2 and CX3 all have rclone installed via the module system:

```console
$ module spider rclone
$ module load rclone/1.75.1
```

## Securing Your RClone Configuration

Your rclone configuration will typically be stored in `~/.config/rclone/rclone.conf` in your home directory of each HPC cluster. Once you have configured a storage service, be it remote cloud or an on-premise facility such as the RDF-Active, this file will contain access credentials to that storage service. We therefore strongly recommend that you encrypt this file, particularly when it is stored on a shared file system such as the an HPC file system. The configuration file may be encrypted in the following way:

1. Load the rclone module on the login node.

```console
$ module load rclone
```

2. Run the rclone config command to start the configuration wizard.

```
$ rclone config
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q>
```

3. Enter "s" to run the "Set configuration password" command

```console 
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q> s
```

4. Enter "a" to add a password to your configuration file. Make sure to provide a sufficiently long and complex password when requested and to store this password in a safe space such as a password manager (for more advice on passwords see [ICT Passwords page](https://www.imperial.ac.uk/admin-services/ict/self-service/be-secure/passwords-and-extra-security/passwords/)).

```console
Your configuration is not encrypted.
If you add a password, you will protect your login information to cloud services.
a) Add Password
q) Quit to main menu
a/q> a
Enter NEW configuration password:
password:
Confirm NEW configuration password:
password:
Password set
```

5. Once you've set a password you can enter "q" to return to the main menu.

```console
Your configuration is encrypted.
c) Change Password
u) Unencrypt configuration
q) Quit to main menu
c/u/q> q

No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q>
```

Every time you now run an rclone command such as transferring data to/from a remote service, you will be prompted to enter the configuration password.

It IS possible to provide the rclone configuration password using an environment variable, `RCLONE_CONFIG_PASS`, but we urge caution when using this, particularly on shared nodes such as the login nodes. If it becomes necessary to do this, please keep the lifetime of the environment variable short. For example:

```console
$ read -rsp "Enter rclone config password: " RCLONE_CONFIG_PASS
$ echo
$ RCLONE_CONFIG_PASS="$RCLONE_CONFIG_PASS" rclone listremotes
$ unset RCLONE_CONFIG_PASS
```

## Configuring Rclone for the RDF-Active

Full guidance on using the SMB connector with rclone may be found on the SMB page of the rclone website.

The following steps assist with using the SMB connector with the RDF-Active.

1. Load the rclone module.

```console
$ module load rclone
```

2. Start the configuration wizard (noting the comments in the [section above](#securing-your-rclone-configuration) about securing your configuration):

```console
$ rclone config
```

3. Set up a "new remote"

You will need the following core information when create the new remote storage location:

* **Type of storage to configure**: SMB / CIFS (smb)
* **SMB server hostname**: rdf-active.ic.ac.uk
* **SMB username**: Your Imperial username (without @ic.ac.uk)
* **SMB port number**: Default/445
* **SMB password**: Your Imperial password
* **Domain**: ic.ac.uk
* **Service principal name**: Leave blank
* **Use kerberos**: Default (false)
* **Advanced config**: Don't edit the advanced configuration.

Follow the steps to save the remote configuration. You should now have a new remote entry which can be listed with:

```console
$ rclone listremotes
rdfactive:
```

## Using the RDF-Active remote with rclone

You should now be able to use your "rdfactive" remote with standard [rclone commands](https://rclone.org/commands/). You can follow the guidance on [where are my files section](../access/index.md#where-are-my-files) to identify the path that you need to find your files. For example:

```console
$ rclone lsd rdfactive:research/
```

lists the faculty directories in the base of the RDF-Active.

Under some directory paths, you may encounter files and folders located within a .snapshot directory. We recommend excluding these from transfers by default. This can be achieved using the `--exclude` option:

```console
$ rclone ls rdfactive:research/faculty/department/ --exclude "**.snapshots/**"
```

Files can be copied from the rdfactive to your local directory using the copy command. For example:

```console
$ rclone copy rdfactive:research/faculty/department/researchgroup/allocation/myfile.txt ./
```

Would copy `myfile.txt` from the RDF-Active to your local directory.

You will find more examples and advice on using rclone, including for data synchronisation, in the [rclone documentation](https://rclone.org/docs/).