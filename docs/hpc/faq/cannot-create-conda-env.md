# Cannot create a new Conda Environment

Some of users have faced issues creating a new conda environment. Let us suppose a user is trying to create an environment named `my_env`. Assuming you've followed the [Conda setup guide](../applications/guides/conda.md#using-conda) The user will use the commands:

```console
#Load the modules.
eval "$(~/miniforge3/bin/conda shell.bash hook)"
 
#Create the new environment
conda create -n my_env
```

The user gets a long error message as shown below (only a portion of message is shown for brevity).

```console
Collecting package metadata (current_repodata.json): done
Solving environment: done
 
# >>>>>>>>>>>>>>>>>>>>>> ERROR REPORT <<<<<<<<<<<<<<<<<<<<<<
 
    Traceback (most recent call last):
      File "/rds/general/user/username/home/miniforge3/lib/python3.7/site-packages/conda/gateways/repodata/__init__.py", line 132, in conda_http_errors
        yield
      File "/rds/general/user/username/home/miniforge3/lib/python3.7/site-packages/conda/gateways/repodata/__init__.py", line 101, in repodata
        response.raise_for_status()
      File "/rds/general/user/username/home/miniforge3/lib/python3.7/site-packages/requests/models.py", line 1021, in raise_for_status
        raise HTTPError(http_error_msg, response=self)
    requests.exceptions.HTTPError: 404 Client Error: Not Found for url: https://conda.anaconda.org/imperial-college-research-computing/linux-64/current_repodata.json
```

The last line of the error message shown above is quite useful as it gives us an hint on how to resolve the same. The error arises because of an entry imperial-college-research-computing in the condarc configuration file (the file which contains list of channels where conda should look for while downloading and installing packages.

To solve this issue, we only need to remove this entry from the above file. We can use the following command for that purpose

```console
conda config --remove channels imperial-college-research-computing
```

After running the above command, you should be able to create your environment.
