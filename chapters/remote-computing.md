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



Shell Tips & Tricks
-------------------

Redirect all output to a file:

```sh
./script.sh 2>&1 > output.log
```

See [this StackOverflow post](https://stackoverflow.com/a/818284).

Display output on screen and save to a file:

```sh
./script.sh 2>&1 | tee output.log
```
