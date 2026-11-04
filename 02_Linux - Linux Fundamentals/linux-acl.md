# Linux ACL: Single-User Access Without Opening the File

**Environment:** Red Hat Enterprise Linux (RHEL), bash

## Scenario

One engineer needed to edit a root-owned file. Adding them to the file's group or opening the file to all users was not acceptable, so I used an Access Control List to grant access to that one user only.

If `setfacl` is missing, install it with `sudo dnf install acl`.

## Commands used

`sudo useradd user1` and `sudo useradd user2` - create two test users
`sudo touch /tmp/TX` - create a root-owned file
`sudo chmod 644 /tmp/TX` - owner read/write, everyone else read-only
`getfacl /tmp/TX` - show the baseline permissions
`sudo -u user1 sh -c 'echo test >> /tmp/TX'` - try to write as user1
`sudo setfacl -m u:user1:rw /tmp/TX` - grant user1 read and write only
`getfacl /tmp/TX` - confirm the new entry
`sudo -u user1 sh -c 'echo hello >> /tmp/TX'` - write as user1 again
`sudo -u user2 sh -c 'echo hi >> /tmp/TX'` - try as user2
`ls -l /tmp/TX` - check for the `+` sign
`sudo groupadd devs` and `sudo setfacl -m g:devs:rw /tmp/TX` - grant a group access the same way
`sudo mkdir /tmp/shared` and `sudo setfacl -d -m u:user1:rw /tmp/shared` - set a default ACL so new files inherit access
`sudo -u user1 rm /tmp/TX` - try to delete the file
`sudo setfacl -x u:user1 /tmp/TX` - remove one entry
`sudo setfacl -b /tmp/TX` - remove all ACL entries

## Results

- user1 could not write before the ACL and could write after it.
- user2 was still blocked, so the file was never opened to everyone.
- `ls -l` showed a `+` at the end of the permission string.
- user1 could edit the file but could not delete it (`Operation not permitted`).
- After removing the entries, `getfacl` showed the original permissions again.

## Key takeaways

- An ACL adds fine-grained access for a named user or group on top of normal permissions.
- `setfacl -m` modifies, `-x` removes one entry, `-b` removes all, and `-d` sets defaults for new files in a directory.
- Write permission through an ACL allows editing but not deleting. Deleting depends on the directory's permissions, and `/tmp` also has the sticky bit, which stops users deleting files they do not own.
- Granting access with an ACL is safer than opening the file to all others.

<!--
Add screenshots to a screenshots/ folder, then uncomment:
![getfacl before and after](screenshots/01-getfacl.png)
-->
