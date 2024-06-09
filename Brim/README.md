# Linux Version
## Location of Suricata Rules
When you want to update suricata rules, you can simply replace the file under this.
```
/opt/Brim/resources/app.asar.unpacked/zdeps/suricata/var/lib/suricata/rules
```
# Windows Version
## Location of Suricata Rules
When you want to update suricata rules, you can simply replace the file under this.
```
C:\Users\user\AppData\Local\Programs\brim\resources\app.asar.unpacked\zdeps\suricata\var\lib\suricata\rules
```

# query
`zq` as script language is used.  
This is similar to `jq` one.  
[https://zed.brimdata.io/docs/commands/zq](https://zed.brimdata.io/docs/commands/zq)  
[https://github.com/brimdata/zed/blob/v1.1.0/docs/language/overview.md](https://github.com/brimdata/zed/blob/v1.1.0/docs/language/overview.md)  
```
count() by id
```
```
_path=http | count() by host| sort -r count
```
```
_path="ssl" |count() by issuer | sort -r count
```
