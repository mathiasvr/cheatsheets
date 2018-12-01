# SystemD (Service management)
SystemD is a comprehensive software suite and init system.
This document only considers service management.

## Common usage
```shell
$ systemctl [start/stop/restart/status/enable/disable] <unit>
```

```shell
$ systemctl status
$ systemctl list-units --type=service
```

## Logs
To read the output of a specific service unit:
```shell
$ journalctl -u <unit>
```

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

When changing the service script, the daemon should be reloaded:
```shell
$ systemctl daemon-reload
```

### More examples

Automatically **restart** service if the process terminates:
```ini
[Service]
Type=simple
Restart=always
RestartSec=3
ExecStart=/path/to/script
```
