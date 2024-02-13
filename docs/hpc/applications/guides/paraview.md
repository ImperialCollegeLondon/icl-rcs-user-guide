# Paraview

!!! warning

    We believe these instructions do not work with the recommended remote access method as described [here](../../../remoteaccess.md). We are currently exploring options as to how Paraview can continue to be used remotely. 

Use [Paraview](https://www.paraview.org/) to interactively visualise and analyse large datasets using our computing resources.

Paraview runs in a client-server mode: the data is held and processed by a paraview server running inside a batch job which connects to a graphical client running on your personal computer. 

## Configuring the Paraview Client

First download and install the Paraview client for your computer, either from the [Paraview download site](https://www.paraview.org/download/) or from the [Imperial College software hub](https://www.imperial.ac.uk/admin-services/ict/self-service/computers-printing/devices-and-software/get-software/software-hub/).

Once Paraview is running:

* choose **File-Connect...** or click the **connect icon** directly.
* In the window that opens select "**Add Server**". Fill in the options:

```
Name: Imperial RCS
Server Type: Client/Server (reverse connection)
Port: 11111
```

* press **Configure**.
* Set the **Startup Type** to **Manual**
* Click **Save**
* Highlight the configuration you just created and click **Connect**

Your client should open a pop-up window with a message saying **Waiting for server to connect**. In subsequent use simply select **Imperial RCS** from the **Connect menu**.

## Starting the Paraview Server

The Paraview server must be run as a batch job. Before you can submit the job you will need to configure it with the IP address of the computer where you are running the client. If you are connected to the campus network using OpenVPN then you get find your IP address by opening the OpenVPN dialog and noting down **YOUR PRIVATE IP (IPV4)**. Otherwise you can look it up online.

If this is the first time that you are running Paraview, ensure that any firewall running on your computer has TCP port 11111 open.

Note that when you go to **Open** ... a file from your client you will see the filesystem as the server sees it. Either have your data on the RDS or have your job script copy the data to **$TMPDIR**.

Finally, submit a job using the following template:

```bash
#!/bin/bash
#PBS -lselect=1:ncpus=16:mem=64gb
#PBS -lwalltime=4:0:0
module load paraview/5.10.0
mpiexec pvserver --reverse-connection --client-host=IPADDRESS
```

replacing **IPADDRESS** with the IP you just determined, and optionally tailoring ncpus and mem to match your needs.

As soon as the job starts the Paraview Server will automatically connect to your Client. You may be presented with the message:

```
Display is not accessible on the server side. Remote rendering will be disabled.
```

Click OK to continue and if everything goes OK your client should now be ready for your work.

If this fails, double-check your firewall settings, and - if you are outside the college network - retry after having connected to the VPN service.