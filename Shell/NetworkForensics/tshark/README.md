```
$ tshark -n -r ./foo.pcap -Y 'http.server caontains "foo"' -T fields -e ip.src -e http.server | sort | uniq -c | sort -nr
$ tshark -n -r ./foo.pcap -Y 'http.host contains "pastesite.com" and http.request.method == "POST"'
$ tshark -n -r ./foo.pcap -Y 'frame.number==XXXXX' -T fields -e http.file_data | sed -e 's/xxxx/yyyy'
```