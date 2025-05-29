#dd #posix #Linux

## Example
```zsh
$ binwalk ./test.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
32796         0x801C          Libpcap capture file, little-endian, version 2.4, Ethernet, snaplen: 262144
41546         0xA24A          PNG image, 640 x 400, 8-bit/color RGBA, non-interlaced

$
$ dd if=test.jpg bs=1 skip=32796 of=embedded.pcap
```
|Option|Meaning|
|-|-|
|`if=cat2.jpg`|Input file|
|`bs=1`|Read and write data 1 byte at a time|
|`skip=32796`|Skip the first 32,796 bytes of the input file before starting to copy|
|`of=embedded.pcap`|Output file|
