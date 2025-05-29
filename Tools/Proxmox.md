# TOC
- [TIPs](#tips)
- [Trouble Shooting](#trouble-shooting)

# TIPs
## Create Windows(Client/Server) VM
- [Must prepare VirtIO drivers 1](https://akam1o.hatenablog.jp/entry/2024/03/05/234501)
- [Must prepare VirtIO drivers 2](https://subro.mokuren.ne.jp/1872.html)
- [Window 11 no network problem](https://www.intel.co.jp/content/www/jp/ja/support/articles/000092599/intel-nuc.html)
- [Desktop Experience](https://ameblo.jp/mizuhokuzuhara/entry-12536887629.html)
- [No NIC after VM is created](https://zenn.dev/northeggman/articles/49c6b73c03c81c#virtio%E3%83%89%E3%83%A9%E3%82%A4%E3%83%90%E3%83%BC%E3%81%AEiso%E3%83%80%E3%82%A6%E3%83%B3%E3%83%AD%E3%83%BC%E3%83%89)
    ```
    D drive(virtio-win-0.1.215) > NetKVM> w11 > amd64
    ```

# Trouble Shooting
## when host name is changed  
- don't forget to modify /etc/hosts
- https://qiita.com/honahuku/items/7335158659cfb1b4580f
## when you cannot access web console  
The IP with CIDR on this red line means "web console IP".  
When you change this, you might have to change it back from a console on host.  
![png](https://i.imgur.com/CMknoVV.png)

