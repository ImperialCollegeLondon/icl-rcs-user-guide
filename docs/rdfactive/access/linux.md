# RDF-Active on Linux

The following instructions will show you how to connect (mount) your Linux computer to the RDF-Active. The may be some differences between Linux desktop environments and instructions for some of the most common one's have been provided here.

You must be connected to either the College network or otherwise following the advice on [accessing services remotely](../../remoteaccess.md) before you can connect to the RDS. 

## Gnome (Ubuntu 24.04)

* Open the file manager (**Files**) by clicking on the icon
* Click **+ Other Locations** on the side pane
* Enter the **RDF-Active** path you wish to connect in the **Server Address** box:
    * For the "base" path of the RDF-Active  `smb://rdf-active.ic.ac.uk/research/`
    * Or read the [Where are my files](./index.md#where-are-my-files) section to map a specific storage allocation.
* Select **Registered User**
* Enter your Imperial **username**
* Enter **ic.ac.uk** in **Domain**
* Enter your Imperial **Password**
* Click **Connect**

## Cinnamon (Mint 22.2)

* Open the file manager (**Files**) by clicking the icon
* From the **File** menu, click **Connect to Server..**
* In the window that appears, use the menu to change the **Type** to **Windows share**.
* Enter the following connection details:
    * **Server:** `rdf-active.ic.ac.uk`
    * **Share:** `research`
    * **Folder:** Either leave as "`/`" for the base of the file system or read the [Where are my files](./index.md#where-are-my-files) section to map a specific storage allocation.
    * **Domain Name:** Enter `ic.ac.uk`
    * **User name** and **Password:** Enter your Imperial username and password.
* Click **Connect**