```
$ cat foo.json | jq .
$ cat foo.json | jq -r '.xxx'
$ grep xxx foo.log | jq -cr '{ "xxx.yyy", "xxx.zzz" }' | sort | uniq -c | sort -nr
$ for i in $(cat foo.txt); do grep $i foo.json | jq -cr '{ xxx, yyy }'; done
$ cat foo.log | jq -cr '. | select(.xxx == "aaa" or .xxx == "bbb") | { xxx, "yyy.y1", "yyy.y2" }' | sort | uniq -c | sort -nr
```