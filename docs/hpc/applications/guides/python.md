# Python

The default Python that you will find on our systems is fine for trivial use. For anything demanding, however, you will need to use a different version of Python. The sections below how to use different versions of python on the HPC facility. If you are unable to use one of the methods below to install the python modules you need, please view the Support and Training page for information on raising a ticket for help from us.

## Easybuild Python

We use [Easybuild](../easybuild.md) to provide an optimized installation that users can build upon. To see these modules you will need to load the production tools

```console
module load tools/prod
```

You can then see all the versions of Python available

```console
module av Python
```

The versions called "bare" provide very little and we generally recommend users avoid them. If you are looking to use that standard scientific packages like NumPy, SciPy, and matplotlib then you only need to load a version of the SciPy-bundle. Loading this will automatically load the required version of Python. For example,

```console
module load SciPy-bundle/2022.05-foss-2022a
```

These versions of Python also include virtualenv to allow users to create custom environments. For example, to create an environment called "example-env"

```console
mkdir ~/venv
virtualenv ~/venv/example-env
```

We can then activate this virtual environment with

```console
source ~/venv/example-env/bin/activate
```

Extra packages can then be added using pip.

```console
python3 -m pip install mypy
```

To return to the base environment use

```
deactivate
```

### Example PBS job

Once your environment has been created you will only need to load it in the job. The below example PBS script requests 1 CPU, 4 GBs of ram for 1 hours and then loads "example-env" from before.

```bash
#!/bin/bash
#PBS -l walltime=1:0:0
#PBS -lselect=1:ncpus=1:mem=4gb

module load tools/prod
module load SciPy-bundle/2022.05-foss-2022a
source ~/venv/example-env/bin/activate

cd $PBS_O_WORKDIR

python3 some_python_script.py
```

## Anaconda Python

If you are already familiar with Anaconda Python, this may be your best option in terms of getting your environment (set of python modules) working on the HPC facility. To begin, following the instructions in the [Conda application guide](./conda.md) to setup Conda for your account. Assuming you have done this, the following set of instructions would create an environment called py39 containing python 3.9, and would install numpy in that environment.

```console
module load anaconda3/personal
conda create -n py39 python=3.9
source activate py39
(py39) conda search numpy
(py39) conda install numpy
```

### Conda interoperability with pip

If you cannot find a package you need from a conda channel, you may need to try a conventional `pip install`.

Conda and pip have historically had difficulties getting along.  Pip didn't respected Conda’s environment constraints, while Conda was all too happy to clobber pip-installed software. Conda 4.6.0 adds preview support for better interoperability. With this interoperability, Conda can use pip-installed packages to satisfy dependencies, and can even remove pip-installed software cleanly and replace them with Conda packages when appropriate.

This feature is disabled by default right now because it can significantly impact Conda’s performance.  If you’d like to try it, you can set this condarc setting:

```console
conda config --set pip_interop_enabled True
```

Where previous versions of Conda will show a confusing ambiguity on exactly what’s present in “conda list” if you installed a pip package when it was already present in the environment. Conda 4.6 now shows only one correct entry.