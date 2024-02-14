# Same or Previous Code not working

This is one of the most common type of message or ticket that we receive at RCS. In almost all cases, users say that their code is not working now for some reasons which has been working perfectly until a few days back. Most often, users say that they have not made any changes in the code, input files or their workflows.

When we talk to these users, we notices some small changes introduced by them either intentionally or unintentionally while they were trying something new. We regret to inform that such type of tickets are most difficult to resolve and are most time consuming from our end. We do not have any strategy that we can mention on this page which may help to avoid the issues altogether. This is because the issue may arise from several different reasons which can be summarized here.

Instead, the idea of this page is to provide some guidelines for the user to create a Minimal reproducible example which definitely helps a lot in identifying and resolving an issue.

### Minimal Reproducible Example

A minimal reproducible example is the one which mimics some/all the features of the faulty job under consideration with a major difference that it requires significantly less amount of  computational resources and finishes quickly. In order for creating such an example, please follow the guidelines below (all points may not be valid for everyone):-

* **Separate Directory**:- Please create a folder titled RCS_help in your home directory or TMPDIR and let us know where you have created this directory.
* **Source code and Executable**:- If your workflow involves compilation every single time and we too have to compile the code, please make sure that all the source code files are present in the folder. If you are confident about your executable, please copy the same in the RCS_help folder.
* **Input Files**:- Please make sure that all input files are present in the folder itself. This is because we will be running your code from this folder and we do not want to cause any unintentional damage to your files.
* **Submission script**:- Please provide a submission script for your job so that we can run the same and try to resolve the issue.
* **Wall time**:- Make sure that the job finishes quickly. A job running for 2 to 10 minutes is fine. Please note that lower the wall time required for the job to finish, the easier and quicker it will be for us to sort out the issue.
* **Computational Resources**:- Please ensure that the sample job requires as less resources as possible. Ideally, we expect a total of 1 to 10 cores approximately.
* **Output Files**:- Please tell us what is meant by successful run of the job. We are only interested in the metrics or files associated with the run that can indicate a successful run and we are not interested to know anything about the physics, chemistry etc. behind your output. For example, for one user a successful run may mean completing 100 iterations while for other user a successful run may involve the presence of some output file such as output.txt.

Finally, if you remember even slightly the changes that you have made or the steps you took for trying something new, please let us know. We are here to help and with your help, we may achieve it faster.