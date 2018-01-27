# Flash image to SD Card or USB (macOS)

```bash
$ diskutil list # get disk number e.g. disk2

$ diskutil unmountDisk /dev/disk2

$ sudo dd bs=1m if=image.img of=/dev/rdisk2
```

Press `ctrl+t` for progress.
