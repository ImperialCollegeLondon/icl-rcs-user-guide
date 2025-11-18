# RDF-Active on macOS

The following instructions will show you how to connect your Mac to the RDF-Active so you can access the files. You must be connected to either the college network or otherwise following the advice on [accessing services remotely](../../remoteaccess.md) before you can connect to the RDS.

## Connecting your Mac to the RDS

* Select **Go** from the **Finder menu**
* Select **Connect to Server...**
* Enter the **RDF-Active** path you wish to connect in the **Server Address** box:
    * For the "base" path of the RDF-Active  `smb://rdf-active.ic.ac.uk/research/`
    * Or read the [Where are my files](./index.md#where-are-my-files) section to map a specific storage allocation.
* Click **Connect**
* When you asked to enter your name and password for the server "rdf-active.ic.ac.uk":
    * Select to *Connect As:* **Registered User**
    * In the Name: field, enter your Imperial username as **username@ic.ac.uk**
    * Enter your Imperial password in the *Password* field.
* Click **Connect**

!!! tip
    When you connect to an SMB share (for example, RDF-Active), macOS usually adds only the top-level share to the Finder sidebar, not the specific folder where your storage allocation lives. If you close the Finder window, getting back to that folder can be inconvenient (you might find it under *Go > Recent Folders*).

    To make it easy to return to your allocation, add the exact folder to the Finder sidebar:

    * Connect to the SMB share and navigate to your allocated folder.
    * With that folder open in Finder, go to *File > Add to Sidebar*.
    * Your allocation will now appear in the sidebar for quick access.
