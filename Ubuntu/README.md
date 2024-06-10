# Ubuntu Server
## Ubuntu Server 22.04.4 LTS
- Network
    - netplan
        ```zsh
        $ sudo vi /etc/netplan/99_config.yaml
        $ sudo chmod 600 /etc/netplan/99_config.yaml
        $ sudo netplan apply
        ```
        - /etc/netplan/99_config.yaml
        ```yaml
        network:
            ethernets:
                ens18:
                    dhcp4: false
                    dhcp6: false
                    addresses: [192.168.0.XXX/24]
                    gateway4: 192.168.0.XXX
                    nameservers:
                        addresses: [192.168.0.XXX, 8.8.8.8, 8.8.4.4]
        ```

# nmcli で設定する
IP アドレス設定方式を静的（手動）にした上で IP アドレスを設定する場合
コネクション名は大体 'Wired Connection 1' 。 
補完が効くはず。  
```
nmcli c modify コネクション名 ipv4.method manual ipv4.addresses x.x.x.x/prefix
```
IP アドレス設定のみ行う場合  
```
nmcli c modify コネクション名 ipv4.addresses x.x.x.x/prefix
```
DHCP を利用するよう設定する場合  
```
nmcli c modify コネクション名 ipv4.method auto
```

# ssh設定
## ssh-keygen
```
ssh-keygen -t rsa -b 4096 -c "#8"
```

## ssh-copy-id
```
ssh-copy-id root@XXX.XXX.XXX.XXX
```

# Desktop
## 解像度の変更
to make the GUI scale lager than normal
```
set-display-scale 0.75
```
to make the GUI scale smaller than normal
```
set-display-scale 1.5
```
to make the GUI scale to noramal
```
set-display-scale 1.0
```

# Server
## 解像度の変更
難しい。  
sshで別のターミナル上で調整するのが良い。  
一応grubで変更はできるが、OSがサポートしてる改造までしか上げられず、最近の大型ディスプレイには適さないことがある。  

## キーボードの言語設定
localctlでにsetxkbmapでも変更できなかった。  
そもそもOSの中にJapの設定ファイルがないだけかもしれないが。  
諦めてsshして別ターミナルで入る方が良さそう。  

# VMware(ホスト型)の仮想マシンでDHCPのIPアドレスが割り当てられないとき
```
sudo dhclient
```