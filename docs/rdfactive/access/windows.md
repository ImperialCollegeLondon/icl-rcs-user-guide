# Access the RDF-Active on Windows

The following instructions will show you how to map the RDS as a network drive on your own computer running Windows 11.

!!! warning

    When you have mapped a network drive on your computer:

    * changing network
    * changing Wi-Fi HotSpot
    * disconnecting and reconnecting to the network
    
    may make your remote connection unstable, unusable or even make it fail (this may result in data corruption or even data loss, if you have open files). For this reason and others, we strongly advise users to **always disconnect the RDS mapped drive**, when their workflow has finished and re-map the drive when they need it again.

    Finally, please always make sure to unmap/disconnect any network drive (RDF-Active etc.) **BEFORE changing your main IC account passwords**! This will prevent accessibility issues and avoid any Microsoft account profile related lockouts, due to cached passwords, expired credentials or stale connections.

## Mapping the RDF-Active as a Network Drive

In order to map the RDF-Active as a network drive on your computer, you must be connected to either the college network or otherwise following the advice on [accessing services remotely](../../remoteaccess.md). Once you are appropriately connected, please follow these steps:

### Windows 11

* Click **Start** and type **File Explorer** and click on the icon
* On the side-pane that shows your favourite drives and folders, scroll down until you see **This PC** and right-click the icon
* In the menu that appears, click **Map Network drive**. If this is not visible, click the **Show more options**
* Select a **drive letter** in the box marked **Drive**. It can be any letter that does not have a path next to it already
* Enter the **RDF-Active** path you wish to connect to in the box marked **Folder**,
    * For the "base" path of the RDF-Active  `\\rdf-active.ic.ac.uk\research\`
    * Or read the [Where are my files](./index.md#where-are-my-files) section to map a specific storage allocation. 
* Un-tick the **Reconnect at sign-in** tick box.
* Select **Connect using different credentials**
* Click the **Finish** button
* When prompted, enter your College **username** in the format **username@ic.ac.uk** and your College **password**.