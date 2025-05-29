# Ref
https://qiita.com/tobira-code/items/7ae54a22158aea1a8579

# Python for GDB
python for gbd must be run in GDB.  
However, I prefer to use `pwntools` instead of this.  
```bash
$ gdb
(gdb) python
> a = 1
> print
> b = 2
> end
1
```
- https://stackoverflow.com/questions/4792483/how-to-import-gdb-in-python