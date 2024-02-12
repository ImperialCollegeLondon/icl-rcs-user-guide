# Access the RDS on Windows

The following instructions will show you how to map the RDS as a network drive on your own computer running Windows 10 or later.

!!! warning

    When you have mapped a network drive on your computer:

    * changing network
    * changing Wi-Fi HotSpot
    * disconnecting and reconnecting to the network
    
    may make your remote connection unstable, unusable or even make it fail (this may result in data corruption or even data loss, if you have open files). For this reason and others, we strongly advise users to **always disconnect the RDS mapped drive**, when their workflow has finished and re-map the drive when they need it again.

    Finally, please always make sure to unmap/disconnect any network drive (HPC, RDS etc..) **BEFORE changing your main IC account passwords**! This will prevent accessibility issues and avoid any Microsoft account profile related lockouts, due to cached passwords, expired credentials or stale connections.

## Mapping the RDS as a Network Drive
In order to map the RDS as a network drive on your computer, you must be connected to either the college network or otherwise following the advice on [accessing services remotely](../../remoteaccess.md). Once you are appropriately connected, please follow these steps (note there are a slight variation of steps between Windows 10 and Windows 11):

### Windows 10

* Go to [RDS Directory Paths](../paths.md) and make a note of the **RDS directory path (Windows)** you wish to access
* Click **Start** and type **This PC** and click on the icon
* Click **Computer** at the top of the page
* Click **Map Network drive**
* Select a **drive letter** in the box marked **Drive**. It can be any letter that does not have a path next to it already
* Enter the **RDS directory path (Windows)** in the box marked **Folder**,
    * For a home directory:  `\\rds.imperial.ac.uk\rds\user\your_user_name`
    * For a project: `\\rds.imperial.ac.uk\rds\project\your_project_name`
* Un-tick the **Reconnect at sign-in** tick box.
* Select **Connect using different credentials**
* Click the **Finish** button
* When prompted, enter your College **username** in the format **IC\yourusername** and your College **password** (you may need to click **More choices** followed by **Use a different account** if you are using a College managed computer). If you are unable to authenticate using these credentials but you are certain you have entered the correct password, please try again entering your College **username** as one of the following:  **IC\yourusername**, **yourusername**, **ic\yourusername**, **yourusername@ic.ac.uk**

### Windows 11

* Go to [RDS Directory Paths](../paths.md) and make a note of the **RDS directory path (Windows)** you wish to access
* Click **Start** and type **File Explorer** and click on the icon
* On the side-pane that shows your favourite drives and folders, scroll down until you see **This PC** and right-click the icon
* In the menu that appears, click **Show more options**
* Click **Map Network drive**
* Select a **drive letter** in the box marked **Drive**. It can be any letter that does not have a path next to it already
* Enter the **RDS directory path (Windows)** in the box marked **Folder**,
    * For a home directory:  `\\rds.imperial.ac.uk\rds\user\your_user_name`
    * For a project: `\\rds.imperial.ac.uk\rds\project\your_project_name`
* Un-tick the **Reconnect at sign-in** tick box.
* Select **Connect using different credentials**
* Click the **Finish** button
* When prompted, enter your College **username** in the format **IC\yourusername** and your College **password**. If you are unable to authenticate using these credentials but you are certain you have entered the correct password, please try again entering your College **username** as one of the following:  **IC\yourusername**, **yourusername**, **ic\yourusername**, **yourusername@ic.ac.uk**

