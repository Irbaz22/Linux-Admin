# Linux File Ownership: Service Directory Handoff

**Environment:** Red Hat Enterprise Linux (RHEL), bash

## Scenario

A project directory had to be handed over to a service account and a team group. I created the user and group, transferred ownership recursively, and then checked what a normal user can and cannot do.

## Commands used

`sudo groupadd devteam` - create a group
`sudo useradd appowner` - create a user
`sudo usermod -aG devteam appowner` - add the user to the group without removing other groups
`id appowner` - verify the user and its groups
`mkdir -p ~/service/{src,docs}` - create the project directory tree
`touch ~/service/src/app.py ~/service/docs/notes.txt` - add files
`sudo chown -R appowner:devteam ~/service` - change owner and group for everything inside, recursively
`ls -lR ~/service` - verify the new owner and group on every item
`chown root mine.txt` - as a normal user, try to give away a file I own
`touch /etc/testfile` - try to create a file in `/etc` as a normal user
`ls -ld /etc` - read the permissions on `/etc`
`sudo touch handoff-test/rootfile` - create a root-owned file inside my own directory
`rm handoff-test/rootfile` - delete it as a normal user

## Results

- `chown root` as a normal user failed with `Operation not permitted`. Only root can change a file's owner.
- `touch /etc/testfile` failed with `Permission denied`. `/etc` is owned by root and only the owner has write permission, so my account, counted as "others", cannot create files there.
- Deleting the root-owned file inside my own directory worked after a write-protected warning, because I own the directory and have write permission on it.

## Key takeaways

- In `ls -l`, the third column is the owner and the fourth is the group.
- Only root can change an owner; a normal user can change a group only to one they belong to.
- `-R` applies a change to a directory and everything inside it.
- Whether you can create or delete a file depends on your permissions on the directory that holds it, not just on the file.

<!--
Add screenshots to a screenshots/ folder, then uncomment:
![Ownership listing](screenshots/01-ownership.png)
-->
