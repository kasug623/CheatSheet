#os/linux

# TOC <!-- omit in toc -->
- [Install](#install)
- [Using VyOS as a Router on ESXi](#using-vyos-as-a-router-on-esxi)
	- [1. Set up two network adapters on ESXi.](#1-set-up-two-network-adapters-on-esxi)
	- [2. Configuration on VyOS](#2-configuration-on-vyos)
	- [3. NAT](#3-nat)
	- [4. FW](#4-fw)
		- [Create Firewall Policies](#create-firewall-policies)
		- [Assign Firewall Policies to Interfaces](#assign-firewall-policies-to-interfaces)

# Install
```
$ install image
	Enter Enter ...
$ reboot
	Yes
```
# Using VyOS as a Router on ESXi
## 1. Set up two network adapters on ESXi.  
## 2. Configuration on VyOS
```
install image 
```
The square brackets - [] in the questions indicate the default answer,  
so pressing Enter will select the default value.
Basically, just keep pressing Enter.  
On the third question, type "Yes" instead of "No", and then you can continue hitting Enter to proceed.  
```zsh
show config
```
Use the following to verify if two interfaces are present and ready for NAT configuration:  
```zsh
vyos@vyos:~$ configure
vyos@vyos# commit
vyos@vyos# save
vyos@vyos# exit
```
```zsh
set protocols static route 0.0.0.0/0 next-hop XXX.XXX.XXX.XXX
```
```zsh
vyos@vyos# set interfaces ethernet eth0 address YYY.YYY.YYY.YYY/24
vyos@vyos# set interfaces ethernet eth0 description Client
vyos@vyos# set interfaces ethernet eth1 address ZZZ.ZZZ.ZZZ.ZZZ/24
vyos@vyos# set interfaces ethernet eth1 description Server
```

## 3. NAT
```zsh
# https://docs.vyos.io/en/latest/configuration/nat/nat44.html
vyos@vyos# set nat source rule 1 outbound-interface name 'eth0'
vyos@vyos# set nat source rule 1 source address '172.17.0.0/24'
vyos@vyos# set nat source rule 1 translation address 'masquerade'
```

## 4. FW
For now, we'll keep it wide open.
### Create Firewall Policies
This didn't work at Version 15.
```zsh
set firewall name public-private default-action accept
set firewall name private-public default-action accept
```
### Assign Firewall Policies to Interfaces
```zsh
set interfaces ethernet eth0 firewall int name public-private
set interfaces ethernet eth1 firewall int name private-public
```