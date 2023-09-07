Linux
=====

This chapter is about maintaining a Linux computer. The focus is mainly on
[Arch Linux][arch].

[arch]: https://archlinux.org/


Package Management
------------------

To check which package owns a given file, use `pacman -Qo FILE`. The converse
command, to check which package provides a file, is `pacman -F FILE`.

To [clean the package cache][clean], use the paccache script. To remove all but
the two most recent copies of each package:

```bash
paccache -rk2
```

The `-r` flag removes packages and the `-k` flag controls the number of
versions that are kept. The useful `-u` flag makes the command target only
packages that are not installed.

[clean]: https://wiki.archlinux.org/title/pacman#Cleaning_the_package_cache
