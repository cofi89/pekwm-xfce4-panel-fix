# pekwm-xfce4-panel-fix
This script fixes the problem of windows maximizing over or under xfce4-panel, when it's used in pekwm.

Arguments:
----------
 -w  -- Use wmctrl method ( preffered )
 
 -x	 -- Use xdotool method
 
 -c  -- Check if necessary programs are installed

Info:
-----
Pekwm recognizes only "NET_WM_STRUT" hint, which xfce4-panel doesn't set.
It sets only a "NET_WM_STRUT_PARTIAL".
However non-partial strut can be set from partial's coordinates, and that is 
what this script does. With one of the two methods:

1. Using "wmctrl" to obtain window id(s) of xfce panel, and providing it/them 
to the "xprop" tool, which then gets partial strut and sets a non-partial one 
based on it.

2. Using  "xdotool" to automatically "click" on the xfce panel(s), thus 
providing "xprop" tool with the panel window(s) to act on, which then gets 
partial strut and sets a non-partial one based on it.

First method is preffered, and one which should be used in most cases. 
The only problem with it is that if there happens to be another proccess 
having a "panel" in it's name it will also get a strut set, which is obviously
undesired ( although it shouldn't cause problems ).

Second method only requires that panel is actually visible on screen when 
xdotool "clicks" on it. Default waiting time should be good enough for that.
Also, it requires you to provide coordinates to "click" on. Defaults are for 
one panel on the bottom of a 1920x1080 screen. If you need to change that, 
take a look at the "fix-xdotool" function starting at line 83.

Autostarting:
-------------
Script should be added to the pekwm autostart (~/.pekwm/start), **AFTER**
xfce4-panel entry, preferably with sleep ( 2-3s should be good ), giving panel 
enough time to start and show up on screen ( important for "xdotool" method ). 

Example:
xfce4-panel &
(sleep 2s && /path/to/pekwm-xfce4-panel-fix -w) &

Dependencies:
-------------
- wmctrl
- xdotool 
- xprop

You can use the script to check if they are all installed by running it with "-c" option. 

Xprop is usually installed in major distro's, but in case it's not, it is in:
"xorg-xprop" - Arch/Manjaro
"x11-utils" - Debian/Ubuntu
"xorg-x11-utils" - Fedora/RH
"xprop" - OpenSuse
