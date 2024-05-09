# Rclone

[Rclone](https://rclone.org/) is a command line program for transferring data to/from cloud storage facilities such as Microsoft OneDrive and Sharepoint. Rclone is our recommended utility for transferring data between the RDS and OneDrive/Sharepoint.

Rclone is already installed on the HPC server as a [module](../../hpc/getting-started/accessing-software.md). Users will also need a copy of Rclone on their local machine for the authorisation step but this only needs to be done once and then it can be removed.

!!! warning
    Please do **NOT** use the **mount** option with Rclone. Due to the way the RDS filesystem is setup, if you try to mount the file system with Rclone, the only way you/we can unmount the filesystem is to reboot the server on which you established the mount.

## Accessing Rclone on the HPC login nodes

Rclone is already installed on the HPC service as a module. It can be loaded into your environment in the following way:

```console
$ module load rclone
For setting up rclone for Onedrive checkout: https://rclone.org/onedrive/
$ rclone config
2024/05/09 10:30:50 NOTICE: Config file "/rds/general/user/username/home/.config/rclone/rclone.conf" not found - using defaults
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q>
```

## Securing Your RClone Configuration

Your Rclone configuration on the HPC service will be found in `/rds/general/user/username/home/.config/rclone/rclone.conf`. Once you have configured a remote cloud service, this file will contain access credentials to that cloud service. We therefore strongly recommend that you encrypt this file, particularly when it is stored on a shared file system such as the RDS. The configuration file may be encrypted in the following way:

1. Load the rclone module on the login node.

```console
$ module load rclone
For setting up rclone for Onedrive checkout: https://rclone.org/onedrive/
```

2. Run the rclone config command to start the configuration wizard.

```
$ rclone config
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q>
```

3. Enter "s" to run the "Set configuration password" command

```console 
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q> s
```

4. Enter "a" to add a password to your configuration file. Make sure to provide a sufficiently long and complex password when requested and to store this password in a safe space such as a password manager (for more advice on passwords see [ICT Passwords page](https://www.imperial.ac.uk/admin-services/ict/self-service/be-secure/passwords-and-extra-security/passwords/)).

```console
Your configuration is not encrypted.
If you add a password, you will protect your login information to cloud services.
a) Add Password
q) Quit to main menu
a/q> a
Enter NEW configuration password:
password:
Confirm NEW configuration password:
password:
Password set
```

5. Once you've set a password you can enter "q" to return to the main menu.

```console
Your configuration is encrypted.
c) Change Password
u) Unencrypt configuration
q) Quit to main menu
c/u/q> q

No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q>
```

Every time you now run an rclone command such as transferring data to/from a remote service, you will be prompted to enter the configuration password.

## Configuring Rclone for OneDrive

Full guidance of using the Microsoft OneDrive connector with rclone may be found on the [OneDrive](https://rclone.org/onedrive/) page of the rclone website. The following steps assist with using the OneDrive connector with your Imperial College OneDrive account. These steps should be run on the login nodes.

First, load the rclone module by running the following command:

```console
module load rclone
```

This will make the rclone command available to use in the terminal.

Start the configuration wizard with:

```console
rclone config
```

This will launch a configuration wizard that will guide you through the process of setting up a new remote connection. At the time of writing the steps are:

1. Type "n" to create a new remote.
2. Give a name to your remote, for example, "onedrive".
3. Type the number corresponding to select "Microsoft OneDrive".
4. Leave "client_id" and "client_secret" blank by pressing enter.
5. "n" for no to advanced config
6. "n" for auto config

At this point you will need to generate an access token on another machine. The login nodes are unable to do this as it requires a web browser to setup the OAUTH2. The simplest method is to use your local machine, everything can be removed once an OAUTH2 token has been generated.

1. Open a web browser on your local machine and navigate to [https://rclone.org/downloads/](https://rclone.org/downloads/).
2. Download the correct version of your local machine, for Microsoft Windows laptops/desktops supplied by Imperial college you should download the "Intel/AMD - 64 Bit" version under "Windows".
3. Extract the contents of the zip file and make a note of the location. 
4. rclone is a command line tool so on Windows we will need to open it via "Command Prompt"
    1. Press the Windows key + R
    2. In the text box type "cmd" and press enter
    3. Use the "cd" command just as on the login nodes to navigate to the local folder/directory where you saved the contents of the rclone zip. Normally this is in "C:\Users\your_IC_username\Downloads" or similar
    4. run the following

```console
rclone authorize "onedrive"
```

5. rclone will open a window in your web browser and ask you to authorise the connection. Click Authorise.
6. In the command prompt, rclone will generate an access token. Copy everything between "Paste the following into your remote machine -->" and "<---End paste" back into the original rclone window on the login node
7. Choose number 1 for "OneDrive Personal or Business"
8. It should find you Imperial College drive, which should be 0 (zero). Chose drive 0 and press enter.
9. The returned drive "root" should point to "https://imperiallondon-my.sharepoint.com/personal/your_college_username_ic_ac_uk/Documents" if that is correct press "y" and enter
10. Finally another "Yes this is Ok". You should now see the configured remote. 

```console
Current remotes:
 
Name                 Type
====                 ====
OneDrive             onedrive
```

## Configuring Rclone for SharePoint

Configuring Rclone to access a Sharepoint site uses the same [OneDrive connector](https://rclone.org/onedrive/) as for [Microsoft OneDrive](#configuring-rclone-for-onedrive). There are some small differences in the settings which are detailed below.

First, load the Rclone module by running the following command:

```console
module load rclone
```

This will make the `rclone` command available to use in the terminal.

Start the configuration wizard with:

```console
rclone config
```

This will launch a configuration wizard that will guide you through the process of setting up a new remote connection. At the time of writing the steps are:

1. Type "n" to create a new remote.
2. Give a name to your remote, for example, "sharepoint".
3. Type the number corresponding to select "Microsoft OneDrive".
4. Leave "client_id" and "client_secret" blank by pressing enter.
5. "n" for no to advanced config
6. "n" for auto config

At this point you will need to generate an access token on another machine. The login nodes are unable to do this as it requires a web browser to setup the OAUTH2. The simplest method is to use your local machine, everything can be removed once an OAUTH2 token has been generated.

1. Open a web browser on your local machine and navigate to [https://rclone.org/downloads/](https://rclone.org/downloads/).
2. Download the correct version of your local machine, for Microsoft Windows laptops/desktops supplied by Imperial college you should download the "Intel/AMD - 64 Bit" version under "Windows".
3. Extract the contents of the zip file and make a note of the location. 
4. rclone is a command line tool so on Windows we will need to open it via "Command Prompt"
    1. Press the Windows key + R
    2. In the text box type "cmd" and press enter
    3. Use the "cd" command just as on the login nodes to navigate to the local folder/directory where you saved the contents of the rclone zip. Normally this is in "C:\Users\your_IC_username\Downloads" or similar
    4. run the following

```console
rclone authorize "onedrive"
```

5. rclone will open a window in your web browser and ask you to authorise the connection. Click Authorise
6. In the command prompt, rclone will generate an access token. Copy everything between "Paste the following into your remote machine -->" and "<---End paste" back into the original rclone window on the login node
7. Choose the option to "Search for a Sharepoint site"
8. At the prompt search, for something that will match the desired Sharepoint site.
9. Look through the list generated and type in the number on the left of the desired sharepoint site
10. If there is only one drive select 0 (zero) otherwise select the drive that matches the location of your data
11. Finally another "Yes this is Ok". You should now see the configured remote. 

```console
Current remotes:
 
Name                 Type
====                 ====
OneDrive             onedrive
sharepoint           onedrive
```

## Moving data between OneDrive/Sharepoint and the RDS

First, load the rclone module by running the following command:

```console
module load rclone
```

This will make the rclone command available to use in the terminal. Here we will assume you configured rclone as given by the instructions above. The commands given are the same for OneDrive and sharepoint, you just need to change the name of the remote. 

To copy a file from OneDrive to the RDS run:

```console
rclone copy OneDrive:source_path /rds/general/user/<username>/home/
```

The RDS path can be replaced with home, ephemeral or even a project. 

If you want to copy an entire folder and all its contents, use the recursive flag:

```
rclone copy OneDrive:source_path /rds/general/user/<username>/home/ --recursive
```

Further advice on copying files may be found in the [Rclone documentation](https://rclone.org/docs/).