Remote Computing
================

This chapter is a collection of workflows, tips, and tricks for remote
computing.



Farm
----

You can launch a 4-hour interactive session with:

```
srun --partition=med --time=4:00:00 --unbuffered --pty /bin/bash -il
```

The `-il` argument to bash makes the shell an interactive login shell.


View partitions available for use:

```sh
sacctmgr show associations where user=nulle  | less -S
```

### GPU Nodes

The DataLab GPU slice is partition `gpu-a100-h` and is only accessible from the
`datalabgrp` account. You can launch a 4-hour interactive session on the
DataLab GPU slice with:

```
srun --account=datalabgrp --partition=gpu-a100-h --gres=gpu:1 \
  --time=4:00:00 --unbuffered --pty /bin/bash -il
```

The `--gres=gpu:NUM` argument sets the number of GPUs available, and is
necessary to use a GPU on any of Farm's GPU-enabled nodes. On nodes with more
than 1 GPU, `NUM` can be set to the number of GPUs needed.


### Running Jupyter/RStudio Remotely

Use SLURM to launch an interactive session as described in the previous
sections.

**On the allocated node**, get the hostname:

```sh
hostname -f | cut -d "." -f1
```

Then launch Jupyter:

```sh
jupyter lab --no-browser --ip=HOSTNAME --port=8888
```


The port can be pretty much any large number.

**On your local machine**, use `ssh` to forward local connections to the port
to the allocated node on Farm:

```sh
ssh farm -N -L 8888:HOSTNAME:8888
```

The `-L` argument sets up forwarding and the `-N` argument prevents `ssh` from
opening a login shell.

You can then access Jupyter at <http://localhost:8888/>.


Git Workflow
------------

On your computer, set up a local repository and make some commits.

On the remote server, create a `~/bare_repos` directory:

```sh
mkdir bare_repos
```

Then initialize a bare repo for the repo you want to work with on the server:

```sh
cd bare_repos
git init --bare REPO_NAME
```

In the local repo on your computer, add the bare repo on the server as a remote
repo:

```sh
git remote add farm nulle@farm:~/bare_repos/REPO_NAME
```

Then push to the remote repo:

```sh
git push farm main
```

Finally, on the server, clone the bare repo so that you can work on it:

```
git clone --branch main bare_repos/REPO_NAME REPO_NAME
#git checkout main
```

Now you can commit and push on the server. To pull commits from the server to
your computer, run:

```
git pull farm main
```
