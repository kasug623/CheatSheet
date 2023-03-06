# TOC
- [overview](#overview)
- [setup](#setup)
- [usage](#usage)

# overview
https://arkime.com/


# setup
## point
- elasticsearch's TLS and ID/PW are off.
because it is easy.
- enable systemd on WSL
configure wsl conf for WSL
- just follow arkime's instuction as much as possible

# usage
after initial setup
```
$ ~/arkime/elasticsearchXXXX/bin/elasticsearch
$ systemctl start arikimeviewer
$ /opt/arkime/bin/capture --copy -r ./foo.pcap
```
Then, access to localhost:8005