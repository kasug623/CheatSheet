#os/linux

# TOC <!-- omit in toc -->
- [Network](#network)
  - [Ubuntu Server 20.04 / 22.04.4 LTS](#ubuntu-server-2004--22044-lts)
- ["ssh" configuration](#ssh-configuration)
  - [ssh-keygen](#ssh-keygen)
  - [ssh-copy-id](#ssh-copy-id)
- [Desktop](#desktop)
  - [Change of resolution](#change-of-resolution)
- [Server](#server)
  - [Change of resolution](#change-of-resolution-1)
  - [Keyboard language settings](#keyboard-language-settings)
- [When a DHCP IP address is not assigned in a VMware (host-based) virtual machine.](#when-a-dhcp-ip-address-is-not-assigned-in-a-vmware-host-based-virtual-machine)

# Network
## Ubuntu Server 20.04 / 22.04.4 LTS
- `netplan`
    ```zsh
    $ sudo vi /etc/netplan/99_config.yaml
    ```
    deprecated version
    ```yaml
    network:
        ethernets:
            ens18:
                dhcp4: false
                dhcp6: false
                addresses: [192.168.0.XXX/24]
                gateway4: 192.168.0.XXX
                nameservers:
                    addresses:
                        - 8.8.8.8
                        - 8.8.4.4
    ```
    new version
    ```yaml
    network:
    ethernets:
        ens18:
            dhcp4: false
            dhcp6: false
            addresses: [192.168.0.XXX/24]
            routes:
                - to: default
                  via: 192.168.0.1
            nameservers:
                addresses:
                    - 8.8.8.8
                    - 8.8.4.4
    ```
    ```zsh
    $ sudo chmod 600 /etc/netplan/99_config.yaml
    $ sudo netplan apply
    $ ip a
    ```
- `nmcli`  
    When setting the IP address with the configuration method set to static (manual),
    the connection name is usually 'Wired Connection 1'.  
    Autocomplete should work.
    ```
    $ nmcli c modify <connection name> ipv4.method manual ipv4.addresses x.x.x.x/prefix
    ```
    When setting only the IP address:  
    ```
    $ nmcli c modify <connection name> ipv4.addresses x.x.x.x/prefix
    ```
    When configuring to use DHCP:  
    ```
    $ nmcli c modify <connection name> ipv4.method auto
    ```

# "ssh" configuration
## ssh-keygen
```
$ ssh-keygen -t rsa -b 4096 -c "#8"
```

## ssh-copy-id
```
$ ssh-copy-id root@XXX.XXX.XXX.XXX
```

# Desktop
## Change of resolution
to make the GUI scale lager than normal
```
set-display-scale 0.75
```
to make the GUI scale smaller than normal
```
$ set-display-scale 1.5
```
to make the GUI scale to noramal
```
$ set-display-scale 1.0
```

# Server
## Change of resolution
It’s difficult.  
Adjusting on another terminal via SSH is preferable.  
You can change it with GRUB as a fallback, but it only supports modifications allowed by the OS, so it may not suit modern large displays.  

## Keyboard language settings
I couldn't change it using `localectl` or `setxkbmap`.  
It might be because the OS simply doesn't have the Japanese configuration files.  
It seems better to give up and access via SSH from another terminal.

# When a DHCP IP address is not assigned in a VMware (host-based) virtual machine.
```
$ sudo dhclient
```