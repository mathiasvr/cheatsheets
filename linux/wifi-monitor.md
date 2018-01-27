# Wi-Fi monitoring
Capture raw WLAN network traffic.

## Device listing
List network interfaces and physical devices.
```bash
$ iw dev
$ iw phy [<phyname> info]
```

> Note that any command beginning with `iw dev <devname>` can be replaced with `iw phy <phyname>`, or simply `iw <name>`.

## Multiple interface modes
Add a separate interface to use a physical device in multiple modes (e.g. managed and monitor) simultaneously.

```
$ iw dev wlan0 interface add mon0 type monitor
```

Enable the new device:
```
$ ifconfig mon0 up
```

Disable and remove:
```
$ ifconfig mon0 down
$ iw dev mon0 del
```

## Channel and frequency

Set channel or center frequency:
```
$ iw dev <devname> set channel <channel> [HT20|HT40+|HT40-]
$ iw dev <devname> set freq <freq> [HT20|HT40+|HT40-]
```
You can also specify the channel width (HT mode), usually 20MHz or 40MHz. See `iw phy` for supported rates.

[![2.4 GHz Wi-Fi channels (802.11b,g WLAN).svg](https://upload.wikimedia.org/wikipedia/commons/8/8c/2.4_GHz_Wi-Fi_channels_%28802.11b%2Cg_WLAN%29.svg)](https://commons.wikimedia.org/w/index.php?curid=8353678)

> A single physical device cannot be set to different channels when creating multiple interface. E.g. the monitor device `mon0` must be on the same channel as the managed device `wlan0`, or `wlan0` must be disabled.

## Change MAC address
Randomize device MAC address:
```
$ macchanger -r mon0
```
