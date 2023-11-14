# File Ssharing between Windows and Mac on the same network 
[https://usedoor.jp/howto/digital/pc/windows10-mac-file-folder-kyouyuu/](https://usedoor.jp/howto/digital/pc/windows10-mac-file-folder-kyouyuu/)

# cmd.exe
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

# Hidden Path
- Progra~1
```
cd C:¥Program Files
cd C:¥Progra~1
```
`Program~1` is a shortened, 8.3-style representation of the "Program Files" folder in Windows operating systems. In the era before long file names (LFN) were widely supported, particularly during the time of Windows 95 and Windows 98, file and folder names adhered to the 8.3 naming convention, which allowed for eight characters for the name and three characters for the extension.

In the case of the "Program Files" folder, if the original name exceeded the 8.3 format, it would be truncated, and Progra~1 would be used as a shorter representation. It's essentially a relic from the days when Windows used the 8.3 file naming convention. Nowadays, with the widespread support for long file names, this 8.3-style representation is mostly a compatibility feature and is not commonly used in modern Windows systems. The full and proper name of the folder is still "Program Files."

# Windos PATH (NTFS) is Case-Insensitive
These are the same.  
```
C:¥Windows¥System32¥cmd.exe
C:¥Windows¥system32¥cmd.exe
C:¥Windows¥sysTem32¥cmd.exe
C:¥Windows¥SYSTEM32¥cmd.exe
C:¥Windows¥sYSTEM32¥cmd.exe
```
These are the same.  
```
C:¥Windows¥Syswow64¥cmd.exe
C:¥Windows¥SYSWOW64¥cmd.exe
C:¥Windows¥sysWow64¥cmd.exe
C:¥Windows¥syswoW64¥cmd.exe
C:¥Windows¥sYSWOW64¥cmd.exe
```