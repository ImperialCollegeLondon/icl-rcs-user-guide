# Globus

[Globus](https://www.globus.org/) is a high performance data transfer platform. The RCS run a Globus "endpoint" from which you can:

* **Transfer** large volumes of data between the RDS, your personal computer and Globus-accessible storage at other institutions
* **Share** RDS project allocation data with selected third parties, without requiring them to have a College account (Globus identity required)


## Logging in to Globus

Step 1: Log in to Globus

Visit [https://app.globus.org](https://app.globus.org), please select **Imperial College London** from the trusted organisations in the drop down list:

![Globus Login Page](./img/globus-login.jpg)

The first time you log in you will be asked to complete the registration for a Globus account.

If you have a username and password for any of the institutions you wish to transfer data with, repeat this process to [associate these other identities](https://app.globus.org/account/identities). You can also do this at any later time via "Link another identity" in the web application's Account settings.

Once logged in, you will see the Globus web application File Manager:

![Globus File Manager](./img/globus-file-manager.jpg)

From here you will be able to browse your files and folders in the different **collections**. An endpoint is a location where data resides. The RDS collection is called Imperial College London Research Data Store.

## Transferring Data

Step 1: Under [File Manager](https://app.globus.org/file-manager), in the collection field, select or search for a specific endpoint The RDS collection is called Imperial College London Research Data Store.

Step 2: Choose the files/folders you wish to transfer and on the right pane select "**Transfer or Sync to...**"

![Transferring data with Globus](./img/globus-transferring-data.jpg)

Step 3: This will open a second panel for the destination. Repeat the process to select a destination endpoint and folder.

Step 4: Optionally choose additional options for the transfer in the tab at the bottom. By default file integrity of transferred files will be checked after the copy is complete. All transfers to the RDS will be encrypted in transit.

![Transferring data with Globus options](./img/globus-transferring-data2.jpg)

Step 5: To commence the transfer, click "**Start**" A new transfer request will be created. Details on the transfer and progress can be monitored in the **Activity** tab.

Step 6: You will receive an email notification once the transfer has completed.

## Sharing Data in RDS Project Allocations

The information below is adapted from [https://docs.globus.org/how-to/share-files/](https://docs.globus.org/how-to/share-files/)

If you are the owner or delegated administrator of an RDS project allocation, you have the ability to selectively share data from your project allocations with anyone with a Globus account, without requiring them to have an Imperial College account. 

If the person you wish to share with already has an Imperial College account, then all you need to do is add them to the relevant [RDS access control group](https://selfservice.rcs.imperial.ac.uk/groups/manage/rds/). This will give them direct access as shown above. 

## Creating a Guest Collection

1. Head to [https://app.globus.org/file-manager](https://app.globus.org/file-manager) and make sure to login with your Imperial credentials.
1. In the "Collection" section at the top, search "Imperial" and select "Imperial College London Research Data Store".
1. Select the directory you want to share and press "Share" in the middle section.
1. There will be an option to "Add Guest Collection", choose that and select/modify any options as you see fit, then press "Create Collection".
1. You should then see an option to "Add Permissions - Share With", select that and you should then see the following options:

* **Path** the folder from which to start the sharing. This is relative to the base directory you specified earlier. The default is "/" - sharing everything in the base directory and below
* **Username or email** the person you wish to share with. This should be an institutional ID that the person can log in to Globus with.
* **Permissions** you may grant write access for the user. Note if you can't grant write access to files or folders that you yourself can't write.

6. You should then have a shareable link which you can send to the user you want to share the data with. The user should also be emailed with the link automatically.

If you are making several collections, you may find it convenient to create [Groups](https://app.globus.org/groups).

## Transfer data to your Personal Computer using Globus
For transferring data to your own computer, or another resource that doesn't have its own Globus endpoint, you will need to install [Globus Connect Personal](https://www.globus.org/globus-connect-personal). This creates a Globus endpoint on your personal computer, allowing you to move data using the Globus web app.

