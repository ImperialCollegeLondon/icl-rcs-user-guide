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

We investigated this issue and noted that the error is occurring because of how the `TMPDIR` environment variable is being set by the PBS and how the FreeSurfer parses it. 

In an array job, the `TMPDIR` variable gets assigned with some special characters (like `[]`) which was causing issues. For example, the `TMPDIR` may look something like:

```bash
/var/tmp/pbs.1148088[1].pbs-7
#where 1148088 is the job ID and [1] is the array index.
```

An easy workaround is to set the `TMPDIR` variable to a different location in your job script. We will be using the location `/var/tmp` because it is local to a compute node and is not shared across nodes. The main advantage of using a local storage space is for the applications which are I/O intensive and may read/write lots of small files. Such jobs may cause issues to RDS. Therefore, we are using the `/var/tmp` location. For more details, please see [Temporary Job Space](../../../hpc/getting-started/data-management-on-hpc.md#temporary-job-space).

Below, we give a complete script that can allow you to run your FreeSurfer job as an array job.

```bash
#!/bin/bash
#PBS -l walltime=0:01:00
#PBS -l select=1:ncpus=4:mem=32gb
#PBS -N freesurfer_batch_array
#PBS -J 1-2
#PBS -o /rds/general/user/abc123/home/My_Logs
#PBS -e /rds/general/user/abc123/home/My_Logs

module load FreeSurfer/7.4.1-centos8_x86_64
INPUT_DIR=/rds/general/user/abc123/home/freesurfer_input

# Create an array of all .nii files
files=("$INPUT_DIR"/*.nii)

# Select the file for this job array index
nii_file="${files[$PBS_ARRAY_INDEX-1]}"  # Bash arrays are 0-indexed

# Extract subject name
subj=$(basename "$nii_file" .nii)

echo "PBS assigned tmpdir is = " $TMPDIR

#Replace square brackets with underscores to avoid parsing issues.
FREESURFER_TMP="${TMPDIR//[\[\]]/_}"
echo "$FREESURFER_TMP"

#Make Freesufer_tmp as new tmpdir
export TMPDIR=$FREESURFER_TMP

#Create the directory
mkdir -p "$FREESURFER_TMP"

#Export directory for output. This is where output for one subject will be stored based on the job index. 
export SUBJECTS_DIR=$FREESURFER_TMP

#Copy input file/s to that directory.
cp $nii_file $FREESURFER_TMP

#Change to that directory
cd $FREESURFER_TMP

echo "Starting recon-all for subject: $subj"
#Run the freesurfer command
recon-all -s "$subj" -i "$nii_file" -all -qcache -parallel -openmp 8
echo "Finished subject: $subj"

#Copy the data back to the home directory
cp -r $FREESURFER_TMP /rds/general/user/abc123/home/Results

#Delete the temporary directoy created.
rm -rf $FREESURFER_TMP
echo "Job ended"
```

The line `FREESURFER_TMP="${TMPDIR//[\[\]]/_}"` replaces any square brackets in the `TMPDIR` path with underscores, ensuring that FreeSurfer can parse the path correctly. We later assign the modified path to the `TMPDIR` variable.

After this line, the new `TMPDIR` would look like

```bash
/var/tmp/pbs.1148088_1_.pbs-7
```

Please also note that since the `TMPDIR` is not the one created by the PBS, we need to create and delete the directory manually in the script. With this workaround, you should be able to run your FreeSurfer jobs as an array job without any issues.
