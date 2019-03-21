# xps13-config

Personal configuration files for Ubuntu on a Dell XPS 13


## Keyboard layout ##

`usr/share/X11/xkb` contains
[XKB](https://en.wikipedia.org/wiki/X_keyboard_extension)
configuration files for achieving a UNIX-like layout on today's
prevailing U.S. PC keyboards, where Caps lock is on the left edge of
the keyboard, Control is in the lower left corner, Escape is in the
top left corner beside the function keys, Backspace is one row below
on the right edge, and Backslash is one row below that one and also on
the right edge.

By "UNIX-like," I mean the layout found on the [Happy Hacking
Keyboard](https://en.wikipedia.org/wiki/Happy_Hacking_Keyboard), where

- Control is the leftmost key on the home row,
- Escape is directly to the left of the number keys,
- Backspace (or Delete) is directly above Enter,
- Backslash is above Backspace, and
- there is no dedicated Caps lock key.

The Happy Hacking and Sun Type 3 keyboards split what is the Backspace
key on PC keyboards into two—Backslash and Tilde—so I put Tilde
elsewhere.


### Installation ###

To install this layout:

```bash
mv /usr/share/X11/xkb/rules/evdev.lst /usr/share/X11/xkb/rules/evdev.lst.orig
mv /usr/share/X11/xkb/rules/evdev.xml /usr/share/X11/xkb/rules/evdev.xml.orig
cp usr/share/X11/xkb/rules/evdev.lst usr/share/X11/xkb/rules/evdev.xml \
   /usr/shareX11/xkb/rules/
cp usr/share/X11/xkb/symbols/us-unix /usr/shareX11/xkb/symbols/
```

To apply this layout on the built-in keyboard of a Dell XPS 13 (9380):

```bash
setxkbmap -layout us-unix -option
setxkbmap -layout us-unix -option "altwin:swap_alt_win,ctrl:rctrl_ralt"
```

#### i3 ####

To use in [i3](https://en.wikipedia.org/wiki/I3_(window_manager)), you
can create a script to set the layout and invoke the script from the
i3 configuration file.

For example, the contents of `~/.local/bin/setkeyboardlayout`:

```bash
#!/usr/bin/env bash

if setxkbmap -layout us-unix -option &>/dev/null; then
    setxkbmap -layout us-unix -option "altwin:swap_alt_win,ctrl:rctrl_ralt"
    exit
fi

# On a US layout:
# - Swap Caps lock and left Control
# - Swap Alt and Super
# - Make right Control invoke right Alt
setxkbmap -layout us -option
setxkbmap -layout us -option "ctrl:swapcaps,altwin:swap_alt_win,ctrl:rctrl_ralt"
```

Then add this line to `~/.config/i3/config`:

```
exec --no-startup-id ~/.local/bin/setkeyboardlayout
```


### Reference ###

Some helpful information for defining keyboard layouts in XKB:

- [An Unreliable Guide to XKB Configuration](http://www.charvolant.org/doug/xkb/html/)
  - [5 XKB Configuration Files](http://www.charvolant.org/doug/xkb/html/node5.html)
- [A simple, humble but comprehensive guide to XKB for linux](https://medium.com/@damko/a-simple-humble-but-comprehensive-guide-to-xkb-for-linux-6f1ad5e13450)
  - [damko/xkb_kinesis_advantage_dvorak_layout: A US variant layout for XKB that supports Italians deadkeys. Especially meant for Kinesis Advantage keyboard](https://github.com/damko/xkb_kinesis_advantage_dvorak_layout)
- [Creating custom keyboard layouts for X11 using XKB](https://michal.kosmulski.org/computing/articles/custom-keyboard-layouts-xkb.html)
- [10.10 - Simplest way to swap esc key with ` key - Ask Ubuntu](https://askubuntu.com/questions/32966/simplest-way-to-swap-esc-key-with-key)
  - [ubuntu - keyboard layout howto](https://ubuntuforums.org/showthread.php?p=10286878#post10286878)
