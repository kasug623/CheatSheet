# run with arguments
```zsh
$ gdb --args ./TestBinary -key abcd
```

#  pwndbg>
```zsh
pwndbg> start
pwndbg> run

pwndbg> b *0x080488a7
pwndbg> info breakpoints

pwndbg> x/170xb 0x7fffffffdc6c
pwndbg> x $ebp - 0x214
0xffffcbe4:     0xfffff641
pwndbg> x 0x7fffffffdb78
0x7fffffffdb78: 0x6261616161616169
pwndbg> x/s 0x7fffffffdb78
0x7fffffffdb78: "iaaaaaabjaaaaaabkaaaaaablaaaaaabmaaa"
pwndbg> x/32gx 0x602000

pwndbg> p/d 0x6034a0 - 0x602088
$1 = 5144
pwndbg> p 2
$2 = 2
pwndbg> p f
No symbol table is loaded.  Use the "file" command.
pwndbg> p/d f
No symbol table is loaded.  Use the "file" command.
pwndbg> p "f"
$3 = "f"
pwndbg> p/d 0xf
$4 = 15
pwndbg> p 0xf
$5 = 15
pwndbg> print 5
$6 = 5
pwndbg> p/x $eax
$7 = 0x3
pwndbg> print $rbp - 8
$8 = (void *) 0x7fffffffdb78

pwndbg> cyclic 200
aaaaaaaabaaaaaaacaaaaaaadaaaaaaaeaaaaaaafaaaaaaagaaaaaaahaaaaaaaiaaaaaaajaaaaaaakaaaaaaalaaaaaaamaaaaaaanaaaaaaaoaaaaaaapaaaaaaaqaaaaaaaraaaaaaasaaaaaaataaaaaaauaaaaaaavaaaaaaawaaaaaaaxaaaaaaayaaaaaaa
pwndbg> cyclic --offset baaaaaaacaaaaaaadaa
Lookup pattern must be 8 bytes (use `-n <length>` to lookup pattern of different length)
pwndbg> cyclic --offset baaaaaaaca
Lookup pattern must be 8 bytes (use `-n <length>` to lookup pattern of different length)
pwndbg> cyclic --offset baaaaaaa
Finding cyclic pattern of 8 bytes: b'baaaaaaa' (hex: 0x6261616161616161)
Found at offset 8
pwndbg> cyclic -l iaaaaaab
Finding cyclic pattern of 8 bytes: b'iaaaaaab' (hex: 0x6961616161616162)
Found at offset 264

pwndbg> vmmap

pwndbg> tcache

pwndbg> bins

pwndbg> hexdump $eax
```