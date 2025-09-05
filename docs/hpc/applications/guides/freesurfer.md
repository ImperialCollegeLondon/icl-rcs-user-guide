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

ProgramName: mri_robust_register  ProgramArguments: mri_robust_register -all-info  ProgramVersion: 7.4.1  TimeStamp: 2025/09/03-08:53:49-GMT  BuildTime: Jun 13 2023 23:42:21  BuildStamp: freesurfer-linux-centos8_x86_64-7.4.1-20230613-7eb8460  User: lragta  Machine: cx3-1-10.cx3.hpc.ic.ac.uk  Platform: Linux  PlatformVersion: 4.18.0-477.27.1.el8_8.x86_64  CompilerName: GCC  CompilerVersion: 8.5.0
```

## FreeSurfer job not running in parallel using OMP

In some cases, we have come across issues where people are trying to run their FreeSurfer jobs in parallel using OMP. However, they did not experience any speedup and the code takes almost the same time as if it was running serially.

We explored some pages on the internet to figure out the issue and found that you have to use `-parallel` flag in addition to the `-openmp` flag. Without the `-parallel` flag, the job will not utilize multiple threads effectively. For more details, please see https://afni.nimh.nih.gov/pub/dist/doc/htmldoc/tutorials/fs/fs_fsprep.html.

Thus, your `recon-all` command (one of most basic command of FreeSurfer) would look like

```bash
recon-all -s <subject_id> -i <input_nifti_file> -all -qcache -parallel -openmp <num_threads>
```
