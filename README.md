# FFMPEG
Useful tips. Scripts. Function documentation.
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
### Synopsis
```bash 
ffmpeg [global_options] {[input_file_options] -i input_url} ... {[output_file_options] output_url} ...
```
global_options:
* ```-y``` do not ask for rewrite output file
 
