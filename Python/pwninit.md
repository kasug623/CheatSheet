# Example
```zsh
$ ./vuln
    Inconsistency detected by ld.so: dl-call-libc-early-init.c: 37: _dl_call_libc_early_init: Assertion 'sym != NULL' failed!
$
$ ldd ./vuln
    linux-vdso.so.1 (0x00007f88c830f000)
    libc.so.6 => ./libc.so.6 (0x00007f88c7e00000)
    /lib64/ld-linux-x86-64.so.2 (0x00007f88c8311000)
$ strings ./libc.so.6 | grep -i version
$ pwninit --bin ./vuln --libc ./libc.so.6
$ ldd ./vuln_patched 
    linux-vdso.so.1 (0x00007ffeaabf6000)
    libc.so.6 => ./libc.so.6 (0x00007f8fb4600000)
    ./ld-2.27.so => /lib64/ld-linux-x86-64.so.2 (0x00007f8fb4aa3000)
```
- https://qiita.com/Hashibirokou/items/8d76fc88bd6daaf8f067
- https://sh0ebill.hatenablog.com/entry/2022/09/28/215346