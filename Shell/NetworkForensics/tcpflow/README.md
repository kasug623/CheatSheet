# overview
`tcpflow` is a tool for extracting all TCP data streams from a pcap file to separate files.

```
$ tcpflow -r ./foo.pcap -o ./tcp_streams/
$ tcpflow -r ./foo.pcap -e http -o ./tcp_stream_http/ 'tcp and port 80'
```