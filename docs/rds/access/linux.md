# RDS on Linux

The following instructions will show you how to connect (mount) your Linux computer to the RDS. There may be some differences between Linux desktop environments and instructions for some of the most common one's have been provided here.

You must be connected to either the College network or otherwise following the advice on [accessing services remotely](../../remoteaccess.md) before you can connect to the RDS. 

## Required Packages

Depending on how you wish to connect to the RDS (e.g. command-line or via desktop file browser) you might find it necessary to install additional packages before you are able to use your connection method.

### Command-line mount.cifs

These days the packages to mount SMB exports using `mount.cifs` aren't installed by default so they may need to be installed before the RDS can be mounted; as an example, if you see the "no route to host" error message after running a mount.cifs command, it most like means the required packages aren't installed.

On Ubuntu and Mint you can install support for mount.cifs by running:

```
sudo apt-get install cifs-utils
```

### Gnome Files (Nautilus)

Gnome Files relies on the Gnome Virtual File System (GVFS) to connect to SMB end points such as the RDS. On Ubuntu and Mint you can provide support for this by making sure the `gvfs-backend` package is installed, while on RHEL-based operating systems you can install the `gvfs-smb` package.

## Examples

### Gnome (Ubuntu 22.04)

* Go to the [RDS Directory Paths page](../paths.md) and identify the location that you wish to connect your Linux computer to, making a note of the **RDS Directory Path (macOS and Linux)**
* Open the **file manager** (Files) by clicking on the icon
* Click + **Other Locations** on the side pane
* Enter the **RDS Directory Path (macOS and Linux)** in the **Connect to Server** box in this format e.g. `smb://rds.imperial.ac.uk/rds/project/<project-name>/`
* Select **Registered User**
* Enter your College **Username**
* Enter **IC** in **Domain**
* Enter your College **Password**
* Click **Connect**

### XFCE (Linux Mint 21)

* Go to the [RDS Directory Paths page](../paths.md) and identify the location that you wish to connect your Linux computer to, making a note of the **RDS Directory Path (macOS and Linux)**
* Open the **file manager** (Thunar) by clicking on the icon
* Select **Go** and then **Open Location**.
* The **location bar** on the file manager should now be highlighted. Enter the **RDS Directory Path (macOS and Linux)** in the (now highlighted) **location bar** in this format e.g. `smb://rds.imperial.ac.uk/rds/project/<project-name>/`
* Select **Registered User**
* Enter your College **Username**
* Enter **IC** in **Domain**
* Enter your College **Password**
* Click **Connect**
