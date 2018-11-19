# Minibian Setup

## ssh
username: root, password: raspberry

## Resize sdcard
> `fdisk -l` to list devices.

```
fdisk /dev/mmcblk0
```

`p` to see the current start of the main partition

`d` `2` to delete the main partition

`n` `p` `2` to create a new primary partition, next you need to enter the start (125056?) of the old main partition and then the size (enter for complete SD card).

`w` write the new partition table

`reboot`

### resize filesystem to new size from partition table
```
resize2fs /dev/mmcblk0p2
```

Confirm with `df -h`


## Update
```
apt-get update
apt-get upgrade
```

## easy configuration (but seems to leave junk and daemons)
```
apt-get install raspi-config
```

## lsusb
```
apt-get install usbutils
```

## Wi-Fi

### Install Dependencies

#### firmware
Find vendor with `lsusb`, `dmesg`, kern.log, etc.

Examples:
```
apt-get install firmware-atheros
apt-get install firmware-realtek
apt-get install firmware-ralink
```

#### wpa supplicant
```
apt-get install wpasupplicant
```

#### Regulatory domain settings (crda)
```
apt-get install iw crda wireless-regdb
```

#### useful tools (optional)
```
and apt-get install wireless-tools
```

### Configuration (not working needs more params...)

Edit `/etc/network/interfaces` and add:

```
allow-hotplug wlan0
auto wlan0
iface wlan0 inet dhcp
    wpa-ssid "essid"
    wpa-psk "password"
```
Note: the password can also be entered in hex by running `wpa_passphrase essid password`

### Alternative Configuration

Edit `/etc/network/interfaces`:
```
allow-hotplug wlan0
iface wlan0 inet manual
wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
iface default inet dhcp
```

Edit `/etc/wpa_supplicant/wpa_supplicant.conf`:
```
network={
    ssid="essid"
    psk="password"
}
```
