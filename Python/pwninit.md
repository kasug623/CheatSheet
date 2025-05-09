# Example
```zsh
$ ls -la
...
vuln
libc-2.19.so-hogehogehoge
...
$
$ ldd ./vuln
        linux-gate.so.1 (0xf7ef8000)
        libc.so.6 => /lib/i386-linux-gnu/libc.so.6 (0xf7ca7000)
        /lib/ld-linux.so.2 (0xf7efa000)
$
$ pwninit --bin ./vuln --libc ./libc-2.19.so-hogehogehoge
bin: ./vuln
libc: ./libc-2.19.so-hogehogehoge

fetching linker
https://launchpad.net/ubuntu/+archive/primary/+files//libc6_2.19-0ubuntu6.9_i386.deb
unstripping libc
https://launchpad.net/ubuntu/+archive/primary/+files//libc6-dbg_2.19-0ubuntu6.9_i386.deb
warning: failed unstripping libc: failed running eu-unstrip, please install elfutils: No such file or directory (os error 2)
symlinking ./libc.so.6 -> libc-2.19.so-hogehogehoge
copying ./vuln to ./vuln_patched
running patchelf on ./vuln_patched
writing solve.py stub
$
$ ls -la
...
vuln
vuln_patched
ld-2.19.so
libc-2.19.so-hogehogehoge
libc.so.6 -> libc-2.19.so-hogehogehoge
...
$
$ ldd ./vuln_patched
        linux-gate.so.1 (0xf7ee2000)
        libc.so.6 => ./libc.so.6 (0xf7d2d000)
        ./ld-2.19.so => /lib/ld-linux.so.2 (0xf7ee4000)
```
- https://qiita.com/Hashibirokou/items/8d76fc88bd6daaf8f067
- https://sh0ebill.hatenablog.com/entry/2022/09/28/215346