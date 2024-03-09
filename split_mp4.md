# Split mp4

If you want to just split the video without re-encoding it, use the copy codec for audio and video.
Let say the file is 90 minutes long and we want to split it into 2 files for 45 minutes each
```shell
ffmpeg -ss 00:00:00 -t 00:45:00 -i largefile.mp4 -acodec copy -vcodec copy file1.mp4
ffmpeg -ss 00:45:00 -t 00:90:00 -i largefile.mp4 -acodec copy -vcodec copy file2.mp4
```


## command to split a video file into 10-minute segments without re-encoding
```shell
ffmpeg -i input.mp4 -c copy -map 0 -segment_time 00:10:00 -f segment output%03d.mp4
```
Here's what each part of the command does:

- -i input.mp4: Specifies the input file.
- -c copy: Copies both the audio and video codecs without re-encoding.
- -map 0: Maps all streams (audio, video, subtitles, etc.) from the input file.
- -segment_time 00:10:00: Sets the segment length to 10 minutes.
- -f segment: Specifies that the output should be segmented.
- output%03d.mp4: Specifies the output file pattern. %03d will be replaced by the segment number, padded with zeros to three digits (e.g., output000.mp4, output001.mp4, etc.).
This command will generate a series of files (output000.mp4, output001.mp4, etc.) each with a maximum duration of 10 minutes.
