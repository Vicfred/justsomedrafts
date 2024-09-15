# ONLINE BACKUP

resize to 3456x1944 (90% of 4k resolution) and 75% image quality

normal videos: resize to 2/3 (ie. from 1080p to 720p) and compress videos at crf 32
important videos: compress at crf 32 - 28 (lower is better quality)

# LOCAL BACKUP

store local RAW files for the camera
backup pixel 8 pro photos at 100% resolution and 85% image quality

normal videos: NO resize and compress videos at crf 35 (24 - 26 for x265)
important videos: compress videos at crf 28 (20-24 for x265) (lower is better quality)
___

> batch\_image\_compress.sh
```bash
#!/bin/bash
for image_file in *.jpg; do
  compression=85
  # no resize
  magick $image_file -quality $compression compressed/$image_file
  echo "Compressing" $image_file "by" $compression"%"
done
```

> batch\_image\_resize\_compress.sh
```bash
#!/bin/bash
for image_file in *.jpg; do
  compression=75
  height=3456
  width=1944
  # resize
  # add blur to make it smaller
  magick $image_file -interlace Plane -gaussian-blur 0.05 -quality $compression -resize ${height}x${width}\> compressed/$image_file
  echo "Compressing" $image_file "by" $compression"%" and resizing to ${height}x${width}
done
```

> batch_video_archive.sh
```bash
#!/bin/bash
for video_file in *.mp4; do
  filename=$(basename -- "$video_file")
  extension="${filename##*.}"
  filename="${filename%.*}"
  # set appropiate crf according to how important the video is
  # av1, default crf is 35 lower is better quality, default preset is 10 lower is slower
  # normal video, not so important
  ffmpeg -i "$video_file" -c:v libsvtav1 -map_metadata 0 -movflags use_metadata_tags -crf 28 -preset 4 -c:a libopus -b:a 128k compressed/"$filename".mp4
  # important video, consider not re-encoding instead, maybe only use for professional camera videos
  #ffmpeg -i "$video_file" -c:v libsvtav1 -map_metadata 0 -movflags use_metadata_tags -crf 24 -preset 4 -c:a libopus -b:a 128k compressed/"$filename".mp4

  # x265 DEPRECATED FOR ARCHIVING
  #ffmpeg -i "$video_file" -c:v libx265 -map_metadata 0 -movflags use_metadata_tags -crf 24 -preset slow -c:a aac -b:a 128k compressed/"$filename".mp4
done
```

>  batch_video_online.sh
```bash
#!/bin/bash
for video_file in *.mp4; do
  filename=$(basename -- "$video_file")
  extension="${filename##*.}"
  filename="${filename%.*}"
  # 2/3 of 1080p is 720p
  # av1
  ffmpeg -i "$video_file" -c:v libsvtav1 -vf scale="iw*2/3:ih*2/3" -map_metadata 0 -movflags use_metadata_tags -crf 50 -preset 6 -c:a libopus -b:a 96k compressed/"$filename".mp4
  # 720p vertical video
  #ffmpeg -i "$video_file" -c:v libsvtav1 -vf scale="720:-1" -map_metadata 0 -movflags use_metadata_tags -crf 50 -preset 6 -c:a libopus -b:a 96k compressed/"$filename".mp4
  # x265 DEPRECATED
  #ffmpeg -i "$video_file" -c:v libx265 -vf scale="iw*2/3:ih*2/3" -map_metadata 0 -movflags use_metadata_tags -crf 32 -preset slow -c:a aac -b:a 96k compressed/"$filename".mp4
done
```

> add_bars_instagram.sh
```bash
#!/bin/bash
for image_file in *.jpg; do
  width=$(identify -format '%w' "$image_file")
  height=$(identify -format '%h' "$image_file")
  width_difference=$((height * 4/5 - width))
  bar_size=$((width_difference/2))
  echo "adding bar of size" $bar_size
  convert $image_file -bordercolor black -border $((bar_size))x0 -quality 100 $image_file
  echo "resizing from" $width "x" $height "to 1080x1350"
  convert $image_file -resize 1080x1350 -quality 95 $image_file
done
```
___

# Video utilities
metadata
```
ffprobe input.mp4
```

(no export)
```
INPUT_VIDEO=input.mp4
```

default crf is 28 (lower = better quality), default preset is medium, preserve metadata, re-encode audio

HALF THE SIZE OF DEFAULT ANDROID ENCODING
```
ffmpeg -i $INPUT_VIDEO -c:v libx265 -map_metadata 0 -movflags use_metadata_tags -crf 28 -preset slow -c:a aac -b:a 128k output.mp4
```

crf 26 (lower = better quality), slow preset, preserve metadata, re-encode audio
```
ffmpeg -i $INPUT_VIDEO -c:v libx265 -map_metadata 0 -movflags use_metadata_tags -crf 26 -preset slow -c:a aac -b:a 128k output.mp4
```

downsize to 2/3 (1080p to 720p)
```
ffmpeg -i $INPUT_VIDEO -c:v libx265 -vf scale="iw*2/3:ih*2/3" -map_metadata 0 -movflags use_metadata_tags -crf 28 -preset slow -c:a aac -b:a 128k output.mp4
```

remove audio
```
ffmpeg -i $INPUT_VIDEO -c:v libx265 -map_metadata 0 -movflags use_metadata_tags -an output.mp4
```

> batch rename
```bash
#!/bin/bash extension
for f in *.MP4; do mv -- "$f" "${f%.MP4}.mp4"; done
```
___

# Exif utilities
Add or substract X hours to photo's date
```
exiftool -alldates+=X -filemodifydate+=X -filecreatedate+=X -overwrite_original *.JPG
```

Add 1 year and 2 months
```
exiftool -alldates+="1:2:0 0" FILE
```

Change photo modification date to match exif timestamp
```
exiv2 -T rename *.jpg
```

Change .JPG extension to .jpg
```
for f in *.JPG; do mv -- "$f" "${f%.JPG}.jpg"; done
```

Set jpg filenames to photo original datetime (this is most likely the one I want)
```
exiftool -ext jpg '-FileName<DateTimeOriginal' -d %Y_%m_%d__%H_%M_%S%%-c.%%e .
```

Set jpg filenames to photo creation file
```
exiftool -ext jpg '-FileName<CreateDate' -d %Y_%m_%d__%H_%M_%S%%-c.%%e .
```

View photo metadata
```
exif input.jpg
```

View photo metadata using imagemagick
```
identify -verbose input.jpg
```

Sometimes the file created date is wrong even if the taken date is right (check with ls -al and exif file.JPG)
```
exiftool -filemodifydate+=7 -overwrite_original *.JPG
```

Resize and compress for pixel 8 pro @ 50MP
```
mogrify -resize 30% -quality 90% *.jpg
```

Change filename of mp4 to match creation date
```
exiftool -ext mp4 '-FileName<MediaCreateDate' -d %Y_%m_%d__%H_%M_%S%%-c.%%e .
```

Run this command to see all the time related tags in the file
```
exiftool -time:all -G1 -a -s file.mp4
```

Copy all metadata from one video to another
```
exiftool -tagsFromFile source.mp4 -All:All output.mp4
```

Set specific date
```
exiftool -AllDates="2023-04-02 08:01:00" 2023_04_02__08_01_43.jpg
```

Copy GPS information from one picture into all pictures of a directory 
```
exiftool -tagsfromfile SRCFILE -gps:all -wm cg DIR
```
___

# Scripts
```python
#!/usr/bin/env python
import os
import shutil
import argparse
from pathlib import Path

parser = argparse.ArgumentParser("backup file finder")
parser.add_argument("inputdir", help="input directory")
#folder_dir = "/home/vicfred/arana/photo/25_04_01_test/"
args = parser.parse_args()
folder_dir = args.inputdir
backup_dir = "/home/vicfred/Downloads/Takeout/googlephotos/Bin/"
dst_dir = "/home/vicfred/arana/photo/99_99_99_dst/"
total_files = 0
num_found = 0
num_notfound = 0
for medianame in os.listdir(folder_dir):
    if(medianame.endswith(".jpg") or medianame.endswith(".mp4") or medianame.endswith(".JPG") or medianame.endswith(".MP4")):
        total_files += 1
        backuppath = Path(backup_dir + medianame)
        if(backuppath.is_file()):
            num_found += 1
            shutil.copy2(backup_dir + medianame, dst_dir + medianame)
        else:
            num_notfound += 1
            print(folder_dir + medianame + " not found.")

print("found", num_found, "files", "out of", total_files, "and", num_notfound, "not found.")
```