# Linux Permissions with Numbers: Permission Audit

**Environment:** Red Hat Enterprise Linux (RHEL), bash

## Scenario

A security review asked for correct permissions on six common server files. I picked a numeric mode for each and wrote down the reasoning.

## How the numbers work

Each digit is a sum: read = 4, write = 2, execute = 1. The three digits apply to the user, the group and others, in that order.

## Commands used

`touch private.key index.html backup.sh team.log secrets.env reference.txt` - create the six files
`chmod 600 private.key` - owner read/write only; private keys must not be readable by anyone else
`chmod 644 index.html` - owner read/write, everyone else read; public web content
`chmod 750 backup.sh` - owner full access, group read/execute, others none; a script run by a team
`chmod 664 team.log` - owner and group read/write, others read; a shared log
`chmod 600 secrets.env` - owner read/write only; application secrets
`chmod 444 reference.txt` - read-only for everyone; a document nobody should edit
`ls -l` - verify all six
`chmod u=rw,go= private.key` - same as 600, written with letters
`chmod a=r reference.txt` - same as 444, written with letters
`stat -c '%a %n' private.key reference.txt` - print the numeric mode to prove letters and numbers match

## Why I avoided 777

`777` lets every user on the system modify and execute the file. That removes any protection and lets any account, or any compromised process, change or run it.

## Key takeaways

- The digits are user, group, others, in that order, and each is the sum of 4, 2 and 1.
- A digit cannot be left blank; use `0` for no permission.
- Numeric modes set all three levels at once, while letter notation changes only what you name.
- Secrets and keys belong at `600`; shared files rarely need more than `664` or `750`.

<!--
Add screenshots to a screenshots/ folder, then uncomment:
![Permission listing](screenshots/01-listing.png)
-->
