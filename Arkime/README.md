# TOC
- [overview](#overview)
- [setup](#setup)
- [usage](#usage)

# overview
https://arkime.com/

# setup
## point
- elasticsearch's TLS and ID/PW are off.  
because it is easy to build.
- enable systemd on WSL.  
configure /etc/wsl.conf on WSL
- just follow arkime's instuction as much as possible

# usage
## when you start
after initial setup
```
$ ~/arkime/elasticsearch-X.X.X/bin/elasticsearch
#
# ----- start arkime viewer -----
$ sudo systemctl start arikimeviewer
#
## or
$
$ cd /opt/arkime/viewer
$ /opt/arkime/bin/node /opt/arkime/viewer/viewer.js
$
# -------------------------------
#
$ /opt/arkime/bin/capture --copy -r ./foo.pcap -t foo_tag1 -t foo_tag2
```
Then, access to localhost:8005

## when you end
```
$ sudo systemctl start arikimeviewer
```
Then, stop elasticsearch