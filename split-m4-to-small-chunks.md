# Split a large like 2GB MP4 file into smaller 100-200MB chunks:

## Method 1: FFmpeg (Recommended - Preserves Quality)

### Split by File Size
```bash
# Split into ~100MB chunks
ffmpeg -i input.mp4 -c copy -map 0 -segment_time 600 -f segment -reset_timestamps 1 output_%03d.mp4

> This method is the **simplest approact, one i wud recommend**.This will create files that are approximately 100-200MB each (depending on your video's bitrate) and maintain perfect quality since there's no re-encoding involved.

# For more precise size control (100MB target)
ffmpeg -i input.mp4 -c copy -fs 100M -f segment output_%03d.mp4
```

### Split by Time Duration
```bash
# Split into 10-minute segments (usually ~100-200MB depending on bitrate)
ffmpeg -i input.mp4 -c copy -segment_time 600 -f segment -reset_timestamps 1 output_%03d.mp4

# Split into 15-minute segments
ffmpeg -i input.mp4 -c copy -segment_time 900 -f segment -reset_timestamps 1 output_%03d.mp4
```

### Most Reliable Method (Size-Based)
```bash
# This ensures each part is close to 100MB
ffmpeg -i input.mp4 -c copy -f segment -segment_list_size 0 -segment_time 0 -segment_list_type flat -break_non_keyframes 1 -individual_header_trailer 0 -segment_format_options movflags=+faststart -reset_timestamps 1 -map 0 -f segment -segment_list output_list.txt -segment_size 100M output_%03d.mp4
```

## Method 2: Using MP4Box (Alternative)

```bash
# Install MP4Box (part of GPAC)
# Ubuntu/Debian: sudo apt install gpac
# macOS: brew install gpac

# Split into 600-second (10-minute) segments
MP4Box -split 600 input.mp4

# Split into specific file sizes (100MB)
MP4Box -splits 100M input.mp4
```

## Method 3: Simple FFmpeg with Time Segments

```bash
#!/bin/bash
INPUT="input.mp4"
SEGMENT_TIME=600  # 10 minutes in seconds

# Get total duration
DURATION=$(ffprobe -v quiet -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 "$INPUT")
DURATION=${DURATION%.*}  # Remove decimal part

# Calculate number of segments
SEGMENTS=$((DURATION / SEGMENT_TIME + 1))

# Split the file
for i in $(seq 0 $((SEGMENTS-1))); do
    START=$((i * SEGMENT_TIME))
    ffmpeg -i "$INPUT" -ss $START -t $SEGMENT_TIME -c copy "output_part_$(printf "%03d" $i).mp4"
done
```

## Method 4: One-liner for Quick Splitting

```bash
# Split into 100MB chunks (approximately)
ffmpeg -i input.mp4 -c copy -segment_time 600 -f segment -reset_timestamps 1 output_%03d.mp4

# For 200MB chunks, increase segment_time
ffmpeg -i input.mp4 -c copy -segment_time 1200 -f segment -reset_timestamps 1 output_%03d.mp4
```

## Key Points:

1. **`-c copy`**: Copies streams without re-encoding (fast, no quality loss)
2. **`-segment_time`**: Time in seconds for each segment
3. **`-reset_timestamps 1`**: Resets timestamps for each segment
4. **`%03d`**: Creates numbered files (001, 002, 003, etc.)

## To Determine Optimal Segment Time:

```bash
# Check your video's bitrate first
ffprobe -v quiet -select_streams v:0 -show_entries stream=bit_rate -of default=noprint_wrappers=1:nokey=1 input.mp4

# For 100MB target with 2Mbps video: ~400 seconds per segment
# For 100MB target with 4Mbps video: ~200 seconds per segment
```
