# Nick's Bag of Tricks

[top]: #nicks-bag-of-tricks


## Contributing

The course reader is a live webpage, hosted through GitHub, where you can enter
curriculum content and post it to a public-facing site for learners.

To make alterations to the reader:

1.  Pull changes from upstream with `git pull`.

2.  Edit files; then `git add`, `git commit`, and `git push` your changes.

4.  Run `pixi run build` in a shell to regenerate the HTML files in the
    `_build/`.

5.  Run `pixi run publish` in a shell to update the `gh-pages` branch of the
    repo. This uses the [`ghp-import` Python package][ghp-import]. The live web
    page will update automatically after 1-10 minutes.

[ghp-import]: https://github.com/c-w/ghp-import

([back to top][top])


## Installation

To get started, open a terminal (Git Bash on Windows) and `git clone` a copy of
this repo.

Then follow the instructions in the next section to set up the necessary
software environment.


### Pixi

We *strongly recommend* using [Pixi][], a fast package manager based on the
conda ecosystem, to install the packages required by this repo. To install
Pixi, follow [the official instructions][pixi]. If you prefer not to use Pixi,
it's also possible to manually install the packages using conda or mamba.

[pixi]: https://pixi.sh/

The `pixi.toml` file in this repo lists required packages, while the
`pixi.lock` file lists package versions for each platform. When the lock file
is present, Pixi will attempt to install the exact versions listed. Deleting
the lock file allows Pixi to install other versions, which might help if
installation fails (but beware of inconsistencies between package versions).

To install the required packages, open a terminal and navigate to this repo's
directory. Then run:

```sh
pixi install
```

This will automatically create a virtual environment and install the packages.

To open a shell in the virtual environment, run:

```sh
pixi shell
```

You can run the `pixi shell` command from the repo directory or any of its
subdirectories. Use the virtual environment to run any commands related to this
repo. When you're finished using the virtual environment, you can use the
`exit` command to exit the shell.

> [!NOTE]
> If you're using Windows and Git Bash, the `pixi shell` command is [not yet
> supported][pixi-shell-win]. Instead, you can use the `pixi run` command to
> run commands in the virtual environment. See [the pixi
> documentation][pixi-basics] for examples of how to use `pixi run`.

[pixi-shell-win]: https://github.com/prefix-dev/pixi/issues/417
[pixi-basics]: https://pixi.sh/latest/basic_usage/

([back to top][top])
