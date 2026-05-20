# FFmpeg Cheatsheet

**FFmpeg** is a powerful, open-source multimedia framework for handling video, audio, and subtitles.  
**Basic syntax**: `ffmpeg [global options] -i [input] [options] [output]`

---

## 1. Basic Commands

| Task | Command |
|------|---------|
| Convert video format | `ffmpeg -i input.mp4 output.mkv` |
| Extract audio | `ffmpeg -i input.mp4 -vn output.mp3` |
| Extract video (no audio) | `ffmpeg -i input.mp4 -an output.mp4` |
| Copy streams (fast, no re-encode) | `ffmpeg -i input.mkv -c copy output.mp4` |
| Re-encode with specific codec | `ffmpeg -i input.mp4 -c:v libx264 -c:a aac output.mp4` |

---

## 2. Video Options

```bash
# Resolution / Scaling
ffmpeg -i input.mp4 -vf scale=1920:1080 output.mp4
ffmpeg -i input.mp4 -vf scale=1280:-1 output.mp4   # Keep aspect ratio

# Frame rate
ffmpeg -i input.mp4 -r 30 output.mp4

# Bitrate
ffmpeg -i input.mp4 -b:v 5000k output.mp4          # Video bitrate
ffmpeg -i input.mp4 -b:a 192k output.mp4           # Audio bitrate

# Quality (CRF for x264/x265)
ffmpeg -i input.mp4 -c:v libx264 -crf 23 output.mp4   # 18-28 range (lower = better)

# Codec presets
ffmpeg -i input.mp4 -c:v libx264 -preset slow -crf 22 output.mp4
# Presets: ultrafast, superfast, veryfast, faster, fast, medium, slow, slower, veryslow

```
## 3. Audio Options

```bash
# Audio codecs
ffmpeg -i input.mp4 -c:a aac output.mp4            # AAC (most compatible)
ffmpeg -i input.mp4 -c:a libmp3lame output.mp3
ffmpeg -i input.mp4 -c:a libvorbis output.ogg
ffmpeg -i input.mp4 -c:a copy output.mkv           # Stream copy

# Volume
ffmpeg -i input.mp4 -af volume=1.5 output.mp4      # 150% volume
ffmpeg -i input.mp4 -af volume=-10dB output.mp4

# Channels / Sample rate
ffmpeg -i input.mp4 -ac 2 -ar 44100 output.mp4
```

## 4. Trimming & Cutting

```bash
# Trim from start (first 60 seconds)
ffmpeg -i input.mp4 -t 60 output.mp4

# Trim from specific time
ffmpeg -i input.mp4 -ss 00:01:30 -t 30 output.mp4   # Start at 1:30, 30 seconds long

# Accurate trim (slower, more precise)
ffmpeg -i input.mp4 -ss 00:01:30 -t 30 -c copy output.mp4   # Fast but may lose accuracy
```

## 5. Common Filters (`-vf` / `-af`)

| Filter | Command |
|--------|---------|
| Crop | `ffmpeg -i in.mp4 -vf crop=1280:720:0:0 out.mp4` |
| Rotate 90° clockwise | `ffmpeg -i in.mp4 -vf transpose=1 out.mp4` |
| Horizontal flip | `ffmpeg -i in.mp4 -vf hflip out.mp4` |
| Vertical flip | `ffmpeg -i in.mp4 -vf vflip out.mp4` |
| Grayscale | `ffmpeg -i in.mp4 -vf hue=s=0 out.mp4` |
| Text overlay | `ffmpeg -i in.mp4 -vf "drawtext=text='Hello':fontcolor=white:fontsize=24:x=10:y=10" out.mp4` |
| Fade in/out | `ffmpeg -i in.mp4 -vf fade=in:0:30,fade=out:270:30 out.mp4` |
| Speed up video (2x) | `ffmpeg -i in.mp4 -vf "setpts=0.5*PTS" out.mp4` |
| Slow down video (0.5x) | `ffmpeg -i in.mp4 -vf "setpts=2*PTS" out.mp4` |
| Speed up audio (2x) | `ffmpeg -i in.mp4 -af atempo=2 out.mp4` |

**Multiple filters** (comma-separated):
```bash
ffmpeg -i input.mp4 -vf "scale=1280:720,hflip,crop=640:480" output.mp4
```

## 6. Subtitles

```bash
# Burn subtitles into video
ffmpeg -i video.mp4 -vf subtitles=subs.srt output.mp4

# Burn with styling
ffmpeg -i video.mp4 -vf "subtitles=subs.srt:force_style='Fontsize=24,PrimaryColour=&HFFFFFF&'" output.mp4

# Extract subtitles
ffmpeg -i input.mkv -map 0:s:0 subs.srt

# Add subtitle stream without burning
ffmpeg -i video.mp4 -i subs.srt -c copy -c:s mov_text output.mp4
```

## 7. Advanced / Multiple Inputs

```bash
# Concatenate videos (same codecs & parameters)
echo "file 'part1.mp4'\nfile 'part2.mp4'" > list.txt
ffmpeg -f concat -safe 0 -i list.txt -c copy output.mp4

# Overlay image/video
ffmpeg -i video.mp4 -i logo.png -filter_complex "overlay=10:10" output.mp4

# Picture-in-picture
ffmpeg -i main.mp4 -i pip.mp4 -filter_complex "[0:v][1:v]overlay=10:10[v]" -map "[v]" -map 0:a output.mp4

# Screen recording (Linux/X11)
ffmpeg -f x11grab -framerate 30 -i :0.0 output.mp4

# Screen recording with audio (PulseAudio)
ffmpeg -f x11grab -framerate 30 -i :0.0 -f pulse -i default output.mp4
```

## 8. Hardware Acceleration

```bash
# NVIDIA NVENC (H.264)
ffmpeg -hwaccel cuda -i input.mp4 -c:v h264_nvenc output.mp4

# NVIDIA NVENC (H.265)
ffmpeg -hwaccel cuda -i input.mp4 -c:v hevc_nvenc output.mp4

# Intel Quick Sync
ffmpeg -hwaccel qsv -i input.mp4 -c:v h264_qsv output.mp4

# Apple VideoToolbox (macOS)
ffmpeg -i input.mp4 -c:v h264_videotoolbox output.mp4
```

## 9. Useful Global Options

- `y` → Overwrite output without asking
- `n` → Never overwrite output
- `hide_banner -loglevel error` → Clean, minimal output
- `stats` → Show encoding progress
- `map 0:v:0 -map 0:a:0` → Select specific streams
- `movflags +faststart` → Web-optimized MP4 (move moov atom)

## 10. Image Conversion: WebP ↔ JPG / PNG

### Single File Conversion

```bash
# WebP to JPG
ffmpeg -i input.webp output.jpg

# WebP to PNG (lossless)
ffmpeg -i input.webp output.png

# JPG to WebP
ffmpeg -i input.jpg output.webp

# PNG to WebP
ffmpeg -i input.png output.webp
```

### With Quality Control

```bash
# WebP → JPG with quality (scale 2-31, lower = better)
ffmpeg -i input.webp -q:v 2 output.jpg

# WebP → PNG with compression level (0-9)
ffmpeg -i input.webp -compression_level 6 output.png

# JPG → WebP with quality (0-100, higher = better)
ffmpeg -i input.jpg -q:v 80 output.webp
```

### Batch Conversion (Linux / macOS / Git Bash)

```
# Convert all .webp files to .jpg
for f in *.webp; do ffmpeg -i "$$   f" "   $${f%.webp}.jpg"; done

# Convert all .webp files to .png
for f in *.webp; do ffmpeg -i "$$   f" "   $${f%.webp}.png"; done
```


## Quick Reference Table

| Goal | One-liner Command |
|------|-------------------|
| MP4 → GIF | `ffmpeg -i input.mp4 -vf "fps=10,scale=320:-1:flags=lanczos" -c:v gif output.gif` |
| High quality H.265 | `ffmpeg -i input.mp4 -c:v libx265 -crf 28 output.mkv` |
| Web optimized MP4 | `ffmpeg -i input.mp4 -movflags +faststart -c:v libx264 -crf 23 output.mp4` |
| Extract frames as images | `ffmpeg -i input.mp4 -vf fps=1 img%04d.png` |
| Convert to VP9 (WebM) | `ffmpeg -i input.mp4 -c:v libvpx-vp9 -crf 30 output.webm` |
| Remove audio from video | `ffmpeg -i input.mp4 -an -c:v copy output.mp4` |
| Convert audio only to MP3 | `ffmpeg -i input.mp4 -vn -c:a libmp3lame -q:a 2 output.mp3` |
| Resize and compress for web | `ffmpeg -i input.mp4 -vf scale=1280:-1 -c:v libx264 -crf 23 -preset medium output.mp4` |

---

**Tips**:
- Always use `-c copy` when possible for maximum speed (no re-encoding).
- Test commands on short clips using `-ss` and `-t`.
- Prefer CRF-based encoding over fixed bitrate for better quality/size balance.
- Check supported formats and codecs: `ffmpeg -formats` and `ffmpeg -codecs`.
- For complex filter chains, use `-filter_complex` and label outputs with `[v]` or `[a]`.

For full documentation: [https://ffmpeg.org/documentation.html](https://ffmpeg.org/documentation.html)

*Last updated: May 2026*

