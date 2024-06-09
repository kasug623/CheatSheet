# キーボード設定
## USキーボードを日本語キーボードに変える
### GUIを使っているときは仮想エミュレート端末のキーボード設定を変える必要がある
#### localctlでは変わらない
```
localectl set-keymap jp106
```
![参考コンソール画像](/CentOS/doc/localectl.png)

# setxkbmapで変える
```
setxkbmap -layout jp
```
or
```
setxkbmap jp -model jp106
```
![参考コンソール画像](/CentOS/doc/setxkbmap1.png)

- shutdownやrebootとすると、元に戻るので、永続化させたい場合は、.bashrc等に記載する
![参考コンソール画像](/CentOS/doc/setxkbmap2.png)
![参考コンソール画像](/CentOS/doc/setxkbmap3.png)

# フォルダ名が日本語なのを英語に変えたい at GUI
```
LANG=C xdg-user-dirs-gtk-update
```
[https://toshio-web.com/linux-home-rename?amp=1](https://toshio-web.com/linux-home-rename?amp=1)


## ERROR
sh-4.4# yum update
CentOS Linux 8 - AppStream131
Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist
```
$ sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-Linux-*
$ sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-Linux-*
$ dnf clean all
$ dnf update -y
$ dnf install -y openssh-clients
```