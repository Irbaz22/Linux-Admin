
# Linux – File System & Navigation

Hands-on practice Linux - File System & Navigation,
Practised on Rocky Linux in a VMware Workstation virtual machine.

## Skills Practiced
- Directory creation with mkdir -p
- File creation, copying, and moving (touch, cp, mv)
- Wildcard searches (* and ?)
- File type identification with the file command
- Symbolic links and hard links (ln -s and ln)
- Directory tree verification with find -type f and find -type l

## Hands-On Project: Web Server Document Root Setup

Simulated setting up a production web server directory structure under /srv/webroot/
before web server software installation — a common real-world sysadmin task.

### Directory Structure Built

<img width="574" height="709" alt="Screenshot 2026-10-28 042242" src="https://github.com/user-attachments/assets/ce4fcf4a-7c2f-48de-957f-b988d601d3dc" />

### Key Tasks Completed
- Created full directory tree using mkdir -p in a single command chain
- Created symbolic link: /srv/webroot/current-html → public/html/
  (mirrors how production web servers use symlinks for version switching)
- Created hard link: backup/site.conf.bak → config/site.conf
  (verified matching inodes using ls -li)
- Verified complete structure using find /srv/webroot -type f and -type l

<img width="831" height="516" alt="linux m3 project" src="https://github.com/user-attachments/assets/c0609025-696b-453e-be19-8dc505342e7e" />

## Environment
- OS: Rocky Linux
- Platform: VMware Workstation
- User: Root practice environment
