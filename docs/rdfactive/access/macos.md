# Accessing the RDF-Active on macOS

The following instructions will show you how to access the RDF-Active by mapping it as a network drive on your Mac computer. To do this, you must be connected to the college network either directly or via [a private and secured remote connection](../../remoteaccess.md).

## Connecting your Mac to the RDS

1. Select **Go** from the **Finder menu**.
2. Select **Connect to Server...**
3. Enter the **RDF-Active** path you wish to connect in the **Server Address** box:
   * To access the RDF-Active's "root" directory, the Mac path is `smb://rdf-active.ic.ac.uk/research/`
    * To access a specific storage allocation's directory, review the [Where are my files](./index.md#where-are-my-files) section to identify the allocation's path.
4. Click **Connect**
5. When you asked to enter your name and password for the server "rdf-active.ic.ac.uk":
    * For the "Connect As:" field, enter "**Registered User**".
    * For the name field, enter your Imperial username in the **username@ic.ac.uk** format.
    * For the password, enter your Imperial password.
6. Click **Connect**.

!!! tip
    When you connect to an SMB share (for example, RDF-Active), macOS usually adds only the top-level share to the Finder sidebar, not the specific folder where your storage allocation lives. If you close the Finder window, getting back to that folder can be inconvenient (you might find it under *Go > Recent Folders*).

    To make it easy to return to your allocation, add the exact folder to the Finder sidebar:

    * Connect to the SMB share and navigate to your allocated folder.
    * With that folder open in Finder, go to *File > Add to Sidebar*.
    * Your allocation will now appear in the sidebar for quick access.
