```
$ tshark -n -r ./foo.pcap -Y 'http.server caontains "foo"' -T fields -e ip.src -e http.server | sort | uniq -c | sort -nr
$ tshark -n -r ./foo.pcap -Y 'http.host contains "pastesite.com" and http.request.method == "POST"'
$ tshark -n -r ./foo.pcap -Y 'frame.number==XXXXX' -T fields -e http.file_data | sed -e 's/xxxx/yyyy'
$ tshark -n -C no_desegment_tcp -r ./foo.pcap -T fields -e frame.time -e http.request.method -e http.host -e http.request.uri -e http.user_agent -Y 'http.request' > foo_output.log
$ tshark -n -C no_desegment_tcp -r ./foo.pcap -q -z io,phs
$ tshark -n -C no_desegment_tcp -r ./foo.pcap -T fields -e frame.time_delta_displayed -e frame.time -e http.request.uri -Y 'http.user_agent contains "XXX" and http.request.uri contains "YYY"'
$ tshark -n -C no_desegment_tcp -r ./foo.pcap -T fields -e http.host -Y 'http.cookie contains "utm"' | sort | uniq -c | sort -nr
```