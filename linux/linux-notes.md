# Linux-notes

## POSTMORTEM

### file desctiptor/inodes

space not being freed form disk after deleting a file, docker case: after deleting large file inside docker container there is no gain of free space `df -h` bc. other process is still using file descriptor?

* run ubuntu container, keep several panes open in tmux
* crate big file `$ dd if=/dev/zero of=1G.md bs=1G count=1`
*  keep file open `$ less 1G.md`
* check used space via `df -h` and `docker system df -v` and `du`
* remove `1g.md` file
* check again disk usage
* run inside and outside container `$ sudo lsof | grep deleted`
* Resolution 1. shutdown of relevant process 2. truncate File Size
* `echo > /proc/[pid]/fd/[fd_number]`

To identify the used file size \(in blocks\): `# lsof -Fn -Fs |grep -B1 -i deleted | grep ^s  | cut -c 2- | awk '{s+=$1} END {print s}'`

U can get more info:

* `stat FILE`

```bash
grep ^s    meta-character that math the beginning line
lsof -Fn -Fs    selects the fields to be output
cut -c 2-    selects only these characters, N- from N'th byte, character or 
field to end of line 
```

{% embed url="https://access.redhat.com/solutions/2316" %}

#### df vs du

* It is possible to delete \([unlink](https://linux.die.net/man/2/unlink)\) a file while a program is still writing to it. If this happens the file remains on disk until all references \(including open file handles\) are removed. `du` cannot find these because they have no filename. But they will still have an inode and so still show up when checked with `df`.

{% embed url="http://linuxshellaccount.blogspot.com/2008/12/why-du-and-df-display-different-values.html" %}

### reading ICMP datagram

{% hint style="info" %}
TODO
{% endhint %}

### timezone, set or change

```bash
# check current time zone
timedatectl
# view all zones
timedatactl list-timezones
# set
sudo timedata set-timezone <time_zone>
```

### TODO

* package manager for Debian, list packages matching given pattern: `$ dpkg -l <package name pattern>`
* re-evaluate group membership e.g. after added to docker group: `$ su - <username>`
* How to set short password? edit `minlen` in `/etc/pam.d/common-password`

## TOPICS

### Advanced Topics

* named piped `$mkfifo`
* `rsync`

### awk

for exercises pipe into awk `ls -al`

```bash
# Use of NR built-in var, display line number
awk '{print NR,$0}' FILE
# use field separator semicolon
awk -F:
awk 'FS=":"'
# print only users with id above 1000
cat /etc/passwd | awk -F: '$3>=1000 {print $3,$1}'
cat /etc/passwd | awk 'BEGIN {FS=":"} $3>=1000 {print $3,$1}'
# U do not need cat and pipe
awk -F: '$3>=1000 {print $3,$1}' /etc/passwd
```

links:

{% embed url="https://www.geeksforgeeks.org/awk-command-unixlinux-examples/" %}

{% embed url="https://www.howtogeek.com/562941/how-to-use-the-awk-command-on-linux/" %}

### BASH

Double-check:

* need space around \[ \] e.g. `[ $? -ne 0]` will produce error due to no inner space at the end of brackets.
* `set -x` Print commands and their arguments as they are executed.

#### Script example

```bash
#!/bin/bash
set -x

DATE=$(date)

fping 172.21.100.2

if [ $? -ne 0 ]; then
	echo $DATE restarting ipsec >> /var/log/marcin.ipsec.log
	/usr/sbin/ipsec stop
	sleep 5
	/usr/sbin/ipsec start
	sleep 5
else
	echo $DATE is alive >> /var/log/marcin.ipsec.log
fi
```



### Enable ssh server \(scp\)

needed for `scp`

```bash
$ sudo apt update
$ sudo apt install openssh-server
$ sudo systemctl status ssh

# If you do not know your IP addr:
$ ip a

# Disabling SSH
$ sudo systemctl [stop/start] ssh
$ sudo systemctl [disable/enable] ssh
```

### scp copy files via SSH scp

other apps: `sftp` and `rsync`

```bash
# local -> remote
$ scp local.txt remoteuser@remoteserver:/remote/folder/

# local <- remote
$ scp remoteuser@remoteserver:/remote/folder/remote.txt local.txt

# copy multiple files from local to remote
$ scp myfile1.txt myfile2.txt remoteuser@remoteserver:/remote/folder/

# copy all files from local to remote
$ scp * remoteuser@remoteserver:/remote/folder/

# copy all files and folders recursively from local to remote
$ scp -r * remoteuser@remoteserver:/remote/folder/
```

### cron

Set a cron every certain hours between certain hours

```bash
0 8-17/2 * * * /path/command
# or
0 8,10,12,14,16 * * * /path/command
```

### htop

`H` hide/show user process threads, will be colored in green OR not, enable tree to see forks/clone

### iptables

`-L, --list [chain]` List all rules in the selected chain.

`-S, --list-rules [chain]` Print all rules in the selected chain. If no chain is selected, all chains are printed like iptables-save.

### ls

`-1`   list one file per line. Useful with `wc -l`

### lsof

Find process running on specific Port `# lsof -i TCP:22`

### man

`-k [cmd]` Search the short descriptions and manual page names for the keyword `cmd` as regular expression. Print out any matches. Equivalent to `appropos [cmd]`

better search: `$ man -wk [cmd]` 

```bash
-k, --apropos
-K, --global-apropos
-w    # Don't display the manual pages, but do print the locations
```

### fzf

```bash
CTRL-T    Paste the selected files and dirs onto the command-line
CTRL-R    Paste the selected command from history onto the command-line
ALT-C     cd into the selected dir

#files under current dir
vim **<TAB>

#Host nanes
ssh **<TAB>

#Env
unset/export/unalias **<TAB>

# Preview window, when --preview option is set, fzf automatically starts an
#+external process with the current line as the arg
fzf --previw 'cat {}'

#see man page
man fzf
```

wow usecase for Docker [https://github.com/junegunn/fzf/wiki/examples\#docker](https://github.com/junegunn/fzf/wiki/examples#docker)

Links:

* Luke Smith @[yt](https://www.youtube.com/watch?v=vt33Hp-4RXg)
* another yt tutorial [link](https://www.youtube.com/watch?v=1a5NiMhqAR0)

### setUID

Setuid is a Linux file permission setting that allows a user to execute that file or program with the permission of the owner of that file. This is primarily used to elevate the privileges of the current user. If a file is “setuid” and is owned by the user “root” then a user that has the ability to execute that program will do so as the user root instead of themselves.

{% hint style="danger" %}
Linux ignores the setuid bit on all interpreted executables \(i.e. executables starting with a `#!` line\)
{% endhint %}

### ssh

#### Disable creating .ssh folder during connection

Use `-o` flag with `StrictHostKeyChecking=no` and `UserKnownHostsFile=/dev/null` 

Not sure about the second one ^^^

### ssh manually add keys

manually add ssh keys for key-authentication

```bash
ssh-keygen -t rsa
ssh-keygen -t rsa -b 4096 -f ~/.ssh/name.key -C "Comment"
# copy key.pub to ~/.ssh/authorized_keys
# send key to remote user 
```

### ssh Adding SSH key to the ssh-agent

```bash
# keep ssh keys in ~/.ssh
$ ssh-add [file]
```

### tar



### tcpdump

`-D,  --list-interfaces` Print the list of the network interfaces available on the system and on which tcpdump can capture packets.

#### tcpdump-notes

* `n packets dropped by kernel` due to a lack of buffer space, You can increase the buffer size with `-B, --buffer-size` e.g. `$ tcpdump -B 4096 ...` the number is in kilobytes. OR better way is to apply better filter.

### Ranger

```bash
# MAIN BINDINGS
gn        Create a new tab
gt, gT    Go to the next or previous tab. You can also use TAB and SHITF+TAB
gc        Close the current tab 

# How to display the size of all selected multiple dirs in current view?
v -  selects all items in current view.
So `vdcv` will show you sizes of all dirs.
```

* show shell output and exit status, use `:shell -w [cmd]` to view the output until you press enter.
* ^^^--- U can rebind `!` to `map ! console shell -w%space`

### Pomodoro GNOME

{% hint style="info" %}
[https://gnomepomodoro.org/](https://gnomepomodoro.org/)
{% endhint %}

### timedatectl

```bash
$ timedatectl
$ cat /etc/timezone
$ timedatectl list-timezones
$ sudo timedatectl set-timezone your_time_zone
```

### unrar

```bash
apt install unrar
unrar l file.rar - list files
unrar x rile.rar - open/extract with their original directory structure
```

### htop

`-d --delay=DELAY` Delay between updates, in tenths of seconds

### fping

fping is meant to be used in scripts, so its output is designed to be easy to parse

RTT = Round Trip Time

### python http.server

`python3 -m http.server`

### PAM

Link

* An Easy Guide to Linux-PAM [link](https://dzone.com/articles/linux-pam-easy-guide)

### GNOME prevent from Windows grouping

1. open `dconf-editor`
2. go to `org/gnome/desktop/wm/keybindings`
3. move the value `<Alt>Tab` from `switch-application` to `switch-windows`
4. if using X11, press `<Alt>F2` then type `r` to restart Gnome

{% embed url="https://superuser.com/questions/394376/how-to-prevent-gnome-shells-alttab-from-grouping-windows-from-similar-apps" %}

### tmux

shortcuts: [link](https://tmuxcheatsheet.com/)

```bash
# Window: Ctrl+b 
c - Create window
, - rename current window
& - close current window
p/n - previus/next window
0 .. 9 - select window by number
" - split panes horizontally
% - verically
x - kill current pane

z - zoom pane

```

```text
# Session managment
tmux ls
tmux new -s [session-name]
tmux rename-session [-t session-name] [new-session-name]
```

### sshfs

mount remote directories over a Secure Shell connection

{% embed url="https://wiki.archlinux.org/index.php/SSHFS" %}

{% embed url="https://unix.stackexchange.com/questions/61567/how-to-specify-key-in-sshfs/61572" %}



