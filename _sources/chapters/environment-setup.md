Environment Setup
=================

This chapter describes how to get set up quickly in a new (remote) computing
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


Conda
-----

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


Config Files
------------

Clone the [dotfiles-remote][] repo:

[dotfiles-remote]: https://github.com/nick-ulle/dotfiles-remote

```sh
git clone https://github.com/nick-ulle/dotfiles-remote.git
```


Neovim
------

Get the [Neovim AppImage][neovim] and make a softlink in `~/env/bin` to the
executable.

For shorter deployments, just alias `nvim` to `vim` in `~/.bashrc`.

[neovim]: https://github.com/neovim/neovim/wiki/Installing-Neovim#appimage-universal-linux-package
