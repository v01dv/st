# My build of st - the simple (suckless) terminal

## Features

- Compatibility with `Xresources`.
- Default [catppuccin frappe](https://github.com/catppuccin/st) colors.
- Transparency/alpha, which is also adjustable from your `Xresources`.
- Default font is system "monospace" at 10pt, meaning the font will match your
  system font from [fontconfig](https://github.com/v01dv/.dotfiles/blob/main/fontconfig/.config/fontconfig/fonts.conf).

## Default Keybindings

NOTE: `Caps Lock` is equal to `ESC` in terminal (nvim etc..)

- `Alt+K` - Increase font size (zoom +)
- `Alt+J` - Decrease font size (zoom -)
- `Alt+Shift+Home` - Returns to default font size 
- `Alt+Up/Down` or `Alt+PageUp/PageDown` or `Shift+WeelScroll` - Scrollback
- `Alt+k/j` or faster with `Alt+u/d` - Scroll up/down in history with nvim-bindings
- `Alt+c`  - Copy text
- `Alt+v` or `Shift+Insert`- Paste text

### Unique features (using dmenu)

- `Alt+l` - Follow urls
- `Alt+y` - Copy urls
- `Alt+o` - Copy the output of commands 

## Patches

Extra stuff added to vanilla st (version 0.9) in order of the applied patches:

- [alpha](https://st.suckless.org/patches/alpha/st-alpha-20220206-0.8.5.diff)
- [font2](https://st.suckless.org/patches/font2/st-font2-0.8.5.diff)
- [boxdraw](https://st.suckless.org/patches/boxdraw/st-boxdraw_v2-0.8.5.diff)
- [scrollback](https://st.suckless.org/patches/scrollback/)
    - [st-scrollback-0.8.5](https://st.suckless.org/patches/scrollback/st-scrollback-0.8.5.diff)
    - [st-scrollback-mouse-20220127-2c5edf2](https://st.suckless.org/patches/scrollback/st-scrollback-mouse-20220127-2c5edf2.diff)
- [externalpipe](https://st.suckless.org/patches/externalpipe/): url opening using [this](https://github.com/LukeSmithxyz/st/blob/master/st-urlhandler) script
    - [st-externalpipe-0.8.4](https://st.suckless.org/patches/externalpipe/st-externalpipe-0.8.4.diff)
    - [st-externalpipe-eternal-0.8.3](https://st.suckless.org/patches/externalpipe/st-externalpipe-eternal-0.8.3.diff)
- [st-xresources-20200604-9ba7ecf](https://st.suckless.org/patches/xresources/st-xresources-20200604-9ba7ecf.diff)
- [ligatures](https://st.suckless.org/patches/ligatures/)
    - [st-ligatures-boxdraw-20221120-0.9](https://st.suckless.org/patches/ligatures/0.9/st-ligatures-boxdraw-20221120-0.9.diff)
    - [st-ligatures-alpha-scrollback-20221120-0.9](https://st.suckless.org/patches/ligatures/0.9/st-ligatures-alpha-scrollback-20221120-0.9.diff)

## Installation

You should have xlib header files and libharfbuzz build files installed.

```
git clone https://github.com/v01dv/st
cd st
sudo make clean install
```

`fontconfig` is required for the default build, since it asks `fontconfig` for 
your system monospace font. It might be obvious, but `libX11` and `libXft` are 
required as well. Chances are, you have all of this installed already.

Be sure to have a composite manager (`xcompmgr`, `picom`, etc.) running if you
want transparency.

## How to configure dynamically with Xresources

For many key variables, this build of `st` will look for X settings set in
either `~/.Xdefaults` or `~/.Xresources`. You must run `xrdb` on one of these
files to load the settings.

For example, you can define your desired fonts, transparency or colors:

```
st.font: Liberation Mono:pixelsize=12:antialias=true:autohint=true;
st.alpha: 0.9
st.color0: #111
...
```

The `alpha` value (for transparency) goes from `0` (transparent) to `1`
(opaque).

### Colors

- This build will use catppuccin colors by default and as a fallback.
- If there are Xresources colors defined, those will take priority.

## TODO

- [ ] Fix color emoji showing. Because for some reason complicated emojis (like flags or complex emojis are made of 
multiple ones) are not showing correctly in st and dmenu as well. But in the same time in kitty terminal they are 
showing correctly.
- [ ] Add change alpha settings (https://github.com/LukeSmithxyz/st/issues/311)
- [ ] [bold is not bright](https://st.suckless.org/patches/bold-is-not-bright/) patch.
- [ ] [glyph_wide_support](https://st.suckless.org/patches/glyph_wide_support/) patch.
- [ ] [themed_cursor](https://st.suckless.org/patches/themed_cursor/) patch.

## Credits

- [LukeSmithxyz](https://github.com/LukeSmithxyz/st)

