# `DocRecrypt`
- a Microsoft tool designed to recover access to password-protected Office documents (Word, Excel, PowerPoint)
- https://www.microsoft.com/en-us/download/details.aspx?id=36443
- Default installed path
```powershell
C:\Program Files (x86)\Microsoft Office\DOCRECRYPT\DOCRECRYPT.EXE
```
## Command
```powershell
PS> DOCRECRYPT.EXE -p TestPass -i UnknownPasswordProtected.pptx
```