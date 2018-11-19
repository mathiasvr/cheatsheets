# SystemD (Service management)
SystemD is a comprehensive software suite and init system.
This document only considers service management.

## Common usage
`systemctl [start/stop/status/enable/disable] <unit>`

`systemctl status`, `systemctl list-units --type=service`

## Create startup service
Create a service script, as root, in the directory: `/etc/systemd/system/my.service`

Example content of `my.service`:
```ini
[Unit]
Description=My service

[Service]
ExecStart=/path/to/myscript.sh

[Install]
WantedBy=multi-user.target
```

Make the script executable:
```shell
$ chmod u+x /path/to/myscript.sh
```

Check service syntax with:
```shell
$ systemd-analyze verify /etc/systemd/system/my.service
```

## Logs (service output)
To read the output of a specific service unit:
```
$ journalctl -u <unit>
```