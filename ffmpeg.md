# FFmpeg
https://ffmpeg.org/

## Encode H.264

```shell
$ ffmpeg -i input -c:v libx264 output.mp4
```

Preset for (very) high quality:  

```shell
$ ffmpeg -i input -c:v libx264 -preset veryslow -crf 18 output.mp4
```
Default is `-preset medium -crf 23`


## Concatenate video files
The video files are not re-encoded and must therefore use the same codec.

This example concatenates all `.AVI` files in a directory, such as multiple sequential MJPEG videos.

```shell
$ ffmpeg -f concat -safe 0 -i <(printf "file '$PWD/%s'\n" ./*.AVI) -c copy output.AVI
```

Concatenate and re-encode to H.264:
```shell
$ ffmpeg -f concat -safe 0 -i <(printf "file '$PWD/%s'\n" ./*.AVI) -c:v libx264 output.mp4
```
