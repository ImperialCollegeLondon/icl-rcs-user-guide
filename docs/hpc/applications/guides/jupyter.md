# Jupyter

The [Jupyter Notebook](https://jupyter.org/) enables users to create documents that combine live code with narrative text, mathematical equations, visualizations, interactive controls, and other rich output. It also provides building blocks for interactive computing with data: a file browser, terminals, and a text editor. With the evolution of Jupyter notebook to Jupyter Lab, this expands most of the existing features which enable you to use text editors, terminals, data file viewers, and other custom components side by side with notebooks in a tabbed work area.

## JupyterHub

[JupyterHub](https://jupyter.org/hub) is a web service enabling multiple users to run their Jupyter Notebooks on shared resources. The Imperial College RCS team provide a JupyterHub service which runs interactive Jupyter notebook sessions on HPC hardware by running them as "batch" jobs. The URL for the JupyterHub service is:

[https://jupyter.cx3.rcs.ic.ac.uk](https://jupyter.cx3.rcs.ic.ac.uk)

You must be registered with the HPC service in order to use the JupyterHub service.

!!! warning
	If you are prompted with an "Internal Server Error" when trying to sign in, you should first login to any Microsoft product such as Outlook or Office, with your Imperial account in the same browser window, then try to sign in again.

## Moving to the new Jupyterhub service

If you're coming from our older system using our [older jupyter service](https://jupyter.rcs.imperial.ac.uk/) you may notice a few differences.

This service better manages resource usage. Both memory and CPU limits are controlled and restricted to the amount of resource that you request when you start your job. Should you go over these limits, you will see a message that your kernel has been killed and that it will restart. If you need more resource, you can request more in the server spawn menu, up to 8 cores and 64GB of RAM. If you need even more resource than this, you should submit a batch job.

!!! info
	If you cannot see previously created Conda environments on the new service, please see the [following section below](#using-previously-created-environments-on-the-new-service)

### Stopping a session
To stop a session, you need to go to File > Hub Control Panel > Stop My Server. The session might take a few seconds to properly end.

## Load custom Packages/Modules in JupyterHub

Whilst the default kernels available in Jupyter, whether Python or R, offer a certain number of base packages, users normally want to use their own custom packages within their programs. The good news is that Jupyter integrates perfectly with conda and its package and environment management systems.

The recommended way to work with python and R packages is via conda. This also enables users to create segregated environments for different projects. More information on how to use conda environments can be found [here](./conda.md).

### Python

More information on how to use Python with conda can be found in the [Python application guide](./python.md).

To use custom python modules within a Jupyter lab session:

**On login node run**

* Load module
<br>`eval "$(~/miniforge3/bin/conda shell.bash hook)"`
* Setup a new conda environment for this project. I named this environment "test1" as an example. * Note that I specify a couple of starting packages to be installed: ipykernel and python with a specific version.<br>
`conda create -n test1 python=3.9 ipykernel jupyter_client`
* Activate the environment:<br>
`source activate test1`
* Install desired packages:<br>
`conda search "package_name"`<br>
`conda install package_name[=version]`
* Install python kernel for Jupyter:<br>
`python -m ipykernel install --user --name python39_test1 --display-name "Python3.9 (test1)"`

**On JupyterHub**

1. Start a new [Jupyter Hub](#jupyterhub) session.
1. Select the new "Python3.9 (test1)" icon in the Jupyter Launcher.

This will enable you to access all python modules in the conda environment created. In this case "test1".

### R

More information on how to use R with Anaconda can be found in the R application guide. For R-studio please use Open OnDemand.

To use custom R libraries within a Jupyter lab session:

**On login node run:**

* Load module:<br>
`eval "$(~/miniforge3/bin/conda shell.bash hook)"`
* Setup a new conda environment for this project. I named this environment "ir413" as an example. * Note that I specify a couple of starting packages to be installed: r-irkernel and R with a specific version.<br>
`conda create -n ir413 r-base=4.1.3 r-irkernel jupyter_client -c conda-forge`
* Activate the environment:<br>
`source activate ir413`
* Install desired packages:<br>
`conda search "package_name"`<br>
`conda install package_name[=version] -c conda-forge`
* Install R kernel for jupyter, run R:<br>
`R`

**In R run:**

```R
IRkernel::installspec(user = TRUE)
IRkernel::installspec(name = 'ir413', displayname = 'R 4.1.3 (ir413)')
```

Where "R 4.1.3" is the display name desired to show in the Jupyter launcher.

**Jupyter Hub:**

1. Start a new Jupyter Lab session.
1. Select the new "R 4.1.3 (ir413)" icon in the Jupyter Launcher.
1. This will enable you to access all R libraries in the conda environment created. In this case "test2" with a custom R version of 4.1.3.

## Connecting VSCode to a Jupyterhub session

It's possible to use VSCode to connect to a running JupyterHub session. Please follow the steps below to do so:

#### JupyterHub Steps
1. Login to [JupyterHub](https://jupyter.cx3.rcs.ic.ac.uk/) and navigate to the [token page](https://jupyter.cx3.rcs.ic.ac.uk/hub/token)
1. Set a note and token expiration if you want, then press "Request new API token". Make a note of this and store it securely. If you lose it, or it expires, delete it and create a new one.
1. Start up a Jupyter session with the necessary resource requirements from [here](https://jupyter.cx3.rcs.ic.ac.uk/hub/home)

#### VSCode Steps
1. Open VSCode and install the official Microsoft [Jupyter](https://marketplace.visualstudio.com/items/?itemName=ms-toolsai.jupyter) and [JupyterHub](https://marketplace.visualstudio.com/items/?itemName=ms-toolsai.jupyter-hub) extensions 
1. Create a new notebook by opening the command palette (Ctrl+Shit+P on Windows, Cmd+Shift+P on MacOS), then type in and select the option "Create: New Jupyter Notebook"
1. Open command palette again and search for the option "Notebook: Select Notebook Kernel" > "Select Another Kernel" > "Existing JupyterHub Server".
1. Then paste in the url [https://jupyter.cx3.rcs.ic.ac.uk](https://jupyter.cx3.rcs.ic.ac.uk) 
1. As prompted enter your Imperial username, then paste in the token you previously copied. You may then be prompted to set the name of the server.
1. Select your kernel from the drop down. That's it, you're done!

If you have any issues with that, you can [get in touch with us](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-support/contact-us/).

## Using previously created environments on the new service

We have identified that at some point the behaviour of Jupyter kernels have changed. As of now, kernels are stored in `.local` inside your `$HOME` directories meaning some Conda environments that may have worked in the [previous jupyter](https://jupyter.rcs.imperial.ac.uk/) will not immediately carry over to the new one. 

If you cannot see your previously created environments on the new Jupyterhub service, do this:

First, activate the environment you wish to use:

```console
[user@login ~]$ eval "$(~/miniforge3/bin/conda shell.bash hook)"
[user@login ~]$ conda activate python39_jupyter
```

Then run the following:
```console
[user@login ~]$ python -m ipykernel install --user --name python39_torch_env --display-name "Python3.9 (torch)"
```
You can of course change `--display-name` to whatever you'd like, and the `--name` should be the same as your Conda environment name.
You should now be able to see this kernel after refreshing the page or restarting your session.

