# Linux Permissions with Letters: Break and Fix a Config File

**Environment:** Red Hat Enterprise Linux (RHEL), bash

## Scenario

A deployment script and a configuration file needed the right access. I set sensible permissions, then deliberately removed them to see exactly how Linux fails, and restored everything.

## Commands used

`touch app.conf deploy.sh` - create the two files
`chmod u+x deploy.sh` - make the script executable by the owner only
`chmod u=rw,g=r,o= app.conf` - owner read/write, group read-only, others no access
`ls -l app.conf deploy.sh` - verify the permission strings
`chmod a-r app.conf` - remove read permission from everyone
`cat app.conf` - try to read it
`mkdir testdir` - create a test directory
`chmod a-x testdir` - remove execute permission from the directory for everyone
`cd testdir` - try to enter it
`chmod 755 testdir` - restore access to the directory
`chmod u=rw,g=r,o= app.conf` - restore the config file

## Results

- Reading `app.conf` after removing read for everyone failed with `Permission denied`. Even the owner is blocked until the permission is added back.
- Entering `testdir` after removing execute failed with `Permission denied`.
- Restoring the permissions fixed both.

## Key takeaways

- `r`, `w` and `x` apply separately to the user, the group and others.
- On a directory, `x` means you can enter it with `cd`; it does not mean the directory is a script.
- Permissions apply to the owner too, so removing a bit blocks even the person who created the file.
- Letter notation (`u+x`, `g-w`, `a-r`) changes only what you name.
- `chmod` works the same way on files and directories.

<!--
Add screenshots to a screenshots/ folder, then uncomment:
![Permission denied errors](screenshots/01-errors.png)
-->
