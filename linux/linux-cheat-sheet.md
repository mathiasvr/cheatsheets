# Linux cheat sheet

## Setup

### Updates
```
$ sudo apt-get update -y && sudo apt-get upgrade -y
```

### User accounts
Create user: `useradd -m [-G <group>] <name>`

Set password: `passwd [<user>]`

List: `users`, `groups`

### Hostname
Edit `/etc/hostname` and update `/etc/hosts`.

### Locale
> Mostly to remove warnings

List: `locale`

Generate: `locale-gen "en_US.UTF-8"`
- Or edit locales in: `/etc/locale.gen`

Configure: `dpkg-reconfigure locales`

#### When everything else fails
Try any of these options (not really recommended):

- Run `update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8`
  - or write it directly to `/etc/default/locale`
- Add `LC_ALL=en_US.UTF-8` to `/etc/environment`
- Add `export LC_ALL="en_US.UTF-8"` to `.bashrc`

##### Using SSH
Comment out the `SendEnv LANG LC_*` line in `/etc/ssh/ssh_config` on the ***client***, to not forward locale information to the server.

### Zeroconf (multicast DNS)
```
$ apt-get install avahi-daemon
```
The device should immediately respond to queries, e.g. `ping hostname.local`.


## System utilities

### Package manager
Install: `apt install <name>`, `apt-get install <name>`

Search: `apt search <name>`, `apt-cache search <name>`

### Text files
View: `cat`, `head`, `tail`, `less`

Edit: `nano`, `vi`, `vim`

Search: `grep`

### File and disk space
`du -h`, `df -h`

### Date and time
Display: `date`

Measure: `time <command>`

### Service management

#### SystemD:
`systemctl [start/stop/status/enable/disable] <name>`

`systemctl status`, `systemctl list-units --type=service`

[more...](systemd.md)

#### SysV:
`service <name> [start/stop/status]` or `/etc/init.d/<name> [start/stop/status]`

`update-rc.d <name> [enable/disable]`

`service --status-all`

### Process monitoring
`top`, `htop`, `ps aux` / `ps -ef`


## Debugging

### Hardware and peripherals (device drivers)
View kernel logs: `dmesg`.
> See also `/var/log/dmesg` and `/var/log/kern.log`.

Other useful commands: `lspci`, `lsusb`, `hwinfo`.


## Remote Session (SSH)

### File transfer

#### SCP (SSH)
Upload file from client to server:
```
$ scp <local path> <user>@<host>:<remote path>
```
Swap arguments to download from server to client.

#### rsync (SSH)
- Faster for partial file changes.

```
$ rsync -avz -e ssh <local path> <user>@<host>:<remote path>
```

### Make process continue running after disconnecting
It can be preferable to use [tmux](todo-insert-link) to achieve this.

- Starting new process: (will output to file `nohup.out`)
  - `nohup <command> &`
  - Include stderr: `nohup 2>&1 <command> &`
  - When piping/chaining: `nohup sh -c "cmd1 | cmd2" &`
- Already running process: `ctrl+z`, `bg`, `disown -h`

## Todo: Utilities, unix, terminal,

### split output into files of 500mb
stuff | split -b 500m -a 1 -d - fileprefix-


### Background process

Press `ctrl+z` to suspend current process.
`fg` to resume in foreground.
`bg` to resume in background.


### todo: bash ishm (unix terminal)
- redirect
- write and append (also with `tee`, maybe not)

Redirect IO streams (`stdout`, `stderr` and `stdin`) to a file descriptor (fd):
```
$ command > fd.out 2> fd.err < fd.in
```
  - Use `>>` to append instead of `>` which overwrites.
  - Use `2>&1` to redirect `stderr` to same file descriptor as `stdout`.
  - Use `/dev/null` as fd to "throw away".

#### control keys:
- ctrl+ c,d z,t, a,e
- https://en.wikipedia.org/wiki/Control_key