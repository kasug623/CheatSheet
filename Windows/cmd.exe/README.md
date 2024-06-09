# several version of cmd.exe
64bit-cmd.exe
```
C:¥Windows¥System32¥cmd.exe
```
32bit-cmd.exe
```
C:¥Windows¥Syswow64¥cmd.exe
```
call 64bit-cmd.exe from 32bit-cmd.exe
```
C:¥Windows¥sysnative¥cmd.exe
```

# folder manupulation
## cd ~/
```console
$ cd %userprofile%
```
## display ADS of file
add /r option
```console
$ dir /r
```

# process check
## wmi
use grep and filter display
```console
$ wmic process where name="cmd.exe" get ProcessID, ExecutablePath
```

# Forensics Technique
## Zimmerman Tool
### EtxECmd.exe
```console
$ EvtxECmd.exe -f Security.evtx --csv C:\User\user\Desktop --csvf security.csv
```

# Malware Analysys
## Attacker
```console
$ schtasks /create /TN "MyTask" /XML "TaskContentXML"
```
`/Create` is the same as `/create`.  
`/TN` is the same as `/tn`.  
`/XML` is the same as `/xml`.  