# Arch Installer

- Use Ventoy to create an install USB
- After rebooting into the USB run the following

```sh
# Zero drive; first get drive like "nvme0n1"
lsblk -o NAME,SIZE,MODEL
dd if=/dev/urandom of=/dev/nvme0n1 bs=4M status=progress

# Setup keyring
pacman-key --init
pacman-key --populate archlinux
pacman -Syy

# Install
archinstall
```



# Core apps
```sh
sudo pacman -S iwd vi nano git less
```



# Configure git
```sh
git config --global init.defaultBranch main
```



# Touchpad
```sh
sudo nano /etc/X11/xorg.conf.d/30-touchpad.conf

# Then add this
Section "InputClass"
    Identifier "touchpad"
    Driver "libinput"
    MatchIsTouchpad "on"
    Option "Tapping" "on"
    Option "TapButton2" "2"  # Two-finger tap for right-click
    Option "TapButton3" "3"  # Three-finger tap for middle-click
EndSection
```



# Fix resolution by lowering DPI

```sh
# ~/.Xresources
Xft.dpi: 192
xterm*faceName: DejaVu Sans Mono:size=10
xterm*background: #050f0e
xterm*foreground: #ffffff
```

Then to merge the updates and refresh with SHIFT+META+R

```sh
xrdb -merge ~/.Xresources
```



# i3 theme

Add the following to `~/.config/i3/config`:

```
# Start i3bar to display a workspace bar (plus the system information i3status
# finds out, if available)
bar {
  status_command i3status

  colors {
      background #050f0e
      separator #FF1342
      focused_workspace #FF1342 #FF1342 #ffffff
      inactive_workspace #062621 #062621 #12FFBC
      urgent_workspace #ffc826 #ffc826 #050f0e
  }
}

# Window Colors
# Inspiration: https://marketplace.visualstudio.com/items?itemName=TheEdgesofBen.punk-runner

                        # border, bg, text, indicator, child_border
client.focused          #FF1342 #FF1342 #ffffff #FF1342 #FF1342
client.unfocused        #050f0e #050f0e #3D996B #FF1342 #050f0e
client.focused_inactive #333333 #5f676a #ffffff #484e50 #5f676a
client.urgent           #ffc826 #ffc826 #050f0e #ffc826 #ffc826
client.placeholder      #000000 #0c0c0c #ffffff #000000 #0c0c0c
```

then create `~/.config/i3status/config` with:

```sh
mkdir -p ~/.config/i3status
cp /etc/i3status.conf ~/.config/i3status/config
```

Then add this to it:

```
general {
        colors = true
        interval = 5
        color_good = "#12FFBC"
        color_degraded = "#ffc826"
        color_bad = "#FF1342"
        separator = " | "
}
```



# Audio via HDMI

- First list your sinks `pactl list sinks`
- Find the HDMI and set it `pactl set-default-sink <sink-index>`





# Fonts and Emojis
- Install emojis `sudo pacman -S ttf-liberation noto-fonts noto-fonts-emoji ttf-dejavu noto-fonts-emoji`
- Edit `~/.config/fontconfig/fonts.conf`

```
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias>
    <family>sans-serif</family>
    <prefer>
      <family>Noto Sans</family>
      <family>Noto Color Emoji</family>
    </prefer>
  </alias>
  <alias>
    <family>monospace</family>
    <prefer>
      <family>Noto Mono</family>
      <family>Noto Color Emoji</family>
    </prefer>
  </alias>
</fontconfig>xterm*faceName: Liberation Mono
xterm*faceSize: 10
```

- Refresh fonts with `fc-cache -fv`
- Install emoji picker `yay -S x11-emoji-picker`
- Add keyboard shortcut to `~/.config/i3/config` with `bindsym $mod+period exec emoji-picker`

# Screenshot and screen recording

- Install:

```sh
# Screenshots
sudo pacman -S flameshot
```

- Add these keybindings to `~/.config/i3/config`

```sh
bindsym Print exec flameshot gui  # Full GUI interface on PrtScrn
bindsym Shift+Print exec flameshot gui -s  # Direct region selection
```

# Bashrc
```sh
#
# ~/.bashrc
#

# If not running interactively, don't do anything
[[ $- != *i* ]] && return

alias ls='ls --color=auto'
alias grep='grep --color=auto'
PS1='[\u@\h \W]\$ '

alias browser='brave --force-device-scale-factor=1.6 &'
alias vi='vim'

export HEY_BASE="$HOME/github/oz/hey.sh"
export HEY_GIT="$HOME/github/oz/dailies"
export HEY_MODEL='google/gemini-2.0-flash-001'
export API_OPENROUTER='sk-or-v1-'
alias save="$HOME/github/oz/hey.sh/bot/git"
alias chat="$HOME/github/oz/hey.sh/chat"
alias sshadd="eval $(ssh-agent); ssh-add"
```



# ~/.nanorc
```
include "/usr/share/nano/markdown.nanorc"
```
