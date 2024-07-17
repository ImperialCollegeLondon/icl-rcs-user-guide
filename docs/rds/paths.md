# RDS Directory Paths

The folder or directories you have access to on the RDS are dependent upon whether you are registered to use the HPC service, and whether you have access to any RDS project spaces. The following table is a guide on how to access these spaces, both when logged into the HPC service, and when [accessing from your own personal computer](./access/index.md).

| Location | Who has access | RDS Directory Path | RDS Directory Path on HPC |
| -------- | -------------- | ------------------ | ------------------------- |
| RDS Project Space | RDS project space members<br>(including registered HPC users) | `\\rds.imperial.ac.uk\rds\project\<project-name>` (Windows)<br>`smb://rds.imperial.ac.uk/rds/project/<project-name>/` (macOS and Linux) | `/rds/general/project/<project-name>` |
| [Home Space](../hpc/getting-started/data-management-on-hpc.md#home-directory) | HPC Users | `\\rds.imperial.ac.uk\rds\user\<yourusername>\home` (Windows)<br>`smb://rds.imperial.ac.uk/rds/user/<yourusername>/home` (macOS and Linux) | `/rds/general/user/<yourusername>/home` |
| [Ephemeral Space](../hpc/getting-started/data-management-on-hpc.md#ephemeral-directory) | HPC Users | `\\rds.imperial.ac.uk\rds\user\<yourusername>\ephemeral` (Windows)<br>`smb://rds.imperial.ac.uk/rds/user/<yourusername>/ephemeral` (macOS and Linux) | `/rds/general/user/<yourusername>/ephemeral` |
| RDS Project Space<br>(alternative for HPC users) | HPC users who are<br>members of an RDS project space | `\\rds.imperial.ac.uk\rds\user\<yourusername>\projects\<project-name>` (Windows)<br>`smb://rds.imperial.ac.uk/rds/user/<yourusername>/projects/<project-name>` (macOS and Linux) | `/rds/general/user/<yourusername>/projects/<project-name>` |