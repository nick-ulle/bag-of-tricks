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



Initial Setup
-------------

This section describes how to get set up quickly in a new (remote) computing
environment.

Create an `~/env` directory to store files related to the computing
environment:

```sh
mkdir ~/env
```

Create an `~/env/bin` directory to store binaries:

```sh
mkdir ~/env/bin
```

This directory will be added to the `PATH` environment variable by `~/.profile`
in the config files.


### Miniconda

Download the [Miniconda][] install script:

[miniconda]: https://docs.conda.io/en/latest/miniconda.html

```sh
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

Run the script to install Miniconda to the `~/env` directory:

```sh
chmod +x Miniconda3-latest-Linux-x86_64.sh
./Miniconda3-latest-Linux-x86_64.sh
```

Add the `conda-forge` to the channels list:

```sh
conda config --prepend channels conda-forge
```

:::{tip}
In the command above, you can use `--append` instead of `--prepend` to give
conda-forge lower priority than the official Anaconda channels.
:::


### Mamba

Install [mamba][], a faster alternative to conda, in the `base` environment:

[mamba]: https://github.com/mamba-org/mamba

```sh
conda install -n base mamba
```

:::{note}
The [Conda User Guide says][no-base]:

> You don't want to put programs into your base environment, though.

This is because [the `base` environment is where conda itself is
installed][so-base], so if it breaks, your entire conda install breaks.

Nevertheless, mamba must be installed in `base` in order to install packages to
the correct directories.

[no-base]: https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html#managing-environments
[so-base]: https://stackoverflow.com/a/56504279/1166039
:::


### The `main` Environment

Create and activate the `main` environment:

```sh
conda create -n main
conda activate main
```

The `main` environment is where you should install tools applicable to almost
all projects. For instance, tmux and stow:

```sh
mamba install tmux stow
```

And modern shell tools:

```sh
mamba install exa fd-find ripgrep sd
```

:::{note}
Modern shell tools replace older tools that have been standards for decades.
They improve usability and efficiency, and add new features. My preferred
modern shell tools are:

* [exa][] replaces `ls`
* [fd][] replaces `find`
* [ripgrep][] replaces `grep`
* [sd][] replaces `sed` and `awk`

[exa]: https://github.com/ogham/exa
[fd]: https://github.com/sharkdp/fd
[ripgrep]: https://github.com/BurntSushi/ripgrep
[sd]: https://github.com/chmln/sd
:::

Optionally, install Python and R as well:

```sh
# Python & R
mamba install python ipython jupytext pandas
mamba install r
```

:::{tip}
You can add `conda activate main` to `~/.bashrc` to make the shell activate
this environment by default on login.
:::


### Config Files

Clone the [dotfiles-remote][] repo:

[dotfiles-remote]: https://github.com/nick-ulle/dotfiles-remote

```sh
git clone https://github.com/nick-ulle/dotfiles-remote.git
```


### Neovim

Get the [Neovim AppImage][neovim] and make a softlink in `~/env/bin` to the
executable.

For shorter deployments, just alias `nvim` to `vim` in `~/.bashrc`.

[neovim]: https://github.com/neovim/neovim/wiki/Installing-Neovim#appimage-universal-linux-package


### Git Workflow

Set up a repository on GitHub and push some commits. Log into the server and
create a `~/bare_repos` directory:

```sh
mkdir bare_repos
```

```sh
cd bare_repos
mkdir dotfiles-remote
```

In the local repo (on your machine) add the server as a remote:

```sh
git remote add datasci nulle@datasci:~/bare_repos/dotfiles-remote
```

Then:

```sh
git push datasci main
```

```
git clone --branch main bare_repos/dotfiles-remote .dotfiles
cd .dotfiles
#git checkout main
```


