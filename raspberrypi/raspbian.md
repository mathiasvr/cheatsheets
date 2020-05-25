# Raspbian (lite) Setup

## login
username: `pi`, password: `raspberry`

## setup (easy)
Run `sudo raspi-config` and work your magic.

> resize sd card under `advanced options`
> set locale
> enable ssh under `interfacing options`

## ssh
```
$ ssh pi@raspberrypi.local
```
- zeroconf is preconfigured: `raspberrypi.local` 


## update
```
$ sudo apt-get update -y && sudo apt-get upgrade -y
```

# headless
Enable ssh and set up wifi on sd card:
```
$ touch /Volumes/boot/ssh
```

Create file `/Volumes/boot/wpa_supplicant.conf` with contents:
```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="NETWORK-NAME"
    psk="NETWORK-PASSWORD"
}
```

## Wi-Fi
WPA Supplicant should be preconfigured in `/etc/network/interfaces`.

Edit: `/etc/wpa_supplicant/wpa_supplicant.conf` and add:
```
network={
    ssid="essid"
    psk="password"
}
```

## Resize sdcard
> `fdisk -l` to list devices.

```
$ fdisk /dev/mmcblk0
```

`p` to see the current start of the main partition

`d` `2` to delete the main partition

`n` `p` `2` to create a new primary partition, next you need to enter the start (125056?) of the old main partition and then the size (enter for complete SD card).

`w` write the new partition table

`reboot`

### resize filesystem to new size from partition table
```
$ resize2fs /dev/mmcblk0p2
```

Confirm with `df -h`
