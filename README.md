# FFMPEG
Useful tips. Scripts. Function documentation.
See also https://www.ostechnix.com/20-ffmpeg-commands-beginners/
## Synopsis
```bash 
ffmpeg [global_options] {[input_file_options] -i input_url} ... {[output_file_options] output_url} ...
```
global_options:
* ```-y``` do not ask for rewrite output file

## FFMPEG help tips
```bash
# Get avaliable hardware acceleration methods:
ffmpeg -hwaccels
# Get avaliable codecs:
ffmpeg -codecs
# Get avaliable formats:
ffmpeg -formats
# Get avaliable filters:
ffmpeg -filters
```
## Video convertation
```bash 
# Easiest way
ffmpeg -i "input_file" "output_file"
```
## H.264 Video Encoding Guide
For more detailes see [ffmpeg_H.264_wiki](https://trac.ffmpeg.org/wiki/Encode/H.264)

```bash
# Base h264 convertation
ffmpeg -y -hwaccel d3d11va -i "input_file"  -c:v libx264 -crf 23 -preset veryslow -tune zerolatency "output_file.mp4"
```

**List of "-preset" values:**
* ultrafast
* superfast
* veryfast
* faster
* fast
* medium – default preset
* slow
* slower
* veryslow
* placebo – ignore this as it is not useful (see FAQ)

**About "-crf" option:**
The range of the CRF scale is 0–51, where 0 is lossless, 23 is the default, and 51 is worst quality possible. A lower value generally leads to higher quality, and a subjectively sane range is 17–28. Consider 17 or 18 to be visually lossless or nearly so; it should look the same or nearly the same as the input but it isn't technically lossless.
The range is exponential, so increasing the CRF value +6 results in roughly half the bitrate / file size, while -6 leads to roughly twice the bitrate.

**About "-tune" option:**
You can optionally use -tune to change settings based upon the specifics of your input. 
Current tunings include:
* film – use for high quality movie content; lowers deblocking
* animation – good for cartoons; uses higher deblocking and more reference frames
* grain – preserves the grain structure in old, grainy film material
* stillimage – good for slideshow-like content
* fastdecode – allows faster decoding by disabling certain filters
* zerolatency – good for fast encoding and low-latency streaming
* psnr – ignore this as it is only used for codec development
* ssim – ignore this as it is only used for codec development

## Scale video
```bash
ffmpeg -y -i "input_file"  -vf scale=720:-2 "output_file"
# For save aspect ratio set up width or height as -1 (some codecs require -2)
```
## Crop video
```bash
ffmpeg -y -ss 00:06:00.0 -i "input_file" -t 00:02:19.0 "output_file"
# -ss time stamp from
# -t length of output video
```
## Extract frames
To extract all frames: 
```bash
ffmpeg -r 1 -i "input_file" -r 1 "$filename%08d.png"
# -r <fps> set up FPS for input or output video
```
## Remove parts without motion
```bash
ffmpeg -i "input_file" -vf "select=gt(scene\,0.003),setpts=N/(25*TB)" "output_file"
# The higher the number, the more change between frames is ignored, in quick testing you might need to go as low as 0.00001-0.00005 depending on the kind of footage you're dealing with
```
## Batch processing. Scale and noise all *.avi files from directory.
```bash
# Windows CMD
FOR /F "tokens=*" %G IN ('dir /b *.avi') DO ffmpeg -y -i "%G" -vf "scale=iw/2:ih/2,noise=alls=20:allf=t+u" "%~nG.avi"
# Linux terminal
for i in *.avi; do ffmpeg -y -i "$i" -vf "scale=iw/2:ih/2,noise=alls=20:allf=t+u" "${i%.*}.avi"; done
```
[Noise filter documentation](https://ffmpeg.org/ffmpeg-all.html#noise-1)

## Concat video files with black frames betveen
```bash
# create black videofile
ffmpeg -y -ss 00:00:00.0 -i input_1.MOV -t 00:00:1.0 -vf "drawbox=t=fill" "black.MOV"
```
Place black.MOV file to directory with other files. Create files.txt an write there:
```bash
file 'input_1.MOV'
file 'black.MOV'
file 'input_2.MOV'
file 'black.MOV'
file 'input_3.MOV'
```
```bash
# concat files
ffmpeg -f concat -safe 0 -i files.txt -c copy output.MOV
```

# FFPLAY
```bash
ffplay "input_file"
```
```bash
# show time stamp 
# TODO: not work!
ffplay -vf "drawtext=text='%{pts\:hms}':box=1:x=(w-tw)/2:y=h-(2*lh)" "input_file"
```
**While playing:**
* q, ESC - Quit.
* f - Toggle full screen.
* p, SPC - Pause.
* m - Toggle mute.
* 9, 0 - Decrease and increase volume respectively.
* /, * - Decrease and increase volume respectively.
* a - Cycle audio channel in the current program.
* v - Cycle video channel.
* t - Cycle subtitle channel in the current program.
* c - Cycle program.
* w - Cycle video filters or show modes.
* s - Step to the next frame. Pause if the stream is not already paused, step to the next video frame, and pause.
* left/right - Seek backward/forward 10 seconds.
* down/up - Seek backward/forward 1 minute.
* page down/page up - Seek to the previous/next chapter. or if there are no chapters Seek backward/forward 10 minutes.
* **right mouse click - Seek to percentage in file corresponding to fraction of width.**
* left mouse double-click - Toggle full screen.

