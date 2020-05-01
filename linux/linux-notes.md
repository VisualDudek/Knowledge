# Linux-notes

* package manager for Debian, list packages matching given pattern: `$ dpkg -l <package name pattern>`
* re-evaluate group membership e.g. after added to docker group: `$ su - <username>`
* How to set short password? edit `minlen` in `/etc/pam.d/common-password`

### Advanced Topics

* named piped `$mkfifo`
* `rsync`

### Enable ssh server

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

### CRON

Set a cron every certain hours between certain hours

```bash
0 8-17/2 * * * /path/command
# or
0 8,10,12,14,16 * * * /path/command
```

### Adding SSH key to the ssh-agent

```bash
# keep ssh keys in ~/.ssh
$ ssh-add [file]
```

### Ranger

```bash
# MAIN BINDINGS
gn        Create a new tab
gt, gT    Go to the next or previous tab. You can also use TAB and SHITF+TAB
gc        Close the current tab 
```

* show shell output and exit status, use `:shell -w [cmd]` to view the output until you press enter.
* ^^^--- U can rebind `!` to `map ! console shell -w%space`

### Pomodoro GNOME

{% hint style="info" %}
[https://gnomepomodoro.org/](https://gnomepomodoro.org/)
{% endhint %}

### Timezone

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

### python http.server

`python3 -m http.server`

### GNOME prevent from Windows grouping

1. open `dconf-editor`
2. go to `org/gnome/desktop/wm/keybindings`
3. move the value `<Alt>Tab` from `switch-application` to `switch-windows`
4. if using X11, press `<Alt>F2` then type `r` to restart Gnome

{% embed url="https://superuser.com/questions/394376/how-to-prevent-gnome-shells-alttab-from-grouping-windows-from-similar-apps" %}

### TMUX

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


```

### SSHFS

mount remote directories over a Secure Shell connection

{% embed url="https://wiki.archlinux.org/index.php/SSHFS" %}

{% embed url="https://unix.stackexchange.com/questions/61567/how-to-specify-key-in-sshfs/61572" %}



