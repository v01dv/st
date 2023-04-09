# My build of st - the simple (suckless) terminal

## Features

- Compatibility with `Xresources`.
- Transparency/alpha, which is also adjustable from your `Xresources`.
- Default [catppuccin frappe](https://github.com/catppuccin/st) colors.
- Default font is system "monospace" at 13px, meaning the font will match your.
  system font from [fontconfig](https://github.com/v01dv/.dotfiles/blob/main/fontconfig/.config/fontconfig/fonts.conf).
- OpenType font [features](https://en.wikipedia.org/wiki/List_of_typographic_features#OpenType_typographic_features) support:
    - [JetBrains Mono](https://github.com/JetBrains/JetBrainsMono/wiki/OpenType-features)
    - [Fira Code](https://github.com/tonsky/FiraCode/wiki/How-to-enable-stylistic-sets)
    - [Cascadia Code](https://github.com/microsoft/cascadia-code#font-features)
    - [Victor Mono](https://github.com/rubjo/victor-mono#font-stylistics)
    - [Iosevka](https://github.com/be5invis/Iosevka/blob/main/doc/character-variants.md)

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
- [ligatures](https://st.suckless.org/patches/ligatures/): also allows to enable OpenType font features.
    - [st-ligatures-boxdraw-20221120-0.9](https://st.suckless.org/patches/ligatures/0.9/st-ligatures-boxdraw-20221120-0.9.diff)
    - [st-ligatures-alpha-scrollback-20221120-0.9](https://st.suckless.org/patches/ligatures/0.9/st-ligatures-alpha-scrollback-20221120-0.9.diff)

## Fonts

The following fonts are necessary in order to run this build of st properly:

- DejaVu (properly displays emojis) ([#346](https://github.com/LukeSmithxyz/st/issues/346#issuecomment-1413141154))
- Noto Color Emoji (colored emoji support) ([#159](https://github.com/LukeSmithxyz/st/issues/159#issuecomment-583699256))
- Noto CJK fonts (for cat nose in the terminal banner)
- Iosevka (for `🞔 ` and `🞕 ` symbols in the terminal prompt)
- Symbols Nerd Font (for icons)
- Cascadia Code, Fira Code or JetBrains Mono (for now I use first one as default monospace font)

Set your fallback fonts (font2), specifically your emoji fonts at a lower size that will avoid icon cropping (e.g. git status icon in nvim-tree plugin)([#67](https://github.com/LukeSmithxyz/st/issues/67#issuecomment-583699156)) 

### How to enable OpenType features in st ([#315](https://github.com/LukeSmithxyz/st/pull/315))

1. Install [ligatures](https://st.suckless.org/patches/ligatures/) patch.
2. To enable necessary OpenType features you have to add it to the `hb_features` array in `hb.c':
```c
hb_feature_t features[] = { FEATURE('s','s','0','1'), FEATURE('s','s','0','2'), FEATURE('z','e','r','o') };
```
3. Recompile st.
4. Test the font feature by command:
```
echo 'impl self r ~= 0' | pango-view --font="Cascadia Code Italic" /dev/stdin
```
### How to enable OpenType features by leveraging the [fontconfig library’s rule](https://protesilaos.com/codelog/2019-07-25-opentype-features-fontconfig/) declaration.

This method works only with fontconfig-aware applications like SpaceFM, Firefox etc.

1. Create preset ~/.config/fontconfig/conf.d/80-font-features.conf:
```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <description>Enable OpenType typographic features.</description>
  <match target="scan">
    <test name="family" compare="eq" ignore-blanks="true">
      <string>Cascadia Code</string>
    </test>
      <edit name="fontfeatures" mode="append">
        <string>ss01 on</string> <!-- stylistic alternates (italic only) -->
        <string>ss02 on</string> <!-- alternate not equal -->
        <string>ss19 on</string> <!-- slashed zero -->
        <string>ss20 on</string> <!-- graphical control characters -->
    </edit>
  </match>
</fontconfig>
```

2. Rebuild font database:
```
fc-cache -f
```

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

