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
With `<op>` is `start`, `stop`, `status`, `enable`, `disable`.

#### SystemD:
`systemctl <op> <name>`

`systemctl list-units --type=service`

#### SysV:
`service <name> <op>` or `/etc/init.d/<name> <op>`

`service --status-all`

### Process monitoring
`ps aux`, `top`, `htop`


## Debugging

### Hardware and peripherals (device drivers)
View kernel logs: `dmesg`.
> See also `/var/log/dmesg` and `/var/log/kern.log`.

Other useful commands: `lspci`, `lsusb`, `hwinfo`.
