> ss.sh
```bash
#!/bin/bash
scrot '/tmp/%F_%T_$wx$h.png' -e 'xclip -selection clipboard -target image/png -i $f' -s
```
> add this to awesome rc.lua to globalkeys variable
```lua
awful.key({ }, "Print", function () awful.util.spawn("/home/vicfred/.misato/ss.sh") end),
```
>lock.sh
```bash
#!/bin/sh

BLANK='#00000000'
CLEAR='#ffffff22'
DEFAULT='#ff00ffcc'
TEXT='#ee00eeee'
WRONG='#880000bb'
VERIFYING='#bb00bbbb'

i3lock \
--insidever-color=$CLEAR     \
--ringver-color=$VERIFYING   \
\
--insidewrong-color=$CLEAR   \
--ringwrong-color=$WRONG     \
\
--inside-color=$BLANK        \
--ring-color=$DEFAULT        \
--line-color=$BLANK          \
--separator-color=$DEFAULT   \
\
--verif-color=$TEXT          \
--wrong-color=$TEXT          \
--time-color=$TEXT           \
--date-color=$TEXT           \
--layout-color=$TEXT         \
--keyhl-color=$WRONG         \
--bshl-color=$WRONG          \
\
--screen 1                   \
--blur 5                     \
--clock                      \
--indicator                  \
--time-str="%H:%M:%S"        \
--date-str="%A, %Y-%m-%d"       \
--keylayout 1                \
```
> To add this to awesome add this to rc.lua to globalkeys variable
```lua
awful.key({ }, "F12", function () awful.util.spawn("/home/vicfred/.misato/lock.sh") end),
```
> wallpaper_changer.sh
```bash
#!/bin/bash
#
# Created by djazz // Dangershy
# Dependencies: feh
#

FOLDER="~/Pictures/wallpapers"
DELAY=10

# to make it loop over lines instead of spaces in filenames
IFS=$'\n';

while true; do
	LIST=`find "$FOLDER" -type f \( -name '*.jpg' -o -name '*.png' \) | shuf`
	for i in $LIST; do
		echo "$i"
#		gsettings set org.gnome.desktop.background picture-uri "file://$i"
		feh "$i" --bg-fill
#		pcmanfm -w "$i"
		sleep ${DELAY}m
	done
	sleep 1
done
```