# Keyboard Settings
## Changing a US Keyboard to a Japanese Keyboard
When using a GUI, you need to change the keyboard settings of the virtual emulated terminal.  
### localectl does not work
```
$ localectl set-keymap jp106
```
![image](/CentOS/doc/localectl.png)

### Change with `setxkbmap`
```
$ setxkbmap -layout jp
## or
$ setxkbmap jp -model jp106
```
![image](/CentOS/doc/setxkbmap1.png)

If you perform shutdown or reboot, it will revert to the original settings, so if you want to make it permanent, add it to .bashrc or similar.
![image](/CentOS/doc/setxkbmap2.png)
![image](/CentOS/doc/setxkbmap3.png)

# Changing Folder Names from Japanese to English in the GUI
```
LANG=C xdg-user-dirs-gtk-update
```
[https://toshio-web.com/linux-home-rename?amp=1](https://toshio-web.com/linux-home-rename?amp=1)

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