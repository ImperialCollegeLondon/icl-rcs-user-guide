# Cannot SSH to the HX1 cluster

HX1 is the new cluster facility at Imperial and at the time of writing this message, the cluster is in pre-production stage. We have reported this issue with the concerned team and this should be resolved soon.

When a user attempts to SSH on the HX1 cluster, they often face the following error message:-

```console
$ ssh username@login.hx1.hpc.ic.ac.uk
 
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:Om4DJD4qMehpKSmHK10xciHZvyLG92krJtYXumwHITw.
Please contact your system administrator.
Add correct host key in /home/username/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /home/username/.ssh/known_hosts:21
  remove with:
  ssh-keygen -f "/home/username/.ssh/known_hosts" -R "login.hx1.hpc.ic.ac.uk"
ECDSA host key for login.hx1.hpc.ic.ac.uk has changed and you have requested strict checking.
Host key verification failed.
```

**The issue arises because of two different SSH keys on the login nodes of HX1 cluster**. Please note that this is not because of the RCS team but an issue from our external partners. As we said above, we hope to resolve this soon.

For users, all they need to do is the step already suggested in the message above i.e. to remove the known ssh key with ssh-keygen command. In general, you need to do the following

```console
ssh-keygen -f "~/.ssh/known_hosts" -R "login.hx1.hpc.ic.ac.uk"
```

Please ensure to point the correct location of .ssh folder in the above command. Usually it is home directory and this is why we have used the ~ symbol in the front.

You may have to repeat this step if you encounter the same issue again in future or until the problem is solved permanently from our end.