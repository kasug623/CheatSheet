# Linux バージョン
## Suricata の rule の場所
suricata.rulesを置き換える際は、以下のパスの中のファイルを入れ替えれば良い。
```
/opt/Brim/resources/app.asar.unpacked/zdeps/suricata/var/lib/suricata/rules
```
# Windows バージョン
## Suricata の rule の場所
suricata.rulesを置き換える際は、以下のパスの中のファイルを入れ替えれば良い。
```
C:\Users\SANSDFIR\AppData\Local\Programs\brim\resources\app.asar.unpacked\zdeps\suricata\var\lib\suricata\rules
```

# query
zqというスクリプト言語。jqの親戚みたいなものっぽい。
[https://zed.brimdata.io/docs/commands/zq](https://zed.brimdata.io/docs/commands/zq)  
[https://github.com/brimdata/zed/blob/v1.1.0/docs/language/overview.md](https://github.com/brimdata/zed/blob/v1.1.0/docs/language/overview.md)  

## グルーピング
```
count() by id
```
```
_path=http | count() by host| sort -r count
```
```
_path="ssl" |count() by issuer | sort -r count
```