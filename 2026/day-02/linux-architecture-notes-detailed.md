# Complete Linux Notes for DevOps Engineers
# Beginner to Advanced Linux Administration Guide

Author: DevOps Linux Learning Notes

---

# Table of Contents

1. What is Linux?
2. Linux Architecture
3. Linux File System
4. Understanding Linux Paths
5. Important Linux Directories
6. File Types in Linux
7. Basic Linux Commands
8. File & Directory Management
9. Permissions & Ownership
10. User & Group Management
11. Process Management
12. Service Management using systemd
13. Networking Commands
14. Storage & Disk Management
15. Mounting Disks
16. Package Management
17. SSH & Remote Access
18. Log Management
19. Compression Commands
20. Monitoring & Troubleshooting
21. Environment Variables
22. Cron Jobs
23. Shutdown/Reboot Commands
24. Important DevOps Troubleshooting Commands
25. Real-Time Linux Admin Tasks
26. Linux Tips for DevOps Engineers

---

# 1. What is Linux?

Linux is an open-source operating system.

It is used in:
- Servers
- Cloud
- AWS/Azure/GCP
- Docker
- Kubernetes
- DevOps
- Networking
- Databases

Linux controls:
- CPU
- Memory
- Storage
- Processes
- Networking
- Hardware communication

---

# 2. Linux Architecture

## Main Components

| Component | Purpose |
|---|---|
| Kernel | Core of OS |
| Shell | Command interpreter |
| File System | Stores files |
| User Space | Applications run |
| systemd/init | Boot & service manager |

---

# 3. Linux File System

Linux uses a tree structure.

Everything starts from:

```bash

##############################################################

4. Understanding Linux Paths
Absolute Path

Starts from /

Example:

/home/devops/file.txt
Relative Path

Path from current directory.

Example:

documents/file.txt
5. Important Linux Directories
Directory	Usage
/	Root
/home	User home directories
/etc	Configurations
/var	Logs/data
/tmp	Temporary files
/opt	Optional software
/usr	User applications
/bin	Essential commands
/sbin	System admin commands
/lib	Libraries
/proc	Process information
/dev	Device files
/mnt	Temporary mounts
/media	USB/CD mounts
6. File Types in Linux
Type	Symbol
File	-
Directory	d
Link	l
Check File Type
file test.txt

Example Output:

test.txt: ASCII text
7. Basic Linux Commands
pwd
Purpose

Shows current working directory.

Command
pwd
Example Output
/home/devops
whoami
Purpose

Shows current logged-in user.

whoami
hostname
Purpose

Displays server hostname.

hostname
date
Purpose

Shows current date/time.

date
uptime
Purpose

Shows server uptime and load.

uptime

Example:

10:30:20 up 10 days, 2 users, load average: 0.20
clear
Purpose

Clears terminal screen.

clear
history
Purpose

Shows previously executed commands.

history
man
Purpose

Displays command manual.

man ls

Quit using:

q
8. File & Directory Management
ls
Purpose

Lists files/directories.

ls
Common Options
Option	Meaning
-l	Long listing
-a	Hidden files
-h	Human readable
-t	Sort by time
Examples
ls -l

Shows:

Permissions
Owner
Size
Date
ls -la

Shows hidden files too.

ls -lh

Shows file sizes in KB/MB/GB.

cd
Purpose

Change directory.

cd /var/log

Go back:

cd ..

Go home:

cd ~
mkdir
Purpose

Create directories.

mkdir test

Create nested directories:

mkdir -p app/logs/data
What -p Means

Creates parent directories automatically.

touch
Purpose

Create empty file.

touch test.txt
cp
Purpose

Copy files/directories.

cp file1 file2

Copy directory:

cp -r dir1 dir2
What -r Means

Recursive copy.

mv
Purpose

Move or rename files.

Rename:

mv old.txt new.txt

Move:

mv file.txt /tmp
rm
Purpose

Delete files/directories.

Delete file:

rm file.txt

Delete folder:

rm -r folder

Force delete:

rm -rf folder
What -rf Means
Option	Meaning
-r	Recursive
-f	Force

WARNING:
Never run:

rm -rf /
cat
Purpose

Display file content.

cat file.txt
less
Purpose

View large files page by page.

less app.log

Useful keys:

Space = next page
q = quit
head
Purpose

Shows first 10 lines.

head file.txt

Show first 20:

head -20 file.txt
tail
Purpose

Shows last lines.

tail file.txt

Live logs:

tail -f app.log
What -f Means

Follow file continuously.

Very useful for monitoring logs.

grep
Purpose

Search text inside files.

grep ERROR app.log

Ignore case:

grep -i error app.log

Count matches:

grep -c ERROR app.log
find
Purpose

Find files/directories.

find / -name nginx.conf

Find files only:

find /var -type f

Find directories:

find /var -type d
9. Permissions & Ownership
ls -l Output Understanding

Example:

-rwxr-xr-- 1 root root 500 Jan 1 test.sh

Breakdown:

Part	Meaning
-	File
rwx	Owner permissions
r-x	Group permissions
r--	Others permissions
chmod
Purpose

Change permissions.

chmod 755 script.sh

Meaning:

7 = rwx
5 = r-x
5 = r-x
chown
Purpose

Change ownership.

chown devops:devops file.txt
10. User & Group Management
useradd
Purpose

Create user.

sudo useradd devops

Create home directory:

sudo useradd -m devops
passwd
Purpose

Set password.

sudo passwd devops
usermod
Purpose

Modify user.

Add sudo access:

sudo usermod -aG sudo devops
What -aG Means
Option	Meaning
-a	Append
-G	Group
id
Purpose

Check user UID/GID.

id devops
11. Process Management
ps
Purpose

View processes.

ps -ef
What -ef Means
Option	Meaning
-e	All processes
-f	Full format
top
Purpose

Real-time process monitoring.

top

Shows:

CPU
Memory
Running processes
htop

Better version of top.

htop
kill
Purpose

Kill process.

kill PID

Force kill:

kill -9 PID
What -9 Means

SIGKILL signal.

Immediately terminates process.

pkill

Kill by process name.

pkill nginx
pstree

Shows process hierarchy.

pstree
12. Service Management using systemd
systemctl
Purpose

Manage services.

Check Status
systemctl status nginx
Start Service
systemctl start nginx
Stop Service
systemctl stop nginx
Restart Service
systemctl restart nginx
Enable at Boot
systemctl enable nginx
Disable at Boot
systemctl disable nginx
journalctl
Purpose

View logs.

journalctl -u nginx
What -u Means

Specific unit/service.

journalctl -xe
Option	Meaning
-x	Extra details
-e	Jump to end
13. Networking Commands
ip a
Purpose

Shows IP addresses.

ip a
ping
Purpose

Check connectivity.

ping google.com
curl
Purpose

Call APIs/websites.

curl google.com

Headers only:

curl -I google.com
wget
Purpose

Download files.

wget URL
netstat
Purpose

Shows network connections and ports.

netstat -tulnp
Meaning of -tulnp
Option	Meaning
-t	TCP
-u	UDP
-l	Listening ports
-n	Numeric output
-p	Process name/PID
Real Usage

Find what is using port 80:

netstat -tulnp | grep 80
ss

Modern replacement for netstat.

ss -tulnp
nslookup

DNS lookup.

nslookup google.com
dig

Advanced DNS troubleshooting.

dig google.com
14. Storage & Disk Management
df
Purpose

Disk free space.

df -h
What -h Means

Human readable.

Shows:

GB
MB
du
Purpose

Directory size.

du -sh /var/log
Option	Meaning
-s	Summary
-h	Human readable
lsblk
Purpose

List disks/partitions.

lsblk
fdisk

Disk partition tool.

fdisk -l
What -l Means

List partitions.

15. Mounting Disks
mount
Purpose

Attach storage device.

mount /dev/xvdf1 /data
umount
Purpose

Unmount device.

umount /data
/etc/fstab

Permanent mount configuration.

Example:

/dev/xvdf1 /data ext4 defaults 0 0
16. Package Management
apt

Ubuntu package manager.

apt update

Updates package list.

apt upgrade

Upgrades installed packages.

apt install nginx

Installs package.

yum/dnf

RHEL/CentOS package manager.

yum install nginx
17. SSH & Remote Access
ssh

Remote login.

ssh user@server
scp

Copy files remotely.

scp file.txt user@server:/tmp
ssh-keygen

Generate SSH keys.

ssh-keygen
18. Log Management
tail -f

Live log monitoring.

tail -f /var/log/messages
grep logs
grep ERROR app.log
Common Logs
File	Purpose
/var/log/messages	System logs
/var/log/secure	Login logs
/var/log/syslog	Ubuntu logs
19. Compression Commands
tar

Create archive:

tar -cvf backup.tar folder
Option	Meaning
-c	Create
-v	Verbose
-f	File

Extract:

tar -xvf backup.tar
Option	Meaning
-x	Extract
gzip

Compress:

gzip file.txt

Decompress:

gunzip file.txt.gz
20. Monitoring & Troubleshooting
free

Memory usage.

free -m
What -m Means

Display in MB.

vmstat

Performance statistics.

vmstat
iostat

Disk IO stats.

iostat
lsof

Open files/ports.

lsof -i :80

Shows process using port 80.

21. Environment Variables
env

Displays environment variables.

env
echo $PATH
Purpose

Shows executable search paths.

echo $PATH

Example:

/usr/local/bin:/usr/bin:/bin

Meaning:
Linux searches commands in these directories.

Example:
When you run:

python

Linux checks:

/usr/local/bin
/usr/bin
/bin

until it finds python executable.

export

Set environment variable.

export APP_ENV=prod
22. Cron Jobs
crontab

Schedule jobs.

crontab -e

Example:

0 2 * * * /backup.sh

Meaning:
Runs daily at 2 AM.

23. Shutdown/Reboot Commands
shutdown

Shutdown immediately:

shutdown now

Shutdown after 10 mins:

shutdown +10
reboot

Restart server.

reboot
poweroff

Power off system.

poweroff
logout

Logout current user.

logout
24. Important DevOps Troubleshooting Commands
High CPU
top
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu
High Memory
free -m
top
Disk Full
df -h
du -sh /*
Service Failure
systemctl status nginx
journalctl -u nginx
Port Issue
netstat -tulnp
ss -tulnp
lsof -i :8080
DNS Issue
dig google.com
nslookup google.com
25. Real-Time Linux Admin Tasks
Task	Commands
Create User	useradd
Reset Password	passwd
Restart Service	systemctl restart
Check Logs	tail/journalctl
Check Disk	df -h
Mount Disk	mount
Check Ports	netstat
Find File	find
Monitor CPU	top
Monitor Memory	free
Kill Process	kill
Check Network	ping/curl
26. Linux Tips for DevOps Engineers
Important Areas to Master
Linux basics
Networking
systemd
Logs
SSH
Storage
Process management
Permissions
Disk troubleshooting