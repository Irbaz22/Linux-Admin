# Linux Command Syntax: Server Workspace Setup and Cleanup

**Environment:** Red Hat Enterprise Linux (RHEL), bash

## Scenario

A fresh server needs a clean working area for a web application. I built the directory structure, inspected it in several ways, and then cleaned it up, using only core commands.

## Commands used

`pwd` - print the current working directory
`mkdir -p webapp/{logs,config,backups}` - create the whole tree in one command; `-p` creates parent directories, and the braces expand into three subdirectories
`ls` - list the contents
`ls -l` - long listing with permissions, owner, size and date
`tree .` - show the directory structure as a tree (install with `sudo dnf install tree` if it is missing)
`ls -ltr` - long listing sorted by time, newest at the bottom
`rm -r webapp` - remove the directory and everything inside it
`ls -l` - confirm the directory is gone

## Command anatomy

Every command follows `command [options] [arguments]`.

- In `ls -ltr`, `ls` is the command, `-l`, `-t` and `-r` are three options grouped under one hyphen, and no argument means it acts on the current directory.
- In `mkdir -p webapp/...`, `-p` is the option and the path is the argument.

## Result

The `webapp` tree with `logs`, `config` and `backups` was created and inspected, then removed with a single command. The final listing confirmed it was gone.

## Key takeaways

- Options change how a command behaves; arguments say what it acts on.
- Short options can be grouped, so `-l -t -r` equals `-ltr`.
- `mkdir -p` is the right way to create nested directories.
- `rm -r` has no recycle bin, so check the path before running it.

<!--
Add screenshots to a screenshots/ folder, then uncomment:
![Directory tree](screenshots/01-tree.png)
-->
