# FreeSurfer

[FreeSurfer](https://surfer.nmr.mgh.harvard.edu/fswiki) is a software package for the analysis and visualization of structural and functional neuroimaging data from cross-sectional or longitudinal studies.

People have experienced some issues in running this software on our cluster. We explain the issues and the solutions below.

## Library not found error

Like most of our software on the cluster, we have installed Freesurfer version 7.4.1 using [Easybuild](https://docs.easybuild.io/). However, the installation seems to have an issue as it does not install the dependent `libgfortran.so.5` library, which is required to run FreeSurfer or other associated packages.

For example, if you load the FreeSurfer module and run the following command:

```bash
# Load the module
$ module load FreeSurfer/7.4.1-centos8_x86_64

# Use one of the binaries in the module
$ mri_robust_register -all-info
```

You will get the following error.

```bash
mri_robust_register: error while loading shared libraries: libgfortran.so.5: cannot open shared object file: No such file or directory
```

An easy workaround to remove this error is to load a suitable GCC module which provides the missing library. You can do this by running the following command:

```bash
$ module load GCC/13.2.0
```

After loading the GCC module, if you run the same command, you will get the following.

```bash
7.4.1

ProgramName: mri_robust_register  ProgramArguments: mri_robust_register -all-info  ProgramVersion: 7.4.1  TimeStamp: 2025/09/03-08:53:49-GMT  BuildTime: Jun 13 2023 23:42:21  BuildStamp: freesurfer-linux-centos8_x86_64-7.4.1-20230613-7eb8460  User: user Machine: cx3-1-10.cx3.hpc.ic.ac.uk  Platform: Linux  PlatformVersion: 4.18.0-477.27.1.el8_8.x86_64  CompilerName: GCC  CompilerVersion: 8.5.0
```

## FreeSurfer job not running in parallel using OpenMP

In some cases, we have come across issues where people are trying to run their FreeSurfer jobs in parallel using OMP. However, they did not experience any speedup and the code takes almost the same time as if it was running serially.

We looked through several online resources to investigate the issue and discovered that the `-parallel` flag needs to be used alongside the `-openmp` flag. Without the `-parallel` flag, the job will not utilize multiple threads effectively. For more details, please see [https://afni.nimh.nih.gov/pub/dist/doc/htmldoc/tutorials/fs/fs_fsprep.html](https://afni.nimh.nih.gov/pub/dist/doc/htmldoc/tutorials/fs/fs_fsprep.html).

Thus, your `recon-all` command (one of most basic command of FreeSurfer) would look like

```bash
recon-all -s <subject_id> -i <input_nifti_file> -all -qcache -parallel -openmp <num_threads>
```

## FreeSurfer array job not running

Recently, we have come across an issue where some people were trying to run FreeSurfer as an array job and they were getting the following error in all of their sub-jobs.

```bash
rca-config: No match.
```

We investigated this issue and noted that the error is occurring because of how the `TMPDIR` environment variable is being set by the PBS and how the FreeSurfer parses it. In an array job, the `TMPDIR` variable gets assigned with some special characters (like `[]`) which was causing issues.

An easy workaround is to set the `TMPDIR` variable to a different location in your job script. For example, you can set it to your home directory or any other directory where you have write permissions.

```bash
mkdir -p $HOME/FreeSurferTempDir
export TMPDIR=$HOME/FreeSurferTempDir
```

This will create a temporary directory in your home directory and set the `TMPDIR` variable to that location, which should resolve the issue.
Please remember to clean up the temporary directory after your job is done to avoid using up your disk quota.
