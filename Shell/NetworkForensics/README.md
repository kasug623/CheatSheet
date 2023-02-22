# TOC


# Intelligence
## PacketTotal
https://packettotal.com/

# domain -> ip
```
$ host google.com
google.com has address 172.217.174.110
google.com has IPv6 address 2404:6800:4004:810::200e
google.com mail is handled by 10 smtp.google.com.
```

# pcap
## pcap info -> capinfos コマンド
pcapの情報を全表示
```
$ capinfos httpload.pcap
File name:           httpload.pcap
File type:           Wireshark/tcpdump/... - pcap
File encapsulation:  Ethernet
File timestamp precision:  microseconds (6)
Packet size limit:   file hdr: 262144 bytes
Number of packets:   2,162
File size:           8,261 kB
Data size:           8,226 kB
Capture duration:    56.321246 seconds
First packet time:   2022-10-09 09:17:08.875479
Last packet time:    2022-10-09 09:18:05.196725
Data byte rate:      146 kBps
Data bit rate:       1,168 kbps
Average packet size: 3805.23 bytes
Average packet rate: 38 packets/s
SHA256:              16c46c8812eadad82f207d64d2b479a3d711124ee1be994bc75d52531a5dfd6a
RIPEMD160:           19284a196b9544ade2a792405d113f5d489010a8
SHA1:                90ea3944a20777b2642dc3548c961d141c44c32a
Strict time order:   True
Number of interfaces in file: 1
Interface #0 info:
                     Encapsulation = Ethernet (1 - ether)
                     Capture length = 262144
                     Time precision = microseconds (6)
                     Time ticks per second = 1000000
                     Number of stat entries = 0
                     Number of packets = 2162
```
オプションで表示項目を絞る。表示項目に対応するオプションはmanで確認、
```
$ capinfos -Hae httpload.pcap
File name:           httpload.pcap
First packet time:   2022-10-09 09:17:08.875479
Last packet time:    2022-10-09 09:18:05.196725
SHA256:              16c46c8812eadad82f207d64d2b479a3d711124ee1be994bc75d52531a5dfd6a
RIPEMD160:           19284a196b9544ade2a792405d113f5d489010a8
SHA1:                90ea3944a20777b2642dc3548c961d141c44c32a
```
別パターン1
```
$ capinfos -Mcsd httpload-non572.pcap 
File name:           httpload-non572.pcap
Number of packets:   416
File size:           3218382 bytes
Data size:           3211702 bytes
```
別パターン2
```
$ capinfos -McsdaeH httpload-non572.pcap 
File name:           httpload-non572.pcap
Number of packets:   416
File size:           3218382 bytes
Data size:           3211702 bytes
First packet time:   2022-10-09 09:17:24.402335
Last packet time:    2022-10-09 09:17:35.795479
SHA256:              682ba25f279da9b51f9aaf02c0ad5744dd895c482027ffb4b52c808e9eb0cfaf
RIPEMD160:           2d8694a833623c92cf84fa3f20d4f89a5b61eefa
SHA1:                5fca075f801755d0e31c3ba07dc6046d65139d30
```

## reduce pcap -> editcap コマンド
日付で削減する。
```
$ capinfos -Hae httpload.pcap
File name:           httpload.pcap
First packet time:   2022-10-09 09:17:08.875479
Last packet time:    2022-10-09 09:18:05.196725
SHA256:              16c46c8812eadad82f207d64d2b479a3d711124ee1be994bc75d52531a5dfd6a
RIPEMD160:           19284a196b9544ade2a792405d113f5d489010a8
SHA1:                90ea3944a20777b2642dc3548c961d141c44c32a
$
$ editcap -B '2022-09-17 09:17:45' httpload.pcap
httpload-firstseconds.pcap
$
$ capinfos -Hae httpload-firstseconds.pcap 
File name:           httpload-firstseconds.pcap
First packet time:   2022-10-09 09:17:08.875479
Last packet time:    2022-10-09 09:17:44.854213
SHA256:              c46009f98645e3281c49796d2eae0adb18b03d6fe7a009bb2ad75510c5a647fd
RIPEMD160:           1de7d5a50411e352510f6a517809c4eb907f57a6
SHA1:                25bdf1e78806c190e0638c808db8c7495d2bd5e5
```

## reduce pcap -> tcpdump コマンド
```
$ tcpdump -n -r httpload.pcap -w httpload-non572.pcap 'tcp and port 80 and not host 70.32.97.206'
reading from file httpload.pcap, link-type EN10MB (Ethernet)
```

## filter pcap -> tshark コマンド
```
$ tshark -n -r httpload-non572.pcap -Y 'http.server' -T fields -e http.server | sort | uniq -c | sort -nr
...
$ tshark -n -r nitroba.pcap -Y 'http.server contains "mod_ssl"' -T fields -e ip.src -e http.server | sort | uniq -c | sort -r
...
```

## sort -n　
-nはsortする際の、先頭列の文字を数字”として扱うように指示する。
（通常は、”文字”として扱うため、uniq -cとの相性がイマイチ）
```
$ tshark -n -r nitroba.pcap -Y 'http.server contains "mod_ssl"' -T fields -e ip.src -e http.server | sort | uniq -c | sort -r
      6 66.150.96.119	Apache/2.0.54 (Debian GNU/Linux) mod_fastcgi/2.4.2 mod_ssl/2.0.54 OpenSSL/0.9.7e
      5 67.15.76.53	Apache/1.3.37 (Unix) mod_ssl/2.8.28 OpenSSL/0.9.7a PHP/5.2.5
     56 69.22.167.239	Apache/1.3.41 (Unix) mod_ssl/2.8.31 OpenSSL/0.9.8a
      5 66.33.212.43	Apache/2.0.61 (Unix) PHP/4.4.7 mod_ssl/2.0.61 OpenSSL/0.9.7e mod_fastcgi/2.4.2 DAV/2
      5 63.247.140.161	Apache/2.0.63 (Unix) mod_ssl/2.0.63 OpenSSL/0.9.7a mod_auth_passthrough/2.1 mod_bwlimited/1.4 FrontPage/5.0.2.2635 PHP/5.2.6
      5 206.55.125.137	Apache/2.0.54 (Unix) mod_ssl/2.0.54 OpenSSL/0.9.7a
      2 66.98.172.25	Apache/1.3.37 (Unix) mod_ssl/2.8.28 OpenSSL/0.9.7a PHP/5.2.5
      2 64.128.80.61	Apache/1.3.41 (Unix) PHP/5.2.6 mod_auth_passthrough/1.8 mod_log_bytes/1.2 mod_bwlimited/1.4 FrontPage/5.0.2.2635 mod_ssl/2.8.31 OpenSSL/0.9.8b
     24 18.7.22.69	MIT Web Server Apache/1.3.26 Mark/1.5 (Unix) mod_ssl/2.8.9 OpenSSL/0.9.7c
      2 18.7.7.97	Apache/1.3.26 (Unix) PHP/4.3.4 mod_fastcgi/mod_fastcgi-SNAP-0203131834 mod_ssl/2.8.9 OpenSSL/0.9.7c
     21 64.94.186.12	Apache/1.3.41 (Unix) mod_ssl/2.8.31 OpenSSL/0.9.8b
      1 85.10.206.119	Apache/2.2.4 (Unix) DAV/2 mod_ssl/2.2.4 OpenSSL/0.9.8d PHP/5.2.1 mod_apreq2-20051231/2.5.7 mod_perl/2.0.2 Perl/v5.8.7
      1 74.54.77.137	Apache/1.3.39 (Unix) mod_auth_passthrough/1.8 mod_log_bytes/1.2 mod_bwlimited/1.4 FrontPage/5.0.2.2635.SR1.2 mod_ssl/2.8.30 OpenSSL/0.9.7a PHP-CGI/0.1b
      1 69.22.167.232	Apache-AdvancedExtranetServer/1.3.27 (Mandrake Linux/8mdk) mod_ssl/2.8.12 OpenSSL/0.9.7
      1 67.15.56.64	Apache/1.3.37 (Unix) mod_ssl/2.8.28 OpenSSL/0.9.7a PHP/5.2.5
     16 209.123.229.231	Apache/1.3.37 (Unix) mod_gzip/1.3.26.1a mod_fastcgi/2.4.2 mod_auth_passthrough/1.8 mod_log_bytes/1.2 mod_bwlimited/1.4 FrontPage/5.0.2.2635.SR1.2 mod_ssl/2.8.28 OpenSSL/0.9.7a PHP-CGI/0.1b
      1 216.64.142.80	Apache/2.0.61 (Unix) mod_ssl/2.0.61 OpenSSL/0.9.8b mod_bwlimited/1.4 mod_auth_passthrough/2.1 FrontPage/5.0.2.2635 PHP/5.2.5
$
$ tshark -n -r nitroba.pcap -Y 'http.server contains "mod_ssl"' -T fields -e ip.src -e http.server | sort | uniq -c | sort -nr
     56 69.22.167.239	Apache/1.3.41 (Unix) mod_ssl/2.8.31 OpenSSL/0.9.8a
     24 18.7.22.69	MIT Web Server Apache/1.3.26 Mark/1.5 (Unix) mod_ssl/2.8.9 OpenSSL/0.9.7c
     21 64.94.186.12	Apache/1.3.41 (Unix) mod_ssl/2.8.31 OpenSSL/0.9.8b
     16 209.123.229.231	Apache/1.3.37 (Unix) mod_gzip/1.3.26.1a mod_fastcgi/2.4.2 mod_auth_passthrough/1.8 mod_log_bytes/1.2 mod_bwlimited/1.4 FrontPage/5.0.2.2635.SR1.2 mod_ssl/2.8.28 OpenSSL/0.9.7a PHP-CGI/0.1b
      6 66.150.96.119	Apache/2.0.54 (Debian GNU/Linux) mod_fastcgi/2.4.2 mod_ssl/2.0.54 OpenSSL/0.9.7e
      5 67.15.76.53	Apache/1.3.37 (Unix) mod_ssl/2.8.28 OpenSSL/0.9.7a PHP/5.2.5
      5 66.33.212.43	Apache/2.0.61 (Unix) PHP/4.4.7 mod_ssl/2.0.61 OpenSSL/0.9.7e mod_fastcgi/2.4.2 DAV/2
      5 63.247.140.161	Apache/2.0.63 (Unix) mod_ssl/2.0.63 OpenSSL/0.9.7a mod_auth_passthrough/2.1 mod_bwlimited/1.4 FrontPage/5.0.2.2635 PHP/5.2.6
      5 206.55.125.137	Apache/2.0.54 (Unix) mod_ssl/2.0.54 OpenSSL/0.9.7a
      2 66.98.172.25	Apache/1.3.37 (Unix) mod_ssl/2.8.28 OpenSSL/0.9.7a PHP/5.2.5
      2 64.128.80.61	Apache/1.3.41 (Unix) PHP/5.2.6 mod_auth_passthrough/1.8 mod_log_bytes/1.2 mod_bwlimited/1.4 FrontPage/5.0.2.2635 mod_ssl/2.8.31 OpenSSL/0.9.8b
      2 18.7.7.97	Apache/1.3.26 (Unix) PHP/4.3.4 mod_fastcgi/mod_fastcgi-SNAP-0203131834 mod_ssl/2.8.9 OpenSSL/0.9.7c
      1 85.10.206.119	Apache/2.2.4 (Unix) DAV/2 mod_ssl/2.2.4 OpenSSL/0.9.8d PHP/5.2.1 mod_apreq2-20051231/2.5.7 mod_perl/2.0.2 Perl/v5.8.7
      1 74.54.77.137	Apache/1.3.39 (Unix) mod_auth_passthrough/1.8 mod_log_bytes/1.2 mod_bwlimited/1.4 FrontPage/5.0.2.2635.SR1.2 mod_ssl/2.8.30 OpenSSL/0.9.7a PHP-CGI/0.1b
      1 69.22.167.232	Apache-AdvancedExtranetServer/1.3.27 (Mandrake Linux/8mdk) mod_ssl/2.8.12 OpenSSL/0.9.7
      1 67.15.56.64	Apache/1.3.37 (Unix) mod_ssl/2.8.28 OpenSSL/0.9.7a PHP/5.2.5
      1 216.64.142.80	Apache/2.0.61 (Unix) mod_ssl/2.0.61 OpenSSL/0.9.8b mod_bwlimited/1.4 mod_auth_passthrough/2.1 FrontPage/5.0.2.2635 PHP/5.2.5
```

# TIPS
## ファイルの中身数文字を表示 > cut コマンド
特に１行に大量の文字がある場合に有効
```
$ cat base64_extraction.txt | cut -c 1-20
UEsDBBQABgAIAAAAIQCe
```

## X列目の値でソートしたいとき > sortコマンド
-kで列を指定する。awkを使わなくてもできる。
- -k : X列目
- -t : 区切り文字
- -n : 値全体を数字として扱う
```
$ tshark -r mta3.pcap -Y dns -T fields -e frame.number -e frame.time -e dns.resp.ttl | sort -n -r -k 7 | head -n 50
3894	Dec  4, 2014 18:27:50.889038000 UTC	3599,299,299,299,299,299,299,299,299,299,299,299
4125	Dec  4, 2014 18:27:53.346500000 UTC	17020,9769,14,14,14,14,14,14,14,14
4856	Dec  4, 2014 18:27:59.913634000 UTC	539,432,299,299,299,299,299,299
4285	Dec  4, 2014 18:27:56.318984000 UTC	188,20,20,20,20,20,20,20,20
1219	Dec  4, 2014 18:27:19.043268000 UTC	21390,90,90,90,90,90,90
```

## base64コマンド
-i オプションが便利。
```
$ man base64
...
       -d, --decode
              decode data

       -i, --ignore-garbage
              when decoding, ignore non-alphabet characters

...
```

## file extraction > unzipコマンド（-t オプション）
テスト的に展開する。
```
$ unzip -t base64_decoded.bin
Archive:  base64_decoded.bin
    testing: [Content_Types].xml      OK
    testing: _rels/.rels              OK
    testing: word/_rels/document.xml.rels   OK
    testing: word/document.xml        OK
    testing: word/theme/theme1.xml    OK
    testing: word/media/image6.jpeg   OK
    testing: word/media/image5.jpeg   OK
    testing: word/media/image4.jpeg   OK
    testing: word/media/image3.jpeg   OK
    testing: word/media/image2.jpeg   OK
    testing: word/media/image1.jpeg   OK
    testing: word/media/image7.jpeg   OK
    testing: word/settings.xml        OK
    testing: word/webSettings.xml     OK
    testing: word/stylesWithEffects.xml   OK
    testing: word/styles.xml          OK
    testing: docProps/core.xml        OK
    testing: word/fontTable.xml       OK
    testing: docProps/app.xml         OK
No errors detected in compressed data of base64_decoded.bin.
```

## パケットのデータに含まれる悪意のあるdllなどの抽出 > binwalkコマンド
[https://qiita.com/out_of_stack/items/1e85eded0a7f115b369a](https://qiita.com/out_of_stack/items/1e85eded0a7f115b369a)
eオプションを付けても，含まれているデータが抽出できないことがある。
```
$ binwalk -e data.txt
```
eオプションで無理な時は-ddを使う。
```
$ binwalk --dd=".*" data.txt
```



## unixtime <-> UTC > dateコマンド
unixtimeからUTCは@がポイント
```
$ date
Mon Oct 17 02:09:09 UTC 2022
$
$ date +%s
1665972550
$
$ date -d @1665972550
Mon Oct 17 02:09:10 UTC 2022
```
NG例
```
$ date -d 1665972550
date: invalid date ‘1665972550’
```

## 再起的なファイルの扱い
```
$ zcat ./2019-12-*/*.gz | head -n 1
{"ts":1575910800.040507,"uid":"CwsZLT3JGF0UFIKl49","id.orig_h":"172.16.4.4","id.orig_p":16671,"id.resp_h":"172.16.10.11","id.resp_p":53,"proto":"udp","trans_id":53306,"rtt":0.017709016799926758,"query":"e3759.dscx.akamaiedge.net","qclass":1,"qclass_name":"C_INTERNET","qtype":1,"qtype_name":"A","rcode":0,"rcode_name":"NOERROR","AA":false,"TC":false,"RD":true,"RA":true,"Z":1,"answers":["23.194.116.212"],"TTLs":[20.0],"rejected":false}
```

## json > jqコマンド
[cf. Official Manual](https://stedolan.github.io/jq/manual/#Basicfilters)
### simple
```
$ zcat ./2019-12-*/*.gz | head -n 1 | jq 
{
  "ts": 1575910800.040507,
  "uid": "CwsZLT3JGF0UFIKl49",
  "id.orig_h": "172.16.4.4",
  "id.orig_p": 16671,
  "id.resp_h": "172.16.10.11",
  "id.resp_p": 53,
  "proto": "udp",
  "trans_id": 53306,
  "rtt": 0.017709016799926758,
  "query": "e3759.dscx.akamaiedge.net",
  "qclass": 1,
  "qclass_name": "C_INTERNET",
  "qtype": 1,
  "qtype_name": "A",
  "rcode": 0,
  "rcode_name": "NOERROR",
  "AA": false,
  "TC": false,
  "RD": true,
  "RA": true,
  "Z": 1,
  "answers": [
    "23.194.116.212"
  ],
  "TTLs": [
    20
  ],
  "rejected": false
}
```

### 特定項目の値を変換して表示したい場合
jqコマンドのオプションを使う
```
$ zcat ./2019-12-*/*.gz | head -n 1 | jq '.ts | todate' 
"2019-12-09T17:00:00Z"
```
NG
```
$ echo "@`zcat ./2019-12-*/*.gz | head -n 1 | jq '.ts'`" | date -d
date: option requires an argument -- 'd'
Try 'date --help' for more information.
```
OK。ただし上手いやり方ではない。
```
$ date -d "@`zcat ./2019-12-*/*.gz | head -n 1 | jq '.ts'`"
Mon Dec  9 17:00:00 UTC 2019
```
#### 複数項目表示する際に一部の値を変換する場合
```
zcat 2019-12-*/dns.*.gz | jq -cr 'select((."id.orig_h" == "172.16.7.27" or ."id.orig_h" == "172.16.6.42") and .rcode_name == "NXDOMAIN") | { ts: .ts | todate, "id.orig_h", query }'
```

### -r オプション
"が付くかつかないか。-r はRawの意味なので、値として取り出すため"は無し
```
$ zcat ./2019-12-*/*.gz | head -n 1 | jq -c '.ts | todate' 
"2019-12-09T17:00:00Z"
$
$ zcat ./2019-12-*/*.gz | head -n 1 | jq -cr '.ts | todate' 
2019-12-09T17:00:00Z
```

### -c オプション
１行で出すか、複数行で出すか。-c はCompactの意味なので、つけると１行で出力する。
```
$ zcat ./2019-12-*/*.gz | head -n 1 | jq -cr '.'
{"ts":1575910800.040507,"uid":"CwsZLT3JGF0UFIKl49","id.orig_h":"172.16.4.4","id.orig_p":16671,"id.resp_h":"172.16.10.11","id.resp_p":53,"proto":"udp","trans_id":53306,"rtt":0.017709016799926758,"query":"e3759.dscx.akamaiedge.net","qclass":1,"qclass_name":"C_INTERNET","qtype":1,"qtype_name":"A","rcode":0,"rcode_name":"NOERROR","AA":false,"TC":false,"RD":true,"RA":true,"Z":1,"answers":["23.194.116.212"],"TTLs":[20],"rejected":false}
$
$ zcat ./2019-12-*/*.gz | head -n 1 | jq -r '.'
{
  "ts": 1575910800.040507,
  "uid": "CwsZLT3JGF0UFIKl49",
  "id.orig_h": "172.16.4.4",
  "id.orig_p": 16671,
  "id.resp_h": "172.16.10.11",
  "id.resp_p": 53,
  "proto": "udp",
  "trans_id": 53306,
  "rtt": 0.017709016799926758,
  "query": "e3759.dscx.akamaiedge.net",
  "qclass": 1,
  "qclass_name": "C_INTERNET",
  "qtype": 1,
  "qtype_name": "A",
  "rcode": 0,
  "rcode_name": "NOERROR",
  "AA": false,
  "TC": false,
  "RD": true,
  "RA": true,
  "Z": 1,
  "answers": [
    "23.194.116.212"
  ],
  "TTLs": [
    20
  ],
  "rejected": false
}
```

### 特定の項目だけを出力する
```
$ zcat ./2019-12-*/*.gz | jq -r '.rcode_name' | sort | uniq -c | sort -nr
 605461 NOERROR
   6500 NXDOMAIN
    180 SERVFAIL
     40 null
```

### 複数の項目を出力する
ポイントは{}で囲んで.を使わない。
```
$ zcat 2019-12-*/dns.*.gz | jq -cr '{ qtype_name, rcode_name }'  | sort | uniq -c | sort -nr
 605276 {"qtype_name":"A","rcode_name":"NOERROR"}
   6076 {"qtype_name":"A","rcode_name":"NXDOMAIN"}
    424 {"qtype_name":"PTR","rcode_name":"NXDOMAIN"}
    164 {"qtype_name":"A","rcode_name":"SERVFAIL"}
    130 {"qtype_name":"PTR","rcode_name":"NOERROR"}
     40 {"qtype_name":"A","rcode_name":null}
     33 {"qtype_name":"NS","rcode_name":"NOERROR"}
     22 {"qtype_name":"DNSKEY","rcode_name":"NOERROR"}
     16 {"qtype_name":null,"rcode_name":"SERVFAIL"}
```
NG
```
$ zcat ./2019-12-*/*.gz | head -n 2 | jq -r '.rcode_name' -r '.qtype_name'
jq: error: Could not open file .qtype_name: No such file or directory
```
微妙なやり方
```
$ zcat ./2019-12-*/*.gz | head -n 2 | jq -r '.rcode_name,.qtype_name'
NOERROR
A
NOERROR
A
```
微妙なやり方:-cをつけてもダメ
```
$ zcat ./2019-12-*/*.gz | head -n 2 | jq -cr '.rcode_name,.qtype_name'
NOERROR
A
NOERROR
A
```

### 条件をつける
#### 条件をつけて、一つの項目を出力する。
```
$ zcat 2019-12-*/dns.*.gz | jq -r 'select(.rcode_name == "NOERROR") | .qtype_name' | sort | uniq -c | sort -nr
 605276 A
    130 PTR
     33 NS
     22 DNSKEY
```
#### 条件をつけて、複数の項目を出力する
```
$ zcat 2019-12-*/dns.*.gz | jq -cr 'select(.rcode_name != "NOERROR") | { qtype_name, rcode_name }'  | sort | uniq -c | sort -nr
   6076 {"qtype_name":"A","rcode_name":"NXDOMAIN"}
    424 {"qtype_name":"PTR","rcode_name":"NXDOMAIN"}
    164 {"qtype_name":"A","rcode_name":"SERVFAIL"}
     40 {"qtype_name":"A","rcode_name":null}
     16 {"qtype_name":null,"rcode_name":"SERVFAIL"}
```
微妙
```
$ zcat 2019-12-*/dns.*.gz | jq -r 'select(.rcode_name != "NOERROR") | { qtype_name, rcode_name }' | sort | uniq -c | sort -nr
   6720 }
   6720 {
   6500   "rcode_name": "NXDOMAIN"
   6280   "qtype_name": "A",
    424   "qtype_name": "PTR",
    180   "rcode_name": "SERVFAIL"
     40   "rcode_name": null
     16   "qtype_name": null,
```
#### 複数条件の付け方
```
zcat 2019-12-*/dns.*.gz | jq -cr 'select((."id.orig_h" == "172.16.7.27" or ."id.orig_h" == "172.16.6.42") and .rcode_name == "NXDOMAIN") | {"id.orig_h", query}' | sort | uniq -c | sort -nr
```

# キーワードがわかっているとき
1. 全体検索はpcapにstringsコマンド
2. 特定のパケットに検索はwiresharkのfind