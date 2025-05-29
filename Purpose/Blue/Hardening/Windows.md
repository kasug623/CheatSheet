#team/blue #os/windows

# Vs. LLMNR/NBT-NS Poisoning and SMB Relay
##  disable LLMNR  
GPO > Computer Configuration > Policies > Administrative Templates > Network > DNS Client > enable "Turn OFF Multicast Name Resolution."

## disable NBT-NS
NBT-NS cannot be disabled via GPO.  
Disabled locally on each host.  
... Ethernet option ... > Internet Protocol Version 4 (TCP/IPv4) > Properties > Advanced > "WINS" tab > Select "Disable NetBIOS over TCP/IP"
- powrehsell automation  
Startup folder  
Local Group Policy > Computer Configuration > Windows Settings > Script (Startup/Shutdown) > Startup  
```powershell
$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces"
Get-ChildItem $regkey |foreach { Set-ItemProperty -Path "$regkey\$($_.pschildname)" -Name NetbiosOptions -Value 2 -Verbose}
```
Share script from AD to domain joined computers
```
\\test.local\SYSVOL\TEST.LOCAL\scripts
```