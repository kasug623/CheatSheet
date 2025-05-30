#os/linux

# TOC <!-- omit in toc -->
- [Keyboard Settings](#keyboard-settings)
  - [Changing a US Keyboard to a Japanese Keyboard](#changing-a-us-keyboard-to-a-japanese-keyboard)
    - [`localectl` does not work](#localectl-does-not-work)
    - [Change with `setxkbmap`](#change-with-setxkbmap)
- [Changing Folder Names from Japanese to English in the GUI](#changing-folder-names-from-japanese-to-english-in-the-gui)
- [Trouble Shooting](#trouble-shooting)
  - [Update Error](#update-error)


# Keyboard Settings
## Changing a US Keyboard to a Japanese Keyboard
When using a GUI, you need to change the keyboard settings of the virtual emulated terminal.  
### `localectl` does not work
```zsh
$ localectl
System Locale: LANG=en_US.UTTF-8
    VC Keymap: us
   X11 Layout: us
$
$ localectl set-keymap jp106
$
$ localectl
System Locale: LANG=en_US.UTTF-8
    VC Keymap: jp106
   X11 Layout: jp
    X11 Model: jp106
  X11 Options: terminate: ctrl_alt_bksp
```
### Change with `setxkbmap`
```zsh
$ setxkbmap -layout jp
## or
$ setxkbmap jp -model jp106
```
```zsh
$ setxkbmap -print
xkb_keymap {
    xkb_keycodes    { include "evdev+aliases(qwery)" };
    xkb_types       { include "complete"} :
    xkb_compat      { include "complete+japan" };
    xkb_symbols     { include "pc+jp+inet(evdev)+terminate(ctrl_alt_bksp)" };
    xkb_geometry    { include "pc(pc104)" };
};
```
If you perform shutdown or reboot, it will revert to the original settings, so if you want to make it permanent, add it to .bashrc or similar.
```zsh
$ echo $SHELL
/bin/bash
$
$ ls -la ~/ | grep "bash"
.bash_history
.bash_logout
.bash_profile
.bashrc
$
$ vim ~/.bashrc
```
# Changing Folder Names from Japanese to English in the GUI
```
$ LANG=C xdg-user-dirs-gtk-update
```
ref. https://toshio-web.com/linux-home-rename?amp=1

# Trouble Shooting
## Update Error
```
sh-4.4# yum update
CentOS Linux 8 - AppStream131
Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist
```
```
$ sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-Linux-*
$ sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-Linux-*
$ dnf clean all
$ dnf update -y
$ dnf install -y openssh-clients
```