## Software

This section will explain how to access software that has been centrally installed on HX2.

Please make sure any software you run on HX2 has been optimised for the hardware. Ideally use software provided by us (which has already been optimised) or otherwise please make sure to use relevant optimisation flags when compiling your own software. Please avoid simply copying binaries from other systems such as CX3, unless the software is commercial and/or only the binary is available.

### Loading Applications

Loading modules/applications on HX2 is similar to that on CX3 except it is not necessary to load the "production" modules (`tools/prod`). Please refer to our main [Loading Applications](../applications/index.md) page for advice on how to load modules on HX1.

### EasyBuild

Most of the software installed on HX2 is done so using the EasyBuild software installation system and will have been optimised for the hardware. Please see our [EasyBuild](../applications/easybuild.md) page for more information.

### Python and Conda Environments

#### Installing miniforge

[Miniforge](https://github.com/conda-forge/miniforge) is a minimal installer for conda that is tailored to use the conda-forge channel by default. It installs both the conda and mamba tools for managing your environments.

The following instructions are based on those found on the Miniforge repository by have been adapted for use on HX1.

```console
[username@hx2a04login01 ~]$ curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 89.3M  100 89.3M    0     0  93.1M      0 --:--:-- --:--:-- --:--:--  194M
```

You should end up with a file called `Miniforge3-Linux-x86_64.sh`. You can then run the installer with:

```console
[username@hx2a04login01 ~]$ bash Miniforge3-Linux-x86_64.sh

Welcome to Miniforge3 25.3.0-3

In order to continue the installation process, please review the license
agreement.
Please, press ENTER to continue
>>>
```

After accepting the license, you will be asked to confirm where to install Miniforge3. The default location of `miniforge3` in your home directory is fine for most circumstances.

Once the files have finished unpacking, you will be asked:

```console
Do you wish to update your shell profile to automatically initialize conda?
This will activate conda on startup and change the command prompt when activated.
If you'd prefer that conda's base environment not be activated on startup,
   run the following command when conda is activated:

conda config --set auto_activate_base false

You can undo this by running `conda init --reverse $SHELL`? [yes|no]
[no] >>>
```

We strongly advise that you do not update your shell profile i.e. respond with "no".

You can now enable miniforge in your environment by running the shell hook:

```console
[username@hx2a04login01 ~]$ eval "$(/hx2-weka/home/username/miniforge3/bin/conda shell.bash hook)"
(base) [username@hx2a04login01 ~]$
```

You can then use the conda commands as usual. Please see our [Conda](../applications/guides/conda.md) page for more information on using conda.
