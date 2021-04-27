# My build of st based on LukeSmithxyz's build 

The [suckless terminal (st)](https://st.suckless.org/) with some additional
features that make it literally the best terminal emulator ever.

## Unique features (using dmenu)

+ **follow urls** by pressing `alt-l`
+ **copy urls** in the same way with `alt-y`
+ **copy the output of commands** with `alt-o`

## Bindings for

+ scroll with `alt-↑/↓` or `alt-pageup/down` or `shift` while scrolling the
  mouse. 
+ OR **vim-bindings**: scroll up/down in history with `alt-k` and `alt-j`.
  Faster with `alt-u`/`alt-d`.
+ **zoom/change font size**: same bindings as above, but holding down shift as
  well. `alt-home` returns to default
+ **copy text** with `alt-c`, **paste** is `alt-v` or `shift-insert`

## Pretty stuff

+ Compatibility with `Xresources`

## Applied patches

+ [st-alpha](http://st.suckless.org/patches/alpha/)
+ [st-nordtheme](http://st.suckless.org/patches/nordtheme/)
+ [st-xresources](http://st.suckless.org/patches/x:resources/)
+ [st-boxdraw](http://st.suckless.org/patches/boxdraw/)
+ [st-font2](http://st.suckless.org/patches/font2/)
+ [st-scrollback](http://st.suckless.org/patches/scrollback/)
+ [st-ligatures](http://st.suckless.org/patches/ligatures/) : st-ligatures-alpha, st-ligatures-boxdraw, st-ligatures-scrollback
+ [st-externalpipe](http://st.suckless.org/patches/externalpipe/) + st-externalpipe-eternal

## Installation 

You should have xlib header files and libharfbuzz build files installed.

Be sure to have a composite manager (`xcompmgr`, `picom`, etc.) running if you
want transparency.

## How to configure dynamically with Xresources

For many key variables, this build of `st` will look for X settings set in
either `~/.Xdefaults` or `~/.Xresources`. You must run `xrdb` on one of these
files to load the settings.

For example, you can define your desired fonts, transparency or colors:

```
*.font:	Liberation Mono:pixelsize=12:antialias=true:autohint=true;
*.alpha: 0.9
*.color0: #111
...
```
The `alpha` value (for transparency) goes from `0` (transparent) to `1`
(opaque).

### Colors

To be clear about the color settings:

- This build will use nordtheme colors by default and as a fallback.
- If there are Xresources colors defined, those will take priority.

## Notes on Emojis and Special Characters

If st crashes when viewing emojis, install
[libxft-bgra](https://aur.archlinux.org/packages/libxft-bgra/) from the AUR.

Note that some special characters may appear truncated if too wide. You might
want to manually set your prefered emoji/special character font to a lower size
in the `config.h` file to avoid this. By default, JoyPixels is used at a
smaller size than the usual text.

