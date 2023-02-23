```
$ nfdump -R foo/ -s ip | tail
$ nfdump -R foo/ -s srcip/bytes -n 5 'not src net XXX.XXX.XXX.XXX/16'
$ nfdump -R foo/ -s srcip/bytes -n 5 'not src net XXX.XXX.XXX.XXX/24 or src net XXX.XXX.YYY.XXX/24'
$ nfdump -R foo/ -s srcip/bytes 'dst host XXX.XXX.XXX.XXX'
$ nfdump -R foo/ -s port:p/bytes 'host XXX.XXX.XXX.XXX'
$ nfdump -R foo/ -o long 'host XXX.XXX.XXX.XXX and (host YYY.YYY.YYY.YYY or host ZZZ.ZZZ.ZZZ.ZZZ)'
$ nfdump -R foo/ 'net XXX.XXX.XXX.XXX/24 and net XXX.XXX.YYY.XXX/24'
$ nfdump -R foo/ -A srcip,dstip -O bytes -o 'fmt:%sa %da %byt' 'net XXX.XXX.XXX.XXX/24 and net XXX.XXX.YYY.XXX/24 and port 445'
$ nfdump -R foo/ -O tstart -o 'fmt:%ts %te % sap % dap %byt' -t 'yyyy/MM/dd.hh:mm:ss-yyyy/MM/dd.hh:mm:ss' 'host in [XXX.XXX.XXX.XXX YYY.YYY.YYY.YYY ZZZ.ZZZ.ZZZ] and net AAA.AAA.AAA.AAA/24 and port 445 and bytes > 850000'
$ nfdump -R ./foo/ -O start -N -o 'fmt:%ts %te %fl %td 'jhost XXX.XXX.XXX.XXX and port YYYY'
```

```
$ numfmt --to=si --suffix=B --round=nearest --format=%.2f 1429898902302
```