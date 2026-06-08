# Accessing the RDF-Active on Windows 11

The following instructions will show you how to access the RDF-Active by mapping it as a network drive on your Windows 11 computer. To do this, you must be connected to the college network either directly or via [a private and secured remote connection](../../remoteaccess.md).

!!! warning

    When you have mapped a network drive on your computer the following activities may make your remote connection unstable, unusable or even make it fail (this may result in data corruption or even data loss, if you have open files).:

    * changing network
    * changing Wi-Fi HotSpot
    * disconnecting and reconnecting to the network
    
    For this reason and others, we strongly advise users to **always disconnect the RDF-Active mapped drive**, when their workflow has finished and re-map the drive when they need it again.

    Finally, please always make sure to unmap/disconnect any network drive (RDF-Active etc.) **BEFORE changing your main IC account passwords**! This will prevent accessibility issues and avoid any Microsoft account profile related lockouts, due to cached passwords, expired credentials or stale connections.

## Mapping the RDF-Active as a Network Drive

1. Click **Start** and type **File Explorer** and click on the icon.
2. On the side-pane that shows your favourite drives and folders, scroll down until you see **This PC** and right-click the icon.
3. In the menu that appears, click **Map Network drive**. If this is not visible, click the **Show more options**.
4. Select a **drive letter** in the box marked **Drive**. It can be any letter that does not have a path next to it already.
5. Enter the **RDF-Active** path you wish to connect to in the box marked **Folder**.
    * To access the RDF-Active's "root" directory, the Windows 11 path is `\\rdf-active.ic.ac.uk\research\`
    * To access a specific storage allocation's directory, review the [Where are my files](./index.md#where-are-my-files) section to identify the allocation's path.
6. Un-tick the **Reconnect at sign-in** tick box.
7. Select **Connect using different credentials**.
8. Click the **Finish** button.
9. When prompted, enter your College **username** in the format **username@ic.ac.uk** and your College **password**.