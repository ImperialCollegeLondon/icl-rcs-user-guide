# Conda

[Conda](https://docs.conda.io/en/latest/index.html) is a package, dependency and environment management system. There are a couple of different versions avaible but we recommend using [conda-forge](https://conda-forge.org/download/). It is one of the ways for users to manage their own environments and supports a wide range of languages: Python, R, Ruby, Lua, Scala, Java, JavaScript, C/ C++, FORTRAN.

Using different conda environments for different projects/applications is highly recommended and certainly offers many advantages:

* conda environments integrate management of different Python versions, including installation and updating of Python itself. On comparison, virtualenvs must be created upon an existing, externally managed Python executable.
* conda environments can track non-python dependencies; for example seamlessly managing dependencies and parallel versions of essential tools like LAPACK or OpenSSL
* Rather than environments built on symlinks – which break the isolation of the virtualenv and can be flimsy at times for non-Python dependencies – conda-envs are true isolated environments within a single executable path.


## Using conda
If its the first time loading you will need to install miniforge. We have created a simple helper script which will install everything or update your installtion if you already have miniforge installed. To get started run the following on any login node:

```console
[user@login ~]$ module load miniforge/3
[user@login ~]$ miniforge-setup
```

You only need to run this once for your user account to install miniforge into your home directory. Once complete you can load conda:

```console
[user@login ~]$ eval "$(~/miniforge3/bin/conda shell.bash hook)"
```

You should use this command any time you wish to load conda. 

If you're using a different distribution of Conda such as `anaconda3` or `miniconda` you can simply replace `miniforge3` in the command above with whatever you are using.

One last thing to note, we recommend creating a new environment for each workflow and to **never** modify the base environment as this can break the conda installation. The best way to prevent this is to disable the automatic loading of the base environment by running:

```console
[user@login ~]$ conda config --set auto_activate_base false
```

## Using Mamba

This above also loads [mamba](https://mamba.readthedocs.io/en/latest/user_guide/mamba.html) into your environment. Mamba is similar to conda but can be a lot quicker and is able to handle more complex installations. Which you use is mostly up to you and your workflow. 

Mamba can be used exactly as you would use Conda. All you need to do differently is use the `mamba` command instead of `conda`. For example: `mamba create` and `mamba install`

## Anaconda licensing

Unfortunately due to changes in the Anaconda license users need to manually disable the *defaults* channel and instead use conda-forge. This shouldn't affect most users as most package developers focus on conda-forge anyway. To make this change please run the following:

```console
[user@login ~]$ conda config --remove channels defaults
[user@login ~]$ conda config --add channels conda-forge
```
To see more about this change, please look below at the [How to disable the defaults channel page](#how-to-disable-the-defaults-channel)

## Conda Best Practices

### Resolving Dependencies
Conda can sometimes have issues resolving dependencies, which is where it tries to find versions of packages that are compatible with one another. With potentially hundreds of packages, this can take a long time. [Mamba](#using-mamba) does a better job at this, but still can't solve everything.

In order to prevent this from happening, we recommend that when you create a Conda environment, you also do your best to install all the packages you need in one go. Here's a simple example of why:

1. Suppose you want to install `Python 3.9` and `package-a v2.0` into a new environment.
1. You start by creating the environment and installing `Python 3.9` This will install the latest version of available of `Python 3.9`. <br /> `conda create -n py39 python=3.9`
1. Now, you want to install `package-a v2.0` into that environment as well. <br /> `conda install package-a=2.0`
1. You get an error saying `package-a requires Python<=3.9.6`

This has occurred because when you did `conda create -n py39 python=3.9`, it installed the latest version of `Python 3.9`, which is `3.9.22`. However, the program `package-a v2.0` requires something older than or equal to `Python 3.9.6`.

As such, the way to avoid this, would be to install both packages in one go, as Conda would see that `package-a v2.0` requires 3.9.6 or earlier, and would install that instead of the latest version:

```console
[user@login ~]$ conda create -n py39 python=3.9 package-a=2.0
```
### Base Conda Environment

Avoid install anything into your base Conda environment. Conda itself and its core dependencies are installed here so modifying them can introduce dependency conflicts or break Conda. So, always create a Conda environment and install your packages into there.

### Updating Conda

Avoid updating Conda itself unless it's very old and is causing issues/missing features. This is because it could cause further dependency issues or be incompatible with some packages.

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
The majority of these commands can be used with Mamba as well.

## How to disable the defaults channel

Conda uses a list of channels to download and install packages. Some of the most common channels include `conda-forge`, `bioconda` etc. If you do not specify any channel, it uses the `defaults` channel by default.

The list of channels is usually stored in a file called `.condarc`. `.condarc` is a configuration file used by Conda to store user-defined settings, such as:

* Channels (where Conda looks for packages)
* Proxy settings
* Default environments
* Custom paths
* Other Conda behaviors

You can see the list of all channels in your `.condarc` file by using the following command

```console
[user@login ~]$ conda config --show channels
```

For example, running in my home directory may give output like

```console
channels:
  - defaults
  - conda-forge
  - bioconda
```

Anaconda recently updated its licensing terms meaning we cannot use the `defaults` channel. We need to remove this channel from all our `.condarc` files. For some users who may have installed their own version of conda, they may have the `.condarc` file in multiple locations. They can use the following command to see the location of such files.

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

For most of the users, the file will be in their home directory. To remove the `defaults` channel, they can use the following command.

```console
[user@login ~]$ conda config --remove channels defaults
```

This will remove the `defaults` channel from your list of channels. To see if `defaults` channel was actually removed, please run this command again.

```console
# Input command
conda config --show channels

# Output
channels:
  - conda-forge
  - bioconda
```

You should not see the `defaults` channel now as shown above.

If you would like to manually delete/edit the file, you can do so by using your favourite editor such as vim, nano etc. For example, you can use

```console
[user@login ~]$ vim /full_path_to_condarc_file
```

Please delete the `defaults` channel, save your file and exit. Please make sure that you do this for all your `.condarc` files.
