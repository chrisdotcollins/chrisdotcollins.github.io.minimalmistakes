---
title: "Linux Laptop Notes"
date: 2020-09-03
categories: ["Linux","BunsenLabs","Singularity"]
---

## Display
Two scripts for single and dual monitor display (if can't get auto-HDMI detection working):
Single:
```
#!/bin/sh
xrandr --output Virtual2 --off --output Virtual1 --primary --mode 1280x800 --rotate normal
```
Dual:
```
#!/bin/sh
xrandr --output Virtual2 --mode 1280x800 --primary --rotate normal --output Virtual1 --primary --mode 1280x800 --right-of Virtual2
```
Can then set up key bindings
Edit .xbindkeysrc
```
# Single screen
"~/single.sh"
	Alt + F7
```

```
# Double screen
"~/dual.sh"
	Alt + F8
```

Edit conky.conf:
```
Alt + F7${alignr}Single Monitor
Alt + F8${alignr}Dual Monitor
```

## Terminals
Tilix looks to be a really nice terminal, with the potential of auto profile switching when moving between hosts (help distinguish servers), but I haven't managed to consistently get it working yet. Prefer the master approach here rather than deploying a script to clients:
- https://deeb.me/20190116/change-profiles-automatically-in-tilix-when-connecting-to-ssh-hosts

## Mail 'client'
Use chromium app to have separate window for email. In BunsenLabs this then gains the icon from the favicon, so a nice appropriate and separate icon on tint2 bar

```
[Desktop Entry]
#.local/share/applications/hullemail.desktop
Name=HullMail
Exec=/usr/bin/chromium --app=https://mail.hull.ac.uk/
Terminal=false
X-MultipleArgs=false
Type=Application
Categories=Network;
```

## Singularity

Getting _"ERROR : Failed to create container process: Operation not permitted" build"_ error when attempting to build:
Quick fix
```
echo 1 > /proc/sys/kernel/unprivileged_userns_clone
```

Permanent fix via sysctl
```
echo 'kernel.unprivileged_userns_clone=1' | sudo tee /etc/sysctl.d/00-local-userns.conf
```



## Miscellaneous
- To stop right click close in tint2, edit config and set "mouse_right = none"



