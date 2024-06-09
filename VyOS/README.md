# install
```
$ install image
	Enter Enter ...
$ reboot
	Yes
```
# ESXi上でVyOSをRouter代わりにする場合
ESXi上でネットワークアダプタを二つ設定する

# vyosで設定
```
install image 
```
[]のついてる質問は、デフォルトの答えなので、
エンターキーを押せば勝手にその値が入る。
基本手金エンターだけでよく、最初から3番目の質問の時にNoではなくYesと入力すれば、
あとは連打でOK

```
show config
```
でインターフェースが二つ出ていればNAT設定ができる

```
vyos@vyos:~$ configure
vyos@vyos#　← 設定モードの表記
```

```
set protocols static route 0.0.0.0/0 next-hop XXX.XXX.XXX.XXX
```


```
vyos@vyos# set interfaces ethernet eth0 address YYY.YYY.YYY.YYY/24
vyos@vyos# set interfaces ethernet eth0 description Client
vyos@vyos# set interfaces ethernet eth1 address ZZZ.ZZZ.ZZZ.ZZZ/24
vyos@vyos# set interfaces ethernet eth1 description Server
```

# FWの設定
とりあえずガバガバにしとく

## FWのポリシーを作る
```
set firewall name public-private default-action accept
set firewall name private-public default-action accept
```

## FWのポリシーをインターフェースに割り当てる
```
set interfaces ethrnet eth0 firewall int name public-private
set interfaces ethrnet eth1 firewall int name private-public
```