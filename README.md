# FFMPEG
Useful tips. Scripts. Function documentation.
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



