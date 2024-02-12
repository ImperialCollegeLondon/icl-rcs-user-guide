# RDS on Linux

The following instructions will show you how to connect (mount) your Linux computer to the RDS. The may be some differences between Linux desktop environments and instructions for some of the most common one's have been provided here.

You must be connected to either the College network or otherwise following the advice on [accessing services remotely](../../remoteaccess.md) before you can connect to the RDS. 

## Gnome (Ubuntu 22.04)

* Go to the [RDS Directory Paths page](../paths.md) and identify the location that you wish to connect your Linux computer to, making a note of the **RDS Directory Path (macOS and Linux)**
* Open the **file manager** (Files) by clicking on the icon
* Click + **Other Locations** on the side pane
* Enter the **RDS Directory Path (macOS and Linux)** in the **Connect to Server** box in this format e.g. `smb://rds.imperial.ac.uk/rds/project/<project-name>/`
* Select **Registered User**
* Enter your College **Username**
* Enter **IC** in **Domain**
* Enter your College **Password**
* Click **Connect**

## XFCE (Linux Mint 21)

* Go to the [RDS Directory Paths page](../paths.md) and identify the location that you wish to connect your Linux computer to, making a note of the **RDS Directory Path (macOS and Linux)**
* Open the **file manager** (Thunar) by clicking on the icon
* Select **Go** and then **Open Location**.
* The **location bar** on the file manager should now be highlighted. Enter the **RDS Directory Path (macOS and Linux)** in the (now highlighted) **location bar** in this format e.g. `smb://rds.imperial.ac.uk/rds/project/<project-name>/`
* Select **Registered User**
* Enter your College **Username**
* Enter **IC** in **Domain**
* Enter your College **Password**
* Click **Connect**