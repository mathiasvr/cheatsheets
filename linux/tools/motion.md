# Camera surveillance with `motion`

## Install

```
$ sudo apt-get install motion 
```

Fix permissions if it complains:
```
$ sudo chown motion:motion /var/lib/motion
$ sudo chmod g+rw /etc/motion/motion.conf
$ sudo chmod g+rw /var/log/motion.log
```

> Note: A more featureful fork can be checked out at https://github.com/Mr-Dave/motion.

## Setup

Edit `/etc/default/motion`:
```
$ start_motion_daemon=yes
```

Edit `/etc/motion/motion.conf` with your configuration,
here are some settings you might want to check out:
```
daemon on 
logfile /var/log/motion.log

width 1280
height 720 

framerate 2 
pre_capture 1
post_capture 1 
max_mpeg_time 600 

stream_localhost off

snapshot_filename %Y%m%d%H%M%S-%v-snapshot
picture_filename  %Y%m%d%H%M%S-%q-%v
movie_filename    %Y%m%d%H%M%S-%v
```

Restart motion service or reboot.

## Multiple cameras

Edit `/etc/motion/motion.conf` (at the bottom) and add a thread configuration for each camera:
```
thread /etc/motion/thread0.conf
thread /etc/motion/thread1.conf
; thread /etc/motion/thread2.conf
; thread /etc/motion/thread3.conf
; thread /etc/motion/thread4.conf
```

Create `thread0.conf`:
```
videodevice /dev/video0
text_left Cam-1
target_dir /var/lib/motion/cam1
stream_port 8081
```

Create `thread1.conf`:
```
videodevice /dev/video1
text_left Cam-2
target_dir /var/lib/motion/cam2
stream_port 8082
```
