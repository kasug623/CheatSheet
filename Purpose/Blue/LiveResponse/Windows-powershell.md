#team/blue #os/windows

# TOC<!-- omit in toc -->
- [Network](#network)

# Network
## displays detailed information about current network connections
```powershell
netstat -ano
netstat -ano | sls <PID>
```
**What is `netstat`?**  
`netstat` is a command available on both Windows and Linux that displays the current network status.
It allows you to check open ports, protocols in use, connected IP addresses, and more.  

`-a` (all)  
Displays all active connections as well as listening ports (ports waiting for incoming connections).  

`-n` (numeric)  
Shows IP addresses and port numbers in numeric form, without resolving hostnames.  

Example: Instead of displaying www.example.com:80, it will show 93.184.216.34:80.  

`-o` (owning process ID)  
Displays the Process ID (PID) associated with each connection.
This is useful for identifying which application is responsible for each network connection.