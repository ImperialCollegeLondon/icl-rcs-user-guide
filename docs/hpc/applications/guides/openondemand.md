# Open OnDemand

[Open OnDemand](https://openondemand.org/) (OOD) is a service that enables interactive applications to run as "batch" jobs on a normal HPC facility; these applications are then accessed via a web browser. The OOD service at Imperial is primarily focused on R and Rstudio. The URL for the OOD service hosted by the Imperial College RCS team is:

[https://openondemand.rcs.ic.ac.uk/](https://openondemand.rcs.ic.ac.uk/)

You must be registered with the HPC service in order to use the OOD service.

We recommend you setup a [Conda](./conda.md) environment when using RStudio; these instructions demonstrate how to do this.

## Creating an Conda Environment

We recommend you familiarise yourself with the use of Conda and R by going to the [R application page](./R.md). You will then need to prepare your Conda environment by first connecting to the login node using SSH (see the [Getting started](../../getting-started/index.md) guide for advice on doing this).

From there, load Anaconda and create a new environemnt.

```console
module load anaconda3/personal
anaconda-setup
conda create -n Renv r-base=4.1.2 -c conda-forge
```

The anaconda-setup command will only need to be run the first time you load anaconda3/personal. The name after "-n" is a unique name for this environment. Once that completes and other packages can be installed via Anaconda. This environment can then be activated with:

```console
source activate Renv
(Renv)
```

**Users should not install packages using package.install as this may lead to conflicts.**

For example, to install tidyverse,

```console
(Renv) conda install -c conda-forge r-tidyverse
```

To find packages, either search on the [Anaconda website](https://anaconda.org/search) or use the following replacing "package_name" with the name of the R package to be installed,

```
(Renv) conda search "package_name"
```

Once all packages have been install OOD can be started.

## Starting Open OnDemand
Open a web browser and go to [https://openondemand.rcs.ic.ac.uk/](https://openondemand.rcs.ic.ac.uk/). Log in with your university username and password if needed. 

To start a new session click on **Interactive Apps** at the top of the page and select 

![OOD Interactive Apps](img/ood-interactive-apps.png)

Once the page load it will look something like the following,

![OOD RStudio Launch](img/ood-rstudio-launch.png)

From the Resources drop down list select the most appropriate instance. Then in the text box below that labelled "Conda Environment Name" input the name of the environment generated above. In this example that would be "Renv". Then click launch. The job will then join a queue. Please be patient while your job sits in queue. The wait time depends on the number of cores and time requested as well as how busy the queue is. Once is starts a button labelled "Connect to RStudio Server" should appear.

Clicking on that will start up Rstudio in a new window using the created Anaconda environment. 

![OOD RStudio Connect](img/ood-rstudio-connect.png)

## Installing packages on Open OnDemand
Once the above has been followed, users can install new packages via the Rstudio Terminal.

**Users should not install packages using Cran or the packages.install command.**

At the top left of the screen select the tab called Terminal. This will drop the user in to a bash screen on the compute node the job is running. This can be useful for running monitoring command like top or free but here it will be used to install packages. 

![OOD RStudio Terminal](./img/ood-rstudio-terminal.png)

First the R environment needs to be loaded, which is done in a similar way to before.

```console
module load anaconda3/personal
source activate Renv
```

Packages can then be installed. For example, to install tidyverse,

```console
(Renv) conda install -c conda-forge r-tidyverse
```

To find packages, either search on the [Anaconda website](https://anaconda.org/search) or use the following replacing "package_name" with the name of the R package to be installed,

```console
(Renv) conda search "package_name"
```

The session doesn't need to be reloaded once packages are installed, simply switch back to the Console tab and load them as normal. This process only needs to be done once to install the required packages, they will persist for future sessions.

## Clearing browser cache

Open on-Demand uses single sign-on to allow users to connect without having to input their password every time. However this can sometime become corrupted, preventing users from even loading the OOD home page. If issue can be identified if you are able to login via an Incognito or private browsing window but not via your normal browser. If closing all browser windows doesn't help then you may have to clear your browser cache. Below is some guidance on how to do this for the major browsers.

### Clearing browser cache on Firefox:

1. Open Firefox and click on the menu button (three horizontal lines) in the top-right corner.
1. Select "Options" (Windows) or "Preferences" (Mac).
1. In the left sidebar, click on "Privacy & Security."
1. Scroll down to the "Cookies and Site Data" section.
1. Click on the "Clear Data" button.
1. Ensure that the "Cached Web Content" option is selected.
1. Click on the "Clear" button.
1. Restart Firefox to complete the process.

### Clearing browser cache on Microsoft Edge:

1. Open Microsoft Edge and click on the menu button (three horizontal dots) in the top-right corner.
1. Select "Settings."
1. Under the "Clear browsing data" section, click on "Choose what to clear."
1. Check the box next to "Cached data and files."
1. Optionally, you can select other types of data you want to clear.
1. Click on the "Clear" button.
1. Restart Microsoft Edge to complete the process.

### Clearing browser cache on Google Chrome:

1. Open Google Chrome and click on the menu button (three vertical dots) in the top-right corner.
1. Select "Settings."
1. If needed, scroll down and click on "Advanced" to expand the advanced settings.
1. Under the "Privacy and security" section, click on "Clear browsing data."
1. Select "All time" to clear all cache.
1. Ensure "Cookies and other site date" is selected
1. Click on the "Clear data" button.
1. Restart Google Chrome to complete the process.