# TOC
- [the way to use "%"](#foreach-object-and)

# Setting
```powershell
# network
PS> New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 172.16.0.5 -PrefixLength 24 -DefaultGateway 172.16.0.254
PS> Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses ("8.8.8.8","8.8.4.4")
# check
PS> Get-NetAdapter
PS> Get-NetIPAddress
PS> Get-NetRoute
# remove
PS> Remove-NetIPAddress -InterfaceAlias "Ethernet"
PS> Remove-NetRoute -InterfaceAlias "Ethernet"
```

# create Domain Env
```powershell
PS> $userOU = "OU=Users,DC=hoge,DC=net"

# Create the new user
PS> New-ADUser `
    -GivenName $userFirstName `
    -Surname $userLastName `
    -Name "$userFirstName $userLastName" `
    -SamAccountName $userSamAccountName `
    -UserPrincipalName "$userSamAccountName@hoge.net" `
    -Path $userOU `
    -AccountPassword $userPassword `
    -Enabled $true `
    -PasswordNeverExpires $false `
    -ChangePasswordAtLogon $true

# Optionally, add the user to groups
PS> Add-ADGroupMember -Identity "Domain Users" -Members $userSamAccountName

# Client
PS> $DomainController = "XXX.XXX.XXX.XXX"
PS> Test-Connection -ComputerName $DomainController -Count 4

PS> Get-NetAdapter
$NetworkAdapter = Get-NetAdapter | Where-Object {$_.Status -eq "Up"}

## DNS
PS> Get-NetAdapter | Select-Object {$_.InterfaceAlias}
PS> Set-DnsClientServerAddress -ServerAddresses $DomainController -InterfaceAlias "Ethernet"

## join computer to domain
PS> $user='Administrator'
PS> $pw='C~'
PS> $sec=ConvertTo-SecureString $pw -AsPlainText -Force
PS> $cred=New-Object System.Management.Automation.PSCredential $user, $sec
PS> Add-Computer -Credential $cred -OUPath 'OU="IT Management",DC=hoge,DC=net'
> DomainName: hoge.net
PS> Restart-Computer
```

# Port Forwarding
run with admin.  
```powershell
PS > netsh interface portproxy show all
PS > netsh interface portproxy delete v4tov4 listenaddress="XXX.XXX.XXX.XXX" listenport="443"
```

# Rename a filename of VMware's `.vmdk`.  
```powershell
for($i=1; $i -lt 17; $i++){
	$j = $i.ToString("00")
	$filename = "test_Windows Server 2019-s0" + $j + ".vmdk"
	$newname = "PJ_kerberos_OS_windows_server_2019-s0" + $j + ".vmdk"
	Rename-Item -Path $filename -NewName $newname
}
```

# File Download
use `Invoke-WebRequest` and `-OutFile` option is a must.  
Only `Invoke-WebRequest` is pretty slow because of displaying a progress on a console.  
`ProgressPreference` is my taste.
```powershell
PS > ProgressPreference = 'SilentlyContinue'
PS > Invoke-WebRequest -Uri hxxps://hogehoge/foo.zip -OutFile ./foo.zip
PS > ProgressPreference = 'Continue'
```
or
```powershell
PS > Invoke-WebRequest -UseBasicParsing -Uri hxxps://hogehoge/foo.zip -OutFile ./foo.zip
```

# Forensics Technique
## autorunsc
```powershell
$strings = @("auto_update.bat", "perfmonsvc64.exe", "perfsvc.exe", "systemsettings.dll")

foreach($i in $strings)
{
	Select-Strings $i *Autorunsc.csv
	Write-Output ""
}
```

# pretty print for multiple csv files with grep
- normal grep  
```powershell
PS > Select-String "SystemPerformanceMonitor" base*.csv
(weird empty line)
a,b,b,s,d
A,B,C,D,
(weird empty line)
```
When there is a large amount of data, it becomes hard to read.  
It's worst when line breaks occur.  
It's also worst when the names of values are long.  
Moreover, line breaks are inserted at the beginning or end.  
- output technique  
```powershell
PS > $a = Select-String "SystemPerformanceMonitor" base*.csv; $a -replace ",", ",`n"

a,
b,
c,
d,
....
```
It becomes a little better, but this is still hard to read.  
It might be good to output normally at first, and then output in this pattern when the specific part you want to see is determined.  
The drawback is that it's difficult to see the overall picture.  
I tried various things, but if simplicity and readability are important, this method seems to be the easiest.  
Not so great with PowerShell.  

# ForEach-Object and %
`0..3` is like range() of python.  
`%` is the same as `ForEach-Object` function. 
```powershell
PS > 0..3|ForEach-Object{echo 5}
5
5
5
5
PS > 0..3|%{echo 5}
5
5
5
5
```

# Malware Analysis
## For Attacker
```powershell
PS > Add-MpPreference -ExclusionPath C:¥User¥user¥AppData¥Roaming¥Evil.exe
```
Excluding Defender's detection  
[https://learn.microsoft.com/en-us/powershell/module/defender/add-mppreference?view=windowsserver2022-ps](https://learn.microsoft.com/en-us/powershell/module/defender/add-mppreference?view=windowsserver2022-ps)