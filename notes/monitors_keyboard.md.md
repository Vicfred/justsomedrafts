Create a directory for the configurations

```
mkdir -p /etc/X11/xorg.conf.d
```

get the identifier from x11-apps/xrandr

```
xrandr -q
```

create the configuration for the monitors in /etc/X11/xorg.conf.d/10-monitor.conf
(looks like /etc/X11/xorg.conf.d/50-video.conf is not needed)

```bash
# Section "Monitor"
# 	Identifier "DP0"
# EndSection
#
# Section "Monitor"
# 	Identifier "DP2"
# 	Option "RightOf" "DP0"
# EndSection
#
# Section "Device"
# 	Identifier "Device0"
# 	Option "DisplayPort-0" "DP0"
# 	Option "DisplayPort-2" "DP2"
# 	driver "amdgpu"
# EndSection

# Section "Monitor"
#     Identifier  "DisplayPort-2"
#     Option      "Primary" "true"
# EndSection
#
# Section "Monitor"
#     Identifier  "HDMI-A-0"
#     Option      "LeftOf" "DisplayPort-2"
# EndSection
```

References:

https://wiki.gentoo.org/wiki/Xorg/Multiple_monitors

https://wiki.archlinux.org/title/multihead

___

>/etc/X11/xorg.conf.d/00-keyboard.conf
```bash
Section "InputClass"
        Identifier "system-keyboard"
        MatchIsKeyboard "on"
        Option "XkbLayout" "us"
        Option "XkbModel" "pc105"
	Option "XkbOptions" "compose:ralt,caps:swapescape"
	Option "AutoRepeat" "3000 25"
EndSection
```