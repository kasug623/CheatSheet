```
$ sudo tcpdump -n -i ens33 -s 0 'tcp adb port 80'
$ sudo tcpdump -n -i ens33 -s 0 'tcp and port 80 and not host XXX.XXX.XXX.XXX'
$ sudo tcpdump -n -i ens33 -s 0 -w ./foo.pcap
$ sudo tcpdump -n -r ./foo.pcap -w ./new-foo.pcap 'tcp and port 80 and not host XXX.XXX.XXX.XXX'
```
