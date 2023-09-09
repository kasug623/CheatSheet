# TOC

## Port Forwarding
run with admin.  
```
> netsh interface portproxy show all
> netsh interface portproxy delete v4tov4 listenaddress="XXX.XXX.XXX.XXX" listenport="443"
```
rename a filename of VMware's `.vmdk`.  
```
for($i=1; $i -lt 17; $i++){
	$j = $i.ToString("00")
	$filename = "test_Windows Server 2019-s0" + $j + ".vmdk"
	$newname = "PJ_kerberos_OS_windows_server_2019-s0" + $j + ".vmdk"
	Rename-Item -Path $filename -NewName $newname
}
```

## FIle Download
use `Invoke-WebRequest` and `-OutFile` option is a must.  
Only `Invoke-WebRequest` is pretty slow because of displaying a progress on a console. 
`ProgressPreference` is my taste.
```
> ProgressPreference = 'SilentlyContinue'
> Invoke-WebRequest -Uri hxxps://hogehoge/foo.zip -OutFile ./foo.zip
> ProgressPreference = 'Continue'
```
or
```
> Invoke-WebRequest -UseBasicParsing -Uri hxxps://hogehoge/foo.zip -OutFile ./foo.zip
```

# Forensics Technique
## autorunsc
```
$strings = @("auto_update.bat", "perfmonsvc64.exe", "perfsvc.exe", "systemsettings.dll")

foreach($i in $strings)
{
	Select-Strings $i *Autorunsc.csv
	Write-Output ""
}
```

##  複数の.csvに対してgrepした時の見やすい出力
- normal grep
```
Select-String "SystemPerformanceMonitor" base*.csv
```
result
```
(weird empty line)
a,b,b,s,d
A,B,C,D,
(weird empty line)
```
大量もデータがあるときに見にくい。折り返しが発生すると最悪。値の名前が長い時も最悪。
しかも冒頭や末尾に改行が入る。
- output technique
```
$a = Select-String "SystemPerformanceMonitor" base*.csv; $a -replace ",", ",`n"
```
result
```
a,
b,
c,
d,
....
```
ちょっとだけましになるが、これはこれで見にくい。最初に普通に出力して、見たい部分が決まったら、このパターンで出力すると良いかも。全体像が見えにくくなるのが難点。色々試したが、簡単かつ見やすいだと、この方法が一番楽そう。Powershellいまいち。

# Malware Analysis
## Attacker
```
> Add-MpPreference -ExclusionPath C:¥User¥user¥AppData¥Roaming¥Evil.exe
```
Excluding Defender's detection 
[https://learn.microsoft.com/en-us/powershell/module/defender/add-mppreference?view=windowsserver2022-ps](https://learn.microsoft.com/en-us/powershell/module/defender/add-mppreference?view=windowsserver2022-ps)
