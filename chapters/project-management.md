Project Management
==================

This chapter describes how to organize projects and ensure results are
reproducible.


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


Git
---

### Tags

To tag a workshop reader run (for example):

```sh
git tag -a 2022-spring -m "The reader used for Spring 2022."
```

The `-a` flag makes an [annotated tag][] which includes a name, email, date,
and message. The `-m` flag sets the message, as with `git commit`. You can
optionally specify a commit number as the final argument to tag a past commit.

[annotated tag]: https://git-scm.com/book/en/v2/Git-Basics-Tagging

To push a tag, run:

```sh
git push origin 2022-spring
```

You can use the `--tags` flag instead to push all tags.

You can use `git tag` with `-d` to remove a tag or `-f` to force update a tag.


### Purging Files

GitHub has a [helpful tutorial about purging files][gh-purge].

[gh-purge]: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository

The simplest way to purge files is to use the [BFG Repo Cleaner][bfg], which is
available [through conda-forge][conda-bfg]. To purge a file or directory, first
remove it with `git rm` and `git commit`. Then run `bfg` with either the
`--delete-files` flag or the `--delete-folders` flag. Both use glob syntax and
search the entire repository (so the argument is a pattern, not a path).

[bfg]: https://rtyley.github.io/bfg-repo-cleaner/
[conda-bfg]: https://anaconda.org/conda-forge/bfg


### Committing on Behalf of Others

To make a commit on behalf of someone else, run:

```
git commit --author "NAME <EMAIL@ucdavis.edu>"
```
