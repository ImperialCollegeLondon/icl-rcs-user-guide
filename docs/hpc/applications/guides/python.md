# Python

The default Python that you will find on our systems is fine for trivial use. For anything demanding, however, you will need to use a different version of Python. The sections below how to use different versions of python on the HPC facility. If you are unable to use one of the methods below to install the python modules you need, please [raise a ticket with us](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-support/contact-us/).

## Easybuild Python

We use [Easybuild](../easybuild.md) to provide an optimized installation that users can build upon. To see these modules you will need to load the production tools

```console
module load tools/prod
```

You can then see all the versions of Python available

```console
module spider Python
```

The versions called "bare" provide very little and we generally recommend users avoid them. If you are looking to use that standard scientific packages like NumPy, SciPy, and matplotlib then you only need to load a version of the SciPy-bundle. Loading this will automatically load the required version of Python. For example,

```console
module load SciPy-bundle/2022.05-foss-2022a
```

These versions of Python also include virtualenv (venv) to allow users to create custom environments. For example, to create an environment called "example-env"

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
It's important to remember that if you create a `venv` with any of the given modules such as `SciPy-bundle/2022.05-foss-2022a`, then you will need to load that same module before you can activate the environment.

## Conda and Python

If you are already familiar with package managers like Anaconda, this may be your best option in terms of getting your environment (set of python modules) working on the HPC facility. To begin, follow the instructions in the [Conda application guide](./conda.md) to setup Conda for your account. Assuming you have done this, the following set of instructions would create an environment called `py39` containing Python 3.9, and would install `numpy` in that environment.

```console
eval "$(~/miniforge3/bin/conda shell.bash hook)"
conda create -n py39 python=3.9 numpy
source activate py39
```
This will:

1. Get Conda ready for use with the `eval` command.
1. Create a Conda environment called `py39` with Python 3.9 and the latest version of `numpy` compatible with it.
1. Activate the environment so that it's ready to be used.

!!! info
	To prevent dependency conflicts in Conda, we recommend installing everything in one go as we did in the 2nd line above, rather than creating your environment and installing one package at a time. You can read more about this on our [Conda Best Practices Page](./conda.md#conda-best-practices).

### Conda interoperability with pip

If you cannot find a package you need from a conda channel, you may need to try a conventional `pip install`.

Conda and pip have historically had difficulties getting along.  Pip didn't respect Conda’s environment constraints, while Conda was all too happy to clobber pip-installed software. Conda 4.6.0 adds preview support for better interoperability. With this interoperability, Conda can use pip-installed packages to satisfy dependencies, and can even remove pip-installed software cleanly and replace them with Conda packages when appropriate.

This feature is disabled by default right now because it can significantly impact Conda’s performance.  If you’d like to try it, you can set this condarc setting:

```console
conda config --set pip_interop_enabled True
```

Where previous versions of Conda will show a confusing ambiguity on exactly what’s present in `conda list` if you installed a pip package when it was already present in the environment. Conda 4.6 now shows only one correct entry.

## Memory Profiling Python

Profiling is an essential technique in software development and optimization. It involves analysing the execution time, memory usage, and other performance-related aspects of a program to identify bottlenecks and areas for improvement. In Python, a common library for memory profiling is memory_profiler. This uses psutil under the hood but gives a much similar interface and the output is given line-by-line.

### memory_profiler
memory_profiler is a library specifically designed to profile memory usage in Python scripts. It allows you to track memory consumption line-by-line, helping you identify memory-intensive sections of your code.

#### Installation
You can install memory_profiler directory from pip. We recommend installing the library within a virtualenv or [conda environment](#conda-and-python). 

```console
python3 -m pip install memory-profiler
```

#### Usage

```python
# importing the library
from memory_profiler import profile
 
# instantiating the decorator
# code for which memory has to
# be monitored
 
@profile
def func():
    x = [1] * (10 ** 7)
    y = [2] * (4 * 10 ** 8)
    y = y[:int(len(y)/2)]
    del x
    return y
 
if __name__ == '__main__':
    func()
```

In this example, the @profile decorator from memory_profiler is used to profile the function func. When the script is executed, memory_profiler will display memory usage for each line within the function.

```console
Line #    Mem usage    Increment  Occurrences   Line Contents
=============================================================
     9     17.2 MiB     17.2 MiB           1   @profile
    10                                         def func():
    11     93.6 MiB     76.4 MiB           1       x = [1] * (10 ** 7)
    12   3145.4 MiB   3051.8 MiB           1       y = [2] * (4 * 10 ** 8)
    13   1619.5 MiB  -1525.8 MiB           1       y = y[:int(len(y)/2)]
    14   1543.2 MiB    -76.3 MiB           1       del x
    15   1543.2 MiB      0.0 MiB           1       return y
```

We can see when extra memory was allocated and when it was cleared. This is clearly a very useful tool but it should be noted that running profiling can slow down code so it should not be left on all the time. 
