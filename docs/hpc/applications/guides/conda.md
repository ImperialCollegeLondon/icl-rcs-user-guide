# Conda

!!! info

    This page has not yet been rewritten for CX3 Phase 2.

[Conda](https://docs.conda.io/en/latest/index.html) is a package, dependency and environment management system. There are a couple of different versions avaible but we recommend using [conda-forge](https://conda-forge.org/download/). It is one of the ways for users to manage their own environments and supports a wide range of languages: Python, R, Ruby, Lua, Scala, Java, JavaScript, C/ C++, FORTRAN.

Using different conda environments for different projects/applications is highly recommended and certainly offers many advantages:

* conda environments integrate management of different Python versions, including installation and updating of Python itself. On comparison, virtualenvs must be created upon an existing, externally managed Python executable.
* conda environments can track non-python dependencies; for example seamlessly managing dependencies and parallel versions of essential tools like LAPACK or OpenSSL
* Rather than environments built on symlinks – which break the isolation of the virtualenv and can be flimsy at times for non-Python dependencies – conda-envs are true isolated environments within a single executable path.


## Using conda
If its the first time loading you will need to install miniforge. We have created a simple helper script which will install everything or update your installtion if you already have miniforge installed. To get started run the following on any login node:

```
module load miniforge/3
miniforge-setup
```

You only need to run this once for your user account to install miniforge into your home directory. Once complete you can load conda,

```
eval "$(~/miniforge3/bin/conda shell.bash hook)"
```

You should use this command any time you wish to load conda. This also loads [mamba](https://mamba.readthedocs.io/en/latest/user_guide/mamba.html) into your environment. Mamba is similar to conda but can be a lot quicker and is able to handle more complex installations. Which you use is mostly up to you and your workflow. 

One last thing to note, we recommend creating a new environment for each workflow and to never modify the base environment as this can break the conda installation. The best way to prevent this is to disable the automatic loading of the base environment by running:

```
conda config --set auto_activate_base false
```

## Conda Table of Commands

| Conda basic commands | COMMAND |
| -------------------- | ------- |
| Get a list of all my environments, active environment is shown with * | conda env list |
| Create a new environment named py39, install Python 3.9 | conda create -n py39 python=3.9 |
| Activate an environment | source activate ENV_NAME |
| Deactivate an environment | conda deactivate |
| List all packages and versions installed in active environment | conda list |
| Search the Anaconda repository for a package | conda search PACKAGENAME |
| Install a package included in Anaconda | conda install PACKAGENAME |
| Remove unused packages and caches | conda clean |

For a full list of commands please see [conda commands](https://docs.conda.io/projects/conda/en/stable/commands/index.html) provided by Anaconda.

## Anaconda3/personal

Long term users will remember the anaconda3/personal module and the attached documentation. This method relied on packages provided by Anaconda Inc. but in 2020 they changed their terms of service requiring all companies with more than 200 employess buy a commercial license. As most workflows on HPC use the conda-forge repo anyway we are moving to using the open source product provided by the same team that manage those repos. Users can continue to use their original environments if they install miniforge but they will have to be activated using the full path. For example, if you installed [Tensorflow](./tensorflow.md) following the old documentation you can still activate that envornment with the following:

```
eval "$(~/miniforge3/bin/conda shell.bash hook)"
conda activate /rds/general/user/username/home/anaconda3/envs/tf2_env
```

If you are still using the old Anaconda3/personal module can we please ask that you [Disable the "defaults" channel](#how-to-disable-the-defaults-channel)  as this may require the University to purchase a license for every user. More information on Anaconda's terms of service can be found at: [https://www.anaconda.com/pricing/terms-of-service-faqs](https://www.anaconda.com/pricing/terms-of-service-faqs)

## How to disable the defaults channel

Conda uses a list of channels to download and install packages. Some of the most common channels include `conda-forge`, `bioconda` etc. If you do not specify any channel, it uses the `defaults` channel by default.

The list of channels is usually stored in a file called `.condarc`. `.condarc` is a configuration file used by Conda to store user-defined settings, such as:

* Channels (where Conda looks for packages)
* Proxy settings
* Default environments
* Custom paths
* Other Conda behaviors

You can see the list of all channels in your `.condarc` file by using the following command

```bash
conda config --show channels
```

For example, running in my home directory may give output like
```bash
channels:
  - defaults
  - conda-forge
  - bioconda
```

As mentioned in the section [Anaconda3/personal](#anaconda3personal), anaconda recently updated its licensing terms and we cannot use the defaults channel. We need to remove this channel from all our `.condarc` files. For some users who may have installed their own version of conda, they may have the `.condarc` file in multiple locations. They can use the following command to see the location of such files.

```bash
# Input Command
conda config --show-sources

# Output of the above command
==> /rds/general/user/lragta/home/.condarc <==
auto_activate_base: False
channel_priority: strict
channels:
  - defaults
  - conda-forge
  - bioconda

```

For most of the users, the file will be in their home directory. To remove the defaults channel, they can use the following command.

```bash
conda config --remove channels defaults
```

This will remove the `defaults` channel from your list of channels. To see if `defaults` channel was actually removed, please run this command again.

```bash
# Input command
conda config --show channels

# Output
channels:
  - conda-forge
  - bioconda
```

You should not see the `defaults` channel now as shown above.

If you would like to manually delete/edit the file, you can do so by using your favourite editor such as vim, nano etc. For example, you can use

```bash
vim /full_path_to_condarc_file
```

Please delete the `defaults` channel, save your file and exit. Please make sure that you do this for all your `.condarc` files.