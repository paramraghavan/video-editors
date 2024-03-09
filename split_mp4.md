# Split mp4

If you want to just split the video without re-encoding it, use the copy codec for audio and video.
Let say the file is 90 minutes long and we want to split it into 2 files for 45 minutes each
```shell
ffmpeg -ss 00:00:00 -t 00:45:00 -i largefile.mp4 -acodec copy -vcodec copy file1.mp4
ffmpeg -ss 00:45:00 -t 00:90:00 -i largefile.mp4 -acodec copy -vcodec copy file2.mp4
```
