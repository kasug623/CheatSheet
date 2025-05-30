#os/linux

# TOC <!-- omit in toc -->
- [Network](#network)

# Network
- kali-linux-2024.2-vmware-amd64
    ```zsh
    $ sudo vim /etc/network/interfaces
    ```
    ```yaml
    auto eth0
    iface eth0 inet static
        address 192.168.1.100
        netmask 255.255.255.0
        gateway 192.168.1.1
        dns-nameservers 8.8.8.8 8.8.4.4
    ```
    ```zsh
    $ vim /etc/resolv.conf
    ```
    ```yaml
    nameserver 8.8.8.8
    nameserver 8.8.4.4
    ```
    ```zsh
    $ sudo systemctl restart networking
    ```