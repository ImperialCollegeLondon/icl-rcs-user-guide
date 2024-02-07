# Using SSH

To access the HPC facility you would normally use a Secure Shell (or SSH) client on your computer to connect to the login nodes. You will need to be on the college network or connected to Unified Acecss in order to connect to the login nodes.

## Hostname for SSH

```console
login.hpc.imperial.ac.uk
```

Do note that due to security concerns, key-based authentication is disabled for the login-nodes. Users will need to login using  their college username and password.

## Linux and macOS

Linux and macOS users have the OpenSSH SSH client automatically installed on their operating system and this can be accessed from a terminal session.

Open a terminal session on your computer.
Run:

```console
ssh username@login.hpc.imperial.ac.uk
```

at the command prompt, substituting your own College username and entering your password when prompted.

## Windows
Windows users have a number of options when it comes to SSH clients, some of these are dependent upon the age of the operating system you are running.

### Putty

Putty is the SSH client traditionally used by many on Windows and is installed on managed College computers. If you are intending to use Putty on your own (personal or unmanaged) computer, please make sure you install the latest version and only download from [https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). If you already have Putty installed, please make sure you update your version until at least 0.72.

### OpenSSH

As of Window 10 (build 1809 and later) an OpenSSH client can be installed on Windows and is accessible via Powershell or Windows Terminal. You may need administrator access to your computer in order to install OpenSSH.

### Windows Subsystem for Linux

Windows Subsystem for Linux or WSL, enables you to run a Linux environment (such as Ubuntu or AlmaLinux) on your computer unmodified. WSL is available on Windows 10 build 1607 and later and installing it on your computer will require administrator access.

### MobaXterm

MobaXterm is a single application for Windows providing an SSH client, graphical sftp browser and X11 server (for remote graphics). If you decide to download the free (home version) of MobaXterm, please make sure you have read and are complying with the license before doing so.

## Running Graphical Applications

Some applications, for example, Matlab and Gaussview have a graphical user interface. You can still use these even when running on our systems although, please be aware of the following:

* Resource (compute, memory or filesystem IO) intensive applications should not be run on the login nodes.
* Unless your own computer is connected to a reasonably fast network, remotely using graphical applications may not be feasible.

In order to see the Graphical User Interface of these applications, you will need an X server running on your computer and you will need to enable X forwarding when you make the connection with the SSH client.

### Linux

All the linux distributions with a graphical desktop should have an X server running by default. You won't have to install anything extra.

### Windows

Install Xming from the Software Hub. If this is not feasible on your computer, the please install VcXSrv instead; the free version of Xming relatively old and not recommended. Please make sure Xming/VcxSrv are running before you connect to the HPC facility.

If you are using Windows Subsystem for Linux terminal, do "export DISPLAY=localhost:0.0" before following the instructions below.

### macOS

Download and install XQuartz. Ensure that it is running before following the instructions below.

### Prepare the connection
You need to connect to our systems with a special "Enable X11 forwarding" flag to allow graphical windows to be displayed on your screen. This will depend on your client/operating system.

#### Command-line SSH clients

On Linux and macOS you have to use a terminal to connect using a regular SSH client with the "Enable X11 forward" flag "-X".

Your connect command will look like:

```
ssh -X username@login.hpc.ic.ac.uk
```

#### Other SSH Clients

On all these clients you will have to look for the "Enable X11 forwarding" option.

For example on Putty, you will need to navigate to Connection -> SSH -> X11 and activate the "Enable X11 forwarding".

## Known Issues
### CheckHostIP Failure

If you are connecting via Zscaler then the IP address of the login nodes will appear to change due to the way Zscaler redirects traffic. This can cause issues with older versions of SSH. From v8.5 OpenSSH disabled IP checking by default which would prevent this error.


* ssh(1): disable CheckHostIP by default. It provides insignificant
benefits while making key rotation significantly more difficult,
especially for hosts behind IP-based load-balancers.

If you are on Linux, or use WSL, you can set `CheckHostIP no` in your `ssh_config`, it will avoid this problem.
