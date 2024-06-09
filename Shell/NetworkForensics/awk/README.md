```
$ grep \.jpg ./access.log | awk '{printf "%s", strftime("%Y-%m-%d_%H:%M:%S UTC:"), $1=""; print $0}
```