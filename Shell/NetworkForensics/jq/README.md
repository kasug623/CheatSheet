```
$ cat foo.json | jq .
$ cat foo.json | jq -r '.xxx'
$ grep XXX\.exe foo.log | jq . '{ XXX_XXX, YYY_YYY, ZZZ_ZZZ }'
$ grep xxx foo.log | jq -cr '{ "xxx.yyy", "xxx.zzz" }' | sort | uniq -c | sort -nr
$ for i in $(cat foo.txt); do grep $i foo.json | jq -cr '{ xxx, yyy }'; done
$ cat foo.log | jq -cr '. | select(.xxx == "aaa" or .xxx == "bbb") | { xxx, "yyy.y1", "yyy.y2" }' | sort | uniq -c | sort -nr
$ zcat ./foo.gz | head -n 1 | jq -cr '.ts | todate'
$ zcat ./foo.gz | jq -r 'select(.rcode_name != "XXX") | .yyy' | sort | uniq -c | sort -nr
$ zcat ./foo.gz | jq -r 'select(.rcode_name != "XXX") | {yyy, zzz}' | sort | uniq -c | sort -nr
$ zcat ./foo.gz | jq -cr 'select((."XXX.YYY" == "AAA" or ."XXX.ZZZ" == "BBB") and .WWW == "CCC") | { DDD: .DDD | todate, "PPP.QQQ", RRR}'
```

