# 📜 Linux CLI Command Bible

**Note**

This document is a personal command “bible” built over time from a mix of real-world usage and continuous learning.  
Some commands listed here are ones I actively use in day-to-day administration, while others are commands I have encountered in manuals, official documentation, forums, and community knowledge bases, and keep as reference for future situations.

The goal of this file is not to claim mastery of every command, but to progressively build a reliable toolbox, revisit it regularly, and deepen understanding through practice as new use cases arise.

---

## 0 - Help, docs, and “what is this command?”

```bash
man <cmd>                # manual
<cmd> --help             # quick help
tldr <cmd>               # simplified examples (if installed)
info <cmd>               # GNU info pages (sometimes richer than man)
type <cmd>               # builtin/alias/function/file?
which <cmd>              # path resolution (less reliable than type)
command -v <cmd>         # reliable “is it available?”
apropos <keyword>        # search man page summaries
```
---

## 1 - Navigation & filesystem basics

```bash
pwd                      # current directory
ls -lah                  # list including hidden, human-readable
cd -                     # jump back to previous directory
tree -a -L 2             # directory tree (if installed)
du -sh *                 # size of items in current dir
df -hT                   # disk free by filesystem type
stat <file>              # file metadata
realpath <path>          # canonical path
```
---

## 2 - Create / copy / move / delete

```bash
mkdir -p /path/to/dir
cp -av src/ dest/         # preserve attrs, verbose
mv -v old new
rm -i <file>              # interactive delete
rm -rf <dir>              # dangerous: recursive force (double-check!)
ln -s target linkname     # symlink
```
---

## 3 - Search: files, content, and “where is X?”

### Find files by name, size, time

```bash
find /path -name "*.log"
find / -type f -size +500M 2>/dev/null
find /var/log -type f -mtime -7
```

### Find content inside files (fast)

```bash
grep -RIn "needle" /path
rg -n "needle" /path       # ripgrep (if installed, faster)
```

### Locate installed files (if locate DB exists)

```bash
updatedb                   # may require root
locate sshd_config
```

---

## 4 - Viewing files (logs, configs, large files)

```bash
cat file
less -S file               # horizontal scroll
tail -n 200 file
tail -f fle                # follow (live)
tail -F file               # follow across rotation
head -n 50 file
wc -l file                 # line count
```

---

## 5 - Text processing

```bash
cut -d: -f1 /etc/passwd
sort file | uniq -c | sort -nr
awk '{print $1,$9}' file
sed -n '1,120p' file              # print range
tr -s ' '                         # squeeze spaces
xargs -0                          # safe with null-delimited input
```

---

## 6 - Permissions & ownership

```bash
ls -l
chmod 644 file
chmod 755 dir
chmod -R u=rwX,g=rX,o= /path        # safe-ish recursive pattern
chown user:group file
chown -R user:group /path
umask
getfacl file                        # ACL view
setfacl -m u:alice:rw file          # set ACL
```

### Common octal cheats:

644 = ```rw-r--r--```

600 = ```rw-------```

755 = ```rwxr-xr-x```

700 = ```rwx------```

---

## 7 - Users, groups, and identity

```bash
id
whoami
groups
getent passwd <user>
getent group <group>
useradd -m -s /bin/bash <user>      # varies by distro policy
usermod -aG sudo <user>             # Debian/Ubuntu (wheel on RHEL)
passwd <user>
userdel -r <user>                   # remove user + home (careful)
```

---

## 8 - Processes, performance, and troubleshooting

```bash
ps aux
ps -ef
top                        # interactive
htop                       # nicer top (if installed)
pgrep -a nginx
pkill -f "pattern"
nice -n 10 <cmd>           # start with lower priority
renice 10 -p <pid>
uptime
free -h
vmstat 1
iostat -xz 1               # sysstat package
```

---

## 9 - Services (systemd)

```bash
systemctl status <svc>
systemctl start <svc>
systemctl stop <svc>
systemctl restart <svc>
systemctl reload <svc>
systemctl enable <svc>       # start on boot
systemctl disable <svc>
systemctl is-enabled <svc>
systemctl list-units --type=service --state=running
```

---

## 10 - Logs & journald

```bash
journalctl -xe
journalctl --since "today"
journalctl --since "2026-01-20 10:00" --until "2026-01-20 12:00"
journalctl -p err..alert --since yesterday
journalctl -k -b          # kernel logs, current boot
```

---

### Classic log files:

```bash
/var/log/syslog           # Debian/Ubuntu
/var/log/messages         # RHEL/CentOS
/var/log/auth.log         # Debian/Ubuntu auth
/var/log/secure           # RHEL auth
```

---

## 11 - Networking (interfaces, routes, DNS, connectivity)

```bash
ip a
ip r
ip neigh
ip link set <iface> up
ping -c 4 1.1.1.1
traceroute <host>                 # or tracepath
mtr <host>                        # best combined tool (if installed)
ss -tulpn
curl -I https://example.com
wget -S --spider https://example.com
```

### DNS checks:

```bash
resolvectl status
dig +short A example.com
dig example.com MX
nslookup example.com
```

### HTTP quick checks:

```bash
curl -v https://example.com
curl -sS https://example.com | head
```

### Firewall:

```bash
ufw status verbose                 # Ubuntu
firewall-cmd --state               # RHEL (firewalld)
firewall-cmd --list-all
nft list ruleset                   # nftables
iptables -S                        # legacy
```

---

## 12 - SSH (remote admin, keys, tunnels)

```bash
ssh user@host
ssh -i ~/.ssh/key user@host
ssh -p 2222 user@host
scp file user@host:/tmp/
rsync -avz ./dir user@host:/opt/dir
```

### Generate keys:

```bash
ssh-keygen -t ed25519 -C "me@host"
ssh-copy-id user@host
```

### Port forwarding (tunnels):

```bash
ssh -L 8080:127.0.0.1:80 user@host         # local -> remote
ssh -R 2222:127.0.0.1:22 user@host         # remote -> local
ssh -D 1080 user@host                      # SOCKS proxy
```

---

## 13 - Packages

### Debian/Ubuntu (apt)

```bash
apt update
apt install <pkg>
apt remove <pkg>
apt purge <pkg>
apt autoremove
apt search <term>
apt show <pkg>
dpkg -l | grep <term>
dpkg -S /path/to/file
```

### RHEL/Fedora (dnf/yum)

```bash
dnf install <pkg>
dnf remove <pkg>
dnf search <term>
dnf info <pkg>
rpm -qa | grep <term>
rpm -qf /path/to/file
```

---

## 14 - Storage: disks, partitions, mounts

```bash
lsblk -f
blkid
fdisk -l                  # careful: read-only use is fine
mount | column -t
mount /dev/sdb1 /mnt
umount /mnt
```

---

## 15 - Archives & compression

```bash
tar -czf backup.tgz /etc
tar -xzf backup.tgz
tar -tvf backup.tgz | head
gzip file
gunzip file.gz
zip -r archive.zip folder/
unzip archive.zip
```

---

## 16 - Scheduling (cron, systemd timers)

### Cron quick view:

```bash
crontab -l
sudo crontab -l
ls -lah /etc/cron.* /etc/crontab
```

### Edit crontab:

```bash
crontab -e
```

### Systemd timers:

```bash
systemctl list-timers --all
```

---

## 17 - Environment & shells

```bash
echo $SHELL
env | sort
export VAR=value
alias ll='ls -lah'
history | tail
```

---

## 18 - Security essentials

```bash
sudo -v                                          # refresh sudo timestamp
sudoedit /etc/ssh/sshd_config                    # safer editing
ss -tulpn                                        # expose listening services
systemctl list-unit-files | grep enabled
last                                             # login history
lastb                                            # failed logins (if enabled)
faillock --user <user>                           # RHEL family
```

### File integrity quick check:

```bash
sha256sum <file>
```

### Permissions audit (common misconfigs):

```bash
find / -xdev -type f -perm -0002 2>/dev/null | head    # world-writable files
```

---

## 19 - Backups & sync (rsync)

```bash
rsync -avh --progress src/ dest/
rsync -avh --delete src/ dest/                         # mirror (careful)
rsync -avh -e "ssh -p 2222" src/ user@host:/path/
```

Dry-run first:

```bash
rsync -avhn --delete src/ dest/
```

---

## 20 - “What changed?” (diffs, configs, troubleshooting)

```bash
diff -u file1 file2
cmp file1 file2
git status
git diff
```

### Config test patterns (examples):

```bash
nginx -t
apachectl configtest
sshd -t
named-checkconf
```

---

## 21 - Handy one-liners

### Top CPU processes:

```bash
ps aux --sort=-%cpu | head -n 15
```

### Top memory processes:

```bash
ps aux --sort=-%mem | head -n 15
```

### Largest files in a path:

```bash
find /path -type f -printf "%s %p\n" | sort -n | tail -n 20
```

### Which package owns a binary:

```bash
command -v <cmd> && dpkg -S "$(command -v <cmd>)" 2>/dev/null || rpm -qf "$(command -v <cmd>)"
```

### Check listening ports + owning processes:

```bash
ss -tulpn
```

### Follow auth failures live:

```bash
journalctl -f | grep -iE "sshd|sudo|failed|invalid"
```

---

## 22 - Quick “first 5 minutes” checklist on a new server

```bash
hostnamectl
uname -a
uptime
df -hT
free -h
ip a && ip r
ss -tulpn
systemctl --failed
journalctl -p err..alert -b --no-pager
```

---
