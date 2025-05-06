# Example
```zsh
$ ROPgadget --binary ./vuln --re "mov dword ptr" | grep ret
$ ROPgadget --binary ./vuln --re "rdi" | grep "mov dword"
$ ROPgadget --binary ./vuln --re "pop rdi"
$ ROPgadget --binary ./vuln_patched
```