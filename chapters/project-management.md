Project Management
==================

This chapter describes how to organize projects and ensure results are
reproducible.


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

