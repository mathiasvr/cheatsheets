# File sharing for macOS with `netatalk`

File sharing for macOS

```
sudo apt-get install netatalk
```

Edit `/etc/netatalk/AppleVolumes.default`, and go to the bottom to add shared directories:
```
~/ "Home Directory"
/media "Media"
```

Restart the netatalk service or reboot.
