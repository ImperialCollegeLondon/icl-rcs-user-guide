# Accessing the RDF-Active on Linux

The following instructions will show you how to access the RDF-Active by mounting it as a network drive on your Linux computer. To do this, you must be connected to the college network either directly or via [a private and secured remote connection](../../remoteaccess.md).

Please note that there may be some differences between Linux desktop environments which requires different steps. Instructions for some of the most common ones such as Ubuntu (with a Gnome environment) and Linux Mint (with a Cinnamon environment) have been provided here.

## Required Packages

Depending on how you wish to connect to the RDF-Active (e.g. command-line or via desktop file browser) you might find it necessary to install additional packages before you are able to use your connection method.

### Command-line: mount.cifs

These days the packages to mount SMB exports using `mount.cifs` aren't installed by default so they may need to be installed before the RDF-Active can be mounted. As an example, if you see the "no route to host" error message after running a mount.cifs command, it most likely means the required packages aren't installed.

On Ubuntu and Mint you can install support for mount.cifs by running:

```
sudo apt-get install cifs-utils
```

### Gnome file browser: Gnome Files

The official file manager for Ubuntu is Gnome Files, formerly known as Nautilus. It relies on the Gnome Virtual File System (GVFS) to connect to SMB end points, such as the RDF-Active.

On Ubuntu and Mint you can provide support for this by making sure the `gvfs-backend` package is installed with a command like:

```
sudo apt install gvfs-backends
```

On RHEL-based operating systems you can instead install the `gvfs-smb` package with a command like:

```
yum install gvfs-smb
```

## Examples

### Gnome (Ubuntu 24.04)

1. Open the file manager (**Files**) by clicking on the icon.
2. Click **+ Other Locations** on the side pane.
3. Enter the **RDF-Active** path you wish to connect in the **Server Address** box:
    * To access the RDF-Active's "root" directory, the Linux path is `smb://rdf-active.ic.ac.uk/research/`
    * To access a specific storage allocation's directory, review the [Where are my files](./index.md#where-are-my-files) section to identify the allocation's path.
4. Select **Registered User**.
5. Enter your Imperial **username**.
6. Enter **ic.ac.uk** in **Domain**.
7. Enter your Imperial **Password**.
8. Click **Connect**.

### Cinnamon (Mint 22.2)

1. Open the file manager (**Files**) by clicking the icon.
2. From the **File** menu, click **Connect to Server..**.
3. In the window that appears, use the menu to change the **Type** to **Windows share**.
4. Enter the following connection details:
    * **Server:** `rdf-active.ic.ac.uk`
    * **Share:** `research`
    * **Folder:** Either leave as "`/`" for the RDF-Active's "root" directory, or enter a specific storage allocation's path, which can be found with the [Where are my files](./index.md#where-are-my-files) section.
    * **Domain Name:** Enter `ic.ac.uk`
    * **User name** and **Password:** Enter your Imperial username and password.
5. Click **Connect**.