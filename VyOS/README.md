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
質問についてる[]はでデフォルトの答えなので、
エンターキーを押せば勝手にその値が入る。  
基本的にエンターだけでよく、最初から3番目の質問の時にNoではなくYesと入力すれば、
あとは連打でOK。

```
show config
```
でインターフェースが二つ出ていればNAT設定ができる。

```
vyos@vyos:~$ configure
vyos@vyos# commit
vyos@vyos# save
vyos@vyos# exit
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
# NAT
```zsh
# https://docs.vyos.io/en/latest/configuration/nat/nat44.html
vyos@vyos# set nat source rule 1 outbound-interface name 'eth0'
vyos@vyos# set nat source rule 1 source address '172.17.0.0/24'
vyos@vyos# set nat source rule 1 translation address 'masquerade'
```

# FW
とりあえずガバガバにしとく。

## FWのポリシーを作る
This didn't work at Version 15.
```
set firewall name public-private default-action accept
set firewall name private-public default-action accept
```

## FWのポリシーをインターフェースに割り当てる
```
set interfaces ethernet eth0 firewall int name public-private
set interfaces ethernet eth1 firewall int name private-public
```