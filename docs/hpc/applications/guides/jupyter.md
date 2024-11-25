# Jupyter

The [Jupyter Notebook](https://jupyter.org/) enables users to create documents that combine live code with narrative text, mathematical equations, visualizations, interactive controls, and other rich output. It also provides building blocks for interactive computing with data: a file browser, terminals, and a text editor. With the evolution of Jupyter notebook to Jupyter Lab, this expands most of the existing features which enable you to use text editors, terminals, data file viewers, and other custom components side by side with notebooks in a tabbed work area.

## JupyterHub

[JupyterHub](https://jupyter.org/hub) is a web service enabling multiple users to run their Jupyter Notebooks on shared resources. The Imperial College RCS team provide a JupyterHub service which runs interactive Jupyter notebook sessions on HPC hardware by running them as "batch" jobs. The URL for the JupyterHub service is:

https://jupyter.rcs.imperial.ac.uk/

You must be registered with the HPC service in order to use the JupyterHub service.

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